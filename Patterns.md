Most Ludii games are abstract strategy, played by rearranging nodes on some sort of graph. 
The easiest Ludeme to understand in this context is the Line. The code snippet

  (is Line 3)
  
is a boolean that is true if any three of the moving player's stones/pieces/nodes/units are 
arranged in a 3-long line, and it is true on a square, triangular, hexagonal, 3.4.3.3.4, or 
arbitrary user-specified graph.

There exists a generalization of this ludeme called the "Pattern". They are created by 
making a list of some arbitrary combination of three basic movements: Forward (F), Right, 
(R), and (L). The most common usage is to detect the formation of a square, which looks like this.

  (is Pattern {F R F R F})
  
See, you start at some arbitrary spot, go forward one (now you've been to two blocks total), 
turn right then go forward one, (three now), then turn right for the second time, now facing
down, and inch forward one to complete the formation. If I switched the tiling, without changing 
that line, the new win formations would be:

  |TRIANGULAR       SQUARE        HEXAGONAL
  |    O              OO             OO
  |   O O             OO            O  O
  
I first got really into abstract strategy games through Wikipedia. I've read many, many articles 
on the topic, and as such I am confident when I say that Teeko has the funniest one. It's a very 
simple game, one of the ones I'll play with small children or adults who don't like to think. You
lay eight nodes on a 5x5 square grid, then move them like Chess kings until you've got a 4-line or a 
square. The game's inventor was exceedingly proud of his creation, with the wikipedia article stating 

    - The rules, as summarized above, are very simple, but the strategy is complicated enough 
      to fill a book, <ins>Scarne on Teeko</ins>, by Scarne (1955).
