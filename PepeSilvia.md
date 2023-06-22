# STATUS UPDATE

- The "deal" ludeme is super scarce. The "dealable types" are Cards and Dominos, although I've found 
some weird limitations on the former and no success on the latter. Making it work as intended for these
types is one matter, the other is generalizing it to dealing Pieces of distinct indices. Below is a stupid
game, a Tic-Tac-Toe variant with cards instead of Os and Xs, and no legitimate win condition. It does 
not work, despite being mostly copied verbatim from the reference.

	(game "DECK" 
		(players 2) 
		(equipment { 
			(board (square 10)) 
			(deck)
			(hand Each size:6)
		}) 
		(rules
			(start {
				(deal Cards 6) 
			})
			(play (move (from (sites Hand Mover)) (to (sites Empty))))
			(end (if (is Loop) (result Mover Win)))
		)
	)
  
Below, the ludeme works fine.

	(game "DOMINO" 
		(players 2) 
		(equipment { 
			(board (square 10)) 
			(dominoes)
			(hand Each size:6)
		}) 
		(rules
			(start {
				(deal Dominoes 6) 
			})
			(play (move (from (sites Hand Mover)) (to (sites Empty))))
			(end (if (is Loop) (result Mover Win)))
		)
	)
  
Jamaican-Style Dominoes requires four players, which is why the fact that the below code does not work
makes me sad.

	(game "DOMINO" 
		(players 4) 
		(equipment { 
			(board (square 10)) 
			(dominoes)
			(hand Each size:6)
		}) 
		(rules
			(start {
				(deal Dominoes 6) 
			})
			(play (move (from (sites Hand Mover)) (to (sites Empty))))
			(end (if (is Loop) (result Mover Win)))
		)
	)
  
There are 28 dominoes. Nevertheless, I am only able to deal half that at the beginning. Running this gives
three dominoes per player, while (deal Dominoes 12) delivers the reasonable error message "You can not deal 
so much dominoes in the initial state." 

My goal is to make this ludeme work as intended for cards and dominoes, then have it do something similar for
general pieces. At present, these are the only two types accepted by the Java class that defines the deal.

- A very odd thing that I have noticed about Ludii is that stacks don't really exist. They can be formed, 
but not really observed or manipulated along any more particular lines than general board regions. In Ludii,
an object can be one of two things, and then one of x subthings, like so:

  1. COMPONENT (a thing)
    1a. PIECE
    1b. TILE
    1c. CARD
    1d. DIE THAT CANNOT BE ROLLED
  2. CONTAINER (a place)
    2a. BOARD
    2b. HAND
    2c. DECK
    2d. SET OF DICE THAT CAN BE ROLLED
  
The stack exists nowhere in the slate of primordial objects. The first time that they are mentioned is 
in the (start) phase, when a set of pieces may be placed as a stack. I got the Knuth Shuffle algorithm 
(sort of) to work on a stack, but, in Ludii, (this was an odd random setback) you are not allowed to hide
a stack. I'm poking around with the Java, experimenting, although the designer stated that Ludii does not
"have games with stack + hidden info (since I disabled the card games in Ludii because that was not enough 
efficient to describe these games)." 

I am still working and what follows is speculation that I will update as further information arises. In 
Deal.java, which has become my little house today, the function sizeStackCell(indexSiteDeck); is called 
on the inputted Deck. This leads me to believe that the current double-for loop that generates cards puts
them in a stack. This is odd because a deck is initiated in the equipment phase, but is supported by the 
fact that hiding cards gives the same error as setting a stack hidden. This leads me to scan back over that
list of containers, and wonder if perhaps a 1x52 board is what we need. I'd then note that we can't really hide
our randomly placed markers. Considering that Dr. Piette said that stacks are inefficient to describe card games 
I think that the issue is that doing so creates too much empty space, by stretching the z-axis a lot. A 2d-offshoot 
to the board *might* work better, but I dunno. A sufficient solution would be the ability to hide a variable array 
list that we can Knuth-shuffle and divvy, I keep that in mind.

This is exceedingly important. The whole point of general game engines is to create well-rounded
and well-documented artificially intelligent agents, capable of strategy. 

Look, John Nash was an economist who solved Hex and demonstrated that a Nash Equilibrium exists for any 
finite game (which can be represented as a grid of numbers, or matrix, that approaches a “steady state” 
on sufficiently many iterations like with a Markov Chain). A matrix that represents every possible chess
position would surely be larger than the atoms in the universe (even the big ones), but it's not hard to
convince yourself that one could exist.

This means, basically, that any game humans play has an unbeatable strategy for the first, second, xor 
nth player, or always draws.

In abstract strategy games, no hidden information, nothing random, this is a "PURE STRATEGY". This means
that the theoretical optimal algorithm will give the same answer every time for the same position. This 
isn't the case for Ludii or most general game systems of course - Monte Carlo Tree Searches *can* determine 
the perfect move, but time is not infinite, and so we don't let them, only sampling some possible moves from 
a given position, randomly.

In games with random elements, the equilibrium is called a mixed strategy. This is a probability distribution 
that chooses between pure strategies randomly. This means that if you had an algorithm on your computer that
did the best job possible at playing poker, and you put it in the same situation twice, it might do two different
things. This super fundamental distinction is something that the current Ludii AIs cannot do. In a Ludii game
with hidden information, the AI can still see whatever has been set hidden. The ability to create a functional
mixed strategy, with a probability density function forged by gradient descent, is vital to card games.

Another crucial element to card-game strategy that must exist to challenge even weak human players in card games
is a memory of what cards have been played. If one presents a chess position to you, you do not need to know in 
what order the pieces arrived to their current positions to determine the optimal play. If one presents a Poker
hand, however, knowledge of what other players have played so far would be crucial to knowing how much to bet,
whether to fold, and anything consequential.

So in conclusion the main thing I'm trying to do in the present is get "deal" to work better by fiddling with the
source code. I'll likely be working in Ludii beyond this summer, and in that regard my goals are to successfully 
hide information from the AIs (this is like the Bowser of this), and come up with a method to store "what has 
happened so far" in a place where they can see it. My temptation is to leave all played cards on the board, so a 
finished game of Hearts would look a bit like Mastermind. I've also got a big backlog of games that I'll likely continue 
to work on, some abstract strategy, and some card-based. My version of "Agram" works except for no hidden information, 
and my versions of Hearts 98 and so on will be similar, at least until I figure out how to make hiding 
work. 

-----------------------------------------------------------------------------------------------------------------

P.S. A "permute" unary IntArrayList option would be beneficial, I've got a nice post building up to it 
among the other set operations on my other blog, but we'd need an option to set a variable hidden. 
Currently, only in-game objects can be set hidden, not random numbers or locations. 

* Variables can only be integers. Lists of integers could be useful, although I am not positive this could 
accomplish anything remember values couldn't. I'm still toying with that, though this can't be hidden. Neither
can variables, at present.
