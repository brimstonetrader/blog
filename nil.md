At the beginning of my delving into the Ludii engine, Dr. Piette informed me that the AIs, in games with 
Hidden Information, "cheat". I've documented on this blog some places in the source code where this is 
allowed to happen, but I've recently implemented a very simple test that verifies this.

The first test, the one I've been using thus far, is the Ludii implementation of The Monty Hall Problem,
which is as follows.

		(game "Monty Hall Problem"
			(players 1)
			(equipment {
				(board (rectangle 1 3))
				(piece "Car" Shared)
				(piece "Goat" Shared)
			})
			
			(rules
				(start {
					(place Random {"Car"})
					(place Random {"Goat"} count:2)
					(set Hidden What (sites Board) to:P1)
				})
				
				phases:{
				(phase "FirstChoice"
					(play 
						(move Select 
							(from (sites Board))
							(then
								(set Hidden What 
									(sites Random 
										(forEach 
											(sites Board) 
											if:(and 
												(!= (site) (last To))
												(!= (id "Car" Shared) (what at:(site)))
											)
										)
										num:1
									) 
									False 
									to:P1
								)
							)
						)
					)
					(nextPhase "FinalChoice")
				)
				
				(phase "FinalChoice"
					(play 
						(move Select 
							(from (sites Board)) 
							(then 
								(set Hidden What (sites Board) False to:P1)
							)
						)
					)
					(end {
						(if 
							("IsPieceAt" "Car" Shared (last To))
							(result P1 Win) 
						)
						(if 
							("IsPieceAt" "Goat" Shared (last To))
							(result P1 Loss) 
						)
					})
				)
				}
			)
		)

If one is choosing their actions at random, there is a 1/3 chance they choose the car first, and 2/3 
the goat. For each, there's a 1/2 chance of switching. This makes 6 equally likely outcomes, of which 
three give the random player a car, and three a goat. If one strategizes optimally, always switching,
this 3/6 probability gets boosted to 2/3*. If one is able to see behind the curtain, of course, the 
odds of winning are 100%, and this demonstrates that all Ludii AIs cheat well.

My method of proof is a bit quicker. The Game of Nil is played on a 2x2 board, where each player only 
is able to see one column. Each lays down a square on either the top or bottom cell in their row. If 
the two form a diagonal, P1 wins. If not, P2. Random play, and optimal play, have a 50-50 spread of
wins between the two players. With perfect information, though, it is a guaranteed P2 Win. Thus, if the
AIs cheat, P1 passes the first turn, which happens here. As I've begun to write a mixed-strategy-capable
AI in the Ludii system, these two tests have helped and will continue to help me gauge progress.

     (game "Nil"
         (players 2)
         (equipment {
             (board (rectangle 2 2) use:Vertex)
             (piece "Square" Each)
             (hand Each size:1)
			 (regions P1 (sites {0 2}))
			 (regions P2 (sites {1 3}))
         })
         (rules 
            
              (start {
                 (place "Square1" (handSite P1   ))
                 (place "Square2" (handSite P2   ))
                 (set Hidden (sites Hand P1) to:P2)
                 (set Hidden (sites Hand P2) to:P1)       
                 (set Hidden (sites {0 2}) to:P2)       
                 (set Hidden (sites {1 3}) to:P1)       
              }      )
                phases:{
             (phase "Lead" 
                 (play 
                     (move 
                         (from (sites Hand Mover))
                         (to (sites moves)))
                 ) (nextPhase "Follow")
             )      
                (phase "Follow" 
                    (play 
                    (priority
                     (move  
                     (from (sites Hand Mover))
                     (to (sites {1 3}))) (pass)))
                     (end {(if (or (and (is Empty 0) (is Empty 1)) 
					               (and (is Empty 2) (is Empty 3))) (result P2 Win))
                           (if (or (and (is Empty 0) (is Empty 3)) 
						           (and (is Empty 1) (is Empty 2))) (result P1 Win))}) 
                    (nextPhase "Lead")
                )
                
                    }
            ))
                
            
            
            //------------------------------------------------------------------------------
            
        (metadata 
            

            
            (ai
                "Nil_ai"
            )
        )




*I remember there was a few years in between my uncle's meth benders where he was normal, and working.
At one point in his life, he gambled for a living, and in doing so wielded my family's strong logical
framework. I was sitting with him one day, and I asked him the Monty Hall Problem, which he'd never 
heard before. Within a minute, he'd worked out that switching was always optimal, and why. I wish he
hadn't gotten back on the ice.
