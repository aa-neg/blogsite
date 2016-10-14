---
layout: post
title:  "Simulating 'sevens card game'"
date:   2016-10-14 08:40:09 +1100
categories: developer, knowledge, sharing, sevens, simulation
---

At work I have been introduced to a new card game 'sevens'. Although this is probably not its real name and possibly not even the official rules however these existing rule set create quite an interesting game. The game has two rounds bidding and then playing and involves 'tricks' such as Euchre. 

<h4>The rules:</h4>

Now the rules are not simple however I find it very similar to poker where the rules seem complicated at first but are actually simple once understood. The game is also quite easy if you have played a previous card game involving 'tricks'.

<h5>Number of rounds</h5>

The game goes for 10 rounds. For the first seven rounds each player is dealt the value of the round in cards and a trick is then drawn and placed face up. For the eighth round the game is played without any tricks. The nineth round the trick is placed facedown. Finially for the tenth round both the trick and the players hand are facedown (unkown to the player) during the bidding round.

<h5>Bidding round</h5>
After the hands have been dealt the player to the left of the dealer will be first to bid (and consequencly first to start the hand) on the amount of tricks that player will win during the round. The caveat to this is that the dealer (final bidder) can not place his bid such that the sum total of the bids is equal to the amount of cards in his hand.

For example consider a game with 3 players. In the second round (2 cards) the first player has bid 0 and the second player has bid 1. The total number of bids in this case is 1 (0 + 1). PLayer 3 (the dealer) can not bid 1 as this would make the total equal to his number of cards (1 + 1 = number of cards = 2). The player can bid 2 or 0.

<h5>Playing round</h5>
So the player that leads the bidding will have first turn. The rules are as follows:

* He can place any card down.(lowering the amount of cards in his hand)
* All following players must 'follow suite' if possible (play a card in his hands with the same suit). 
* The winner of the round if the one with the highest value card of the suite that is played or the highest value card of the trick. 
Note: If you have a trick suite in your hand and another card of the same suite that was lead you can not play your trick and are forced to play the card hence the 'follow suite'.

<h5>Gathering points</h5>
After all cards have been played you gain 10 points if you bidded the correct amount of tricks you won and 2 points for each trick. 

<h4>Simulating this game</h4>

More details will probably following in another post but I found this game very interesting. All source code will be on github https://github.com/aa-neg/card_game_sevens .

The very first step will be to write a generalized version of the game however much simpler (possibly with only 3 rounds). I should then create 'AI' bots which will just perform random valid moves.

Next would be to write some kind of function that would evaluate a 'winning' game strategy against these random AI bots.

There are many ways to move forward with this and I think this will turn out to be a great little side project to work on.
