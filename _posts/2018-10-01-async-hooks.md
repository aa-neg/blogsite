---
layout: post
title:  "Async hooks"
date: 2018-10-01 08:40:09 +1100
categories: nodejs, async, hooks 
---

With nodejs gaining popularity for dealing with microservices the simple conversation comes up on how to do tracing across the various services. Now there is an explicit model where the trace token is passed throughout however this is rather messy and is a very poor separation of concern. In traditional web frameworks where each request is served by a thread an implicit model could be used where the trace token can be set inside the thread local state and passed throughout. 

Node however does provide [async hooks](https://nodejs.org/api/async_ooks.html) and we have used this concept to perform the same implicit trace token implementation. Now this is experimental but essentially it allows access to the call tree allowing you to figure out who called this function and walk your way up the tree to determine which request this should belong to. 

Below is some sample code demonstrating this concept:

Here we can create a middleware which will be our reference along our tree.

```javascript
const CONTEXT_HOOKS = 'context-hooks'
const X_REQ_ID = 'x-request-id'

const namespace = cls.createNamespace(CONTEXT_HOOKS)

export const setupContextHooks = (req: Request, res: Response, next): void => {
  namespace.bindEmitter(req)
  namespace.bindEmitter(res)
  namespace.run(() => {
    let xRequestId = req.headers['x-request-id']
      ? req.headers['x-request-id']
      : randomBytes(16)
    if (xRequestId) {
      setContextVal(xRequestId, req.id)
    }
    next()
  })
}

```

We can then set this context hook and fetch the id each time we need to propagate the trace token.

```javascript
export const setContextVal = (
  xRequestId: string,
  expressReqId?: string
): void => {
  const namespace = cls.getNamespace(CONTEXT_HOOKS)
  if (namespace) {
    namespace.set(X_REQ_ID, {
      'x-request-id': xRequestId,
      'express-req-id': expressReqId
    })
  }
}

export const getContextVal = () => {
  const namespace = cls.getNamespace(CONTEXT_HOOKS)
  return namespace && namespace.get(X_REQ_ID) ? namespace.get(X_REQ_ID) : {}
}

export const contextHooksToHeaders = () => {
  const ctxVal = getContextVal()
  if (ctxVal['x-request-id']) {
    return {
      'x-request-id': ctxVal['x-request-id']
    }
  }
  return {}
}
```

Now each time communicate with other services we can easily propagate the trace token without worrying where we came from:

```javascript
return request({
  method,
  uri,
  headers: Object.assign(contextHooksToHeaders(), {
    Authorization: `Bearer ${SECRET_TOKEN}`
  }),
  followRedirect: false,
  json: true
})
```
