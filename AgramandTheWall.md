The 4 player version of Agram has shuffled cards, via "place Random Stack", but this means it can not 
have any sort of hidden information. This is because "set Hidden" does what it does to a component or
region, but a stack is neither. In Ludii, you've got Components, which can be a Card, Die*, Piece, or 
Tile, and Containers, which can be a Board, Deck, Dice, or Hand. Stacks can be created, but they are not
a Type in Ludii. This is something which could be worked on and fixed. I have not found evidence of 
the "Deck" Ludeme being used successfully, but it conceptually acts as an off-board stack. An on-board
stack would likely need to be classified as a Container. Anyway,

        (define "Switch"
        	(if (and {(> (score P1) (score P2)) (> (score P1) (score P4)) (> (score P1) (score P3))})
        			(set NextPlayer (player 1)) 
        	    	(if (and (> (score P2) (score P3)) (> (score P2) (score P4)))
        		    	(set NextPlayer (player 2)) 
        		        (if (> (score P3) (score P4))
        					(set NextPlayer (player 3))
        					(set NextPlayer (player 4)))))
        )
        
        // https://www.ludii.games/forums/showthread.php?tid=1109&pid=4114#pid4114
        //------------------------------------------------------------------------------		
        (game "Agram" 
        	(players 4) 
        	(equipment { 
        		(board (rectangle 1 5) largeStack:True) 
        		(piece "Square1"   Shared)
        		(piece "Square2"   Shared)
        		(piece "Square3"   Shared)
        		(piece "Square4"   Shared)
        		(piece "Square5"   Shared)
        		(piece "Square6"   Shared)
        		(piece "Square7"   Shared)
        		(piece "Square8"   Shared)
        		(piece "Square9"   Shared)
        		(piece "Square10"  Shared)
        		(piece "Square11"  Shared)
        		(piece "Square12"  Shared)
        		(piece "Square13"  Shared)
        		(piece "Square14"  Shared)
        		(piece "Square15"  Shared)
        		(piece "Square16"  Shared)
        		(piece "Square17"  Shared)
        		(piece "Square18"  Shared)
        		(piece "Square19"  Shared)
        		(piece "Square20"  Shared)
        		(piece "Square21"  Shared)
        		(piece "Square22"  Shared)
        		(piece "Square23"  Shared)
        		(piece "Square24"  Shared)
        		(piece "Square25"  Shared)
        		(piece "Square26"  Shared)
        		(piece "Square27"  Shared)
        		(piece "Square28"  Shared)
        		(piece "Square29"  Shared)
        		(piece "Square30"  Shared)
        		(piece "Square31"  Shared)
        		(piece "Square32"  Shared)
        		(piece "Square33"  Shared)
        		(piece "Square34"  Shared)
        		(piece "Square35"  Shared)
        		(hand Each size:6) 
        		(map "Loc" {(pair P1 "B1") (pair P2 "C1") (pair P3 "D1") (pair P4 "E1")})})   
        
        
        
        			(rules
        				(start {
        				(place Random {"Square1"  "Square2"  "Square3"  "Square4"  "Square5" 		                    
        							   "Square6"  "Square7"  "Square8"  "Square9"  "Square10"		                    
        							   "Square11" "Square12" "Square13" "Square14" "Square15"		                    
        							   "Square16" "Square17" "Square18" "Square19" "Square20"
        							   "Square21" "Square22" "Square23" "Square24" "Square25"		           
        							   "Square26" "Square27" "Square28" "Square29" "Square30"		  
        							   "Square31" "Square32" "Square33" "Square34" "Square35"} (coord:"A1"))
        
        				}) 
        				phases:{
                        
        				(phase "Deal"
        					(play (move Pass (then 
        							(forEach Value min:1 max:6 (fromTo (from (coord:"A1") 
        																level:(value Random (range 1 (- 35 (+ (value) (* 6 (- (count Moves) 1))))))
        															   ) 
        									                           (to  (handSite Mover (- (value) 1)))
        															   ) (then (set Var 1))))))
        						(nextPhase (= (count Moves) 4) "Lead"))
        
        			    (phase "Lead" 
        					(play 
        						(move  
        							(from (sites Hand Mover))
        							(to (mapEntry "Loc" (mover)))
        							(then (and {(set Var (% (what at:(mapEntry "Loc" Mover) level:(topLevel at:(mapEntry "Loc" Mover))) 4)) 
        										(set Score Mover (+ (/ (what at:(mapEntry "Loc" Mover) level:(topLevel at:(mapEntry "Loc" Mover))) 4) 1))})))
        					) (nextPhase "Follow")
        				)				
        
        				(phase "Follow" 
        					(play 
        						(priority {
        								(move
        									(from (sites Hand Mover))
        									(to (mapEntry "Loc" (mover)) if:(= (% (what at:(from)) 4) (var))) (then
        									(set Score Mover (+ (/ (what at:(mapEntry "Loc" Mover) level:(topLevel at:(mapEntry "Loc" Mover))) 4) 1))))
        
        								(move  
        									(from (sites Hand Mover))
        									(to (mapEntry "Loc" (mover)))
        									(then (set Score Mover 0)))}) 
        
        					) (nextPhase (= (% (+ (count Moves) 1) 5) 4) "Trick")
        				)
        
        				(phase "Trick"
        					(play (move Pass (then "Switch")))
        					(end {(if ("HandEmpty" Next) (byScore))})
        					(nextPhase "Lead")
        				)
        
        
        			}
        		)
        	)
        
        
        
        		(metadata    
        			(graphics {
        				(piece Foreground "Square1"  image:"0"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
        				(piece Foreground "Square2"  image:"0"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
        				(piece Foreground "Square3"  image:"0"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
        				(piece Foreground "Square4"  image:"0"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
        
        				(piece Foreground "Square5"  image:"1"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
        				(piece Foreground "Square6"  image:"1"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
        				(piece Foreground "Square7"  image:"1"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
        				(piece Foreground "Square8"  image:"1"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
        
        				(piece Foreground "Square9"  image:"2"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
        				(piece Foreground "Square10" image:"2"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
        				(piece Foreground "Square11" image:"2"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
        				(piece Foreground "Square12" image:"2"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
        
        				(piece Foreground "Square13" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
        				(piece Foreground "Square14" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
        				(piece Foreground "Square15" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
        				(piece Foreground "Square16" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
        
        				(piece Foreground "Square17" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
        				(piece Foreground "Square18" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
        				(piece Foreground "Square19" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
        				(piece Foreground "Square20" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
        
        				(piece Foreground "Square21" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
        				(piece Foreground "Square22" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
        				(piece Foreground "Square23" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
        				(piece Foreground "Square24" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
        
        				(piece Foreground "Square25" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
        				(piece Foreground "Square26" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
        				(piece Foreground "Square27" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
        				(piece Foreground "Square28" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
        
        				(piece Foreground "Square29" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
        				(piece Foreground "Square30" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
        				(piece Foreground "Square31" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
        				(piece Foreground "Square32" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
        
        				(piece Foreground "Square33" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
        				(piece Foreground "Square34" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
        				(piece Foreground "Square35" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
        				(show Edges Hidden)                                                  
        				(show Symbol "Hexagon" (sites Board) fillColour:(colour 185 130 85) edgeColour:(colour 225 182 130)) 
        				(board Placement scale:0.8)
        				(stackType None)
        				(board Background image:"square.svg" fillColour:(colour 185 130 85) edgeColour:(colour 185 130 85) scale:1.45)
        			})
        		)
            
              (define "Switch"
        	(and {
        		(if (and {(> (score P1) (score P2)) (> (score P1) (score P4)) (> (score P1) (score P3))})
        				(set NextPlayer (player 1)) 
        				(if (and (> (score P2) (score P3)) (> (score P2) (score P4)))
        					(set NextPlayer (player 2)) 
        					(if (> (score P3) (score P4))
        						(set NextPlayer (player 3))
        						(set NextPlayer (player 4)))))
        						})
        )
        
        //------------------------------------------------------------------------------		
        (game "Agram" 
        	(players 4) 
        	(equipment { 
        		(board (rectangle 1 5) largeStack:True) 
        		(piece "Square"   Shared)
        		(piece "Square1"   Shared)
        		(piece "Square2"   Shared)
        		(piece "Square3"   Shared)
        		(piece "Square4"   Shared)
        		(piece "Square5"   Shared)
        		(piece "Square6"   Shared)
        		(piece "Square7"   Shared)
        		(piece "Square8"   Shared)
        		(piece "Square9"   Shared)
        		(piece "Square10"  Shared)
        		(piece "Square11"  Shared)
        		(piece "Square12"  Shared)
        		(piece "Square13"  Shared)
        		(piece "Square14"  Shared)
        		(piece "Square15"  Shared)
        		(piece "Square16"  Shared)
        		(piece "Square17"  Shared)
        		(piece "Square18"  Shared)
        		(piece "Square19"  Shared)
        		(piece "Square20"  Shared)
        		(piece "Square21"  Shared)
        		(piece "Square22"  Shared)
        		(piece "Square23"  Shared)
        		(piece "Square24"  Shared)
        		(piece "Square25"  Shared)
        		(piece "Square26"  Shared)
        		(piece "Square27"  Shared)
        		(piece "Square28"  Shared)
        		(piece "Square29"  Shared)
        		(piece "Square30"  Shared)
        		(piece "Square31"  Shared)
        		(piece "Square32"  Shared)
        		(piece "Square33"  Shared)
        		(piece "Square34"  Shared)
        		(piece "Square35"  Shared)
        		(piece "Square36"  Shared)
        		(piece "Square37"  Shared)
        		(piece "Square38"  Shared)
        		(piece "Square39"  Shared)
        		(piece "Square40"  Shared)
        		(piece "Square41"  Shared)
        		(piece "Square42"  Shared)
        		(piece "Square43"  Shared)
        		(piece "Square44"  Shared)
        		(piece "Square45"  Shared)
        		(piece "Square46"  Shared)
        		(piece "Square47"  Shared)
        		(piece "Square48"  Shared)
        		(piece "Square49"  Shared)
        		(piece "Square50"  Shared)
        		(piece "Square51"  Shared)
        		(piece "Square52"  Shared)
        		(hand Each size:13) 
        		(map "Loc" {(pair P1 "B1") (pair P2 "C1") (pair P3 "D1") (pair P4 "E1")})})   
        
        
        
        			(rules
        				(start {
        				(place Stack items:{"Square" "Square1"  "Square2"  "Square3"  "Square4"  "Square5" 		                    
        									         "Square6"  "Square7"  "Square8"  "Square9"  "Square10"		                    
        									         "Square11" "Square12" "Square13" "Square14" "Square15"		                    
        									         "Square16" "Square17" "Square18" "Square19" "Square20"
        									         "Square21" "Square22" "Square23" "Square24" "Square25"		           
        									         "Square26" "Square27" "Square28" "Square29" "Square30"		  
        									         "Square31" "Square32" "Square33" "Square34" "Square35"		           
        									         "Square36" "Square37" "Square38" "Square39" "Square40"		  
        									         "Square41" "Square42" "Square43" "Square44" "Square45"		           
        									         "Square46" "Square47" "Square48" "Square49" "Square50"		  
        									         "Square51" "Square52" "Square"} (coord:"A1"))
        				}) 
        				phases:{
                        
        				(phase "Deal"
        					(play (move Pass (then  
        							(forEach Value min:1 max:13 (fromTo (from (coord:"A1") level:(value Random (range 1 (- 66 (+ (value) (* 13 (count Moves))))))) 
        									                            (to  (handSite Mover (- (value) 1))) 
        									                           )))))
        					(nextPhase (= (count Moves) 4) "Lead"))
        
        			    (phase "Lead" 
        					(play 
        						(move  
        							(from (sites Hand Mover))
        							(to (mapEntry "Loc" (mover)))
        							(then (and {(set Var (% (what at:(mapEntry "Loc" Mover) level:(topLevel at:(mapEntry "Loc" Mover))) 4)) 
        										(if (= (% (var) 4) 1) (set Var "HeartsBroken" 1))
        										(set Score Mover (+ (/ (what at:(mapEntry "Loc" Mover) level:(topLevel at:(mapEntry "Loc" Mover))) 4) 1))})))
        					) (nextPhase "Follow")
        				)				
        
        				(phase "Follow" 
        					(play 
        						(if (= (var "Hearts") 1) 
        							(priority {
        								(move
        									(from (sites Hand Mover))
        									(to (mapEntry "Loc" (mover))) (then
        									(set Score Mover (+ (/ (what at:(mapEntry "Loc" Mover) level:(topLevel at:(mapEntry "Loc" Mover))) 4) 1))))
        
        								(move  
        									(from (sites Hand Mover))
        									(to (mapEntry "Loc" (mover)))
        									(then (set Score Mover 0)))})							
        
        							(priority {
        								(move
        									(from (sites Hand Mover))
        									(to (mapEntry "Loc" (mover)) if:(!= (% (what at:(from)) 4) 1)) (then
        									(set Score Mover (+ (/ (what at:(mapEntry "Loc" Mover) level:(topLevel at:(mapEntry "Loc" Mover))) 4) 1))))
        
        								(move  
        									(from (sites Hand Mover))
        									(to (mapEntry "Loc" (mover)))
        									(then (set Score Mover 0)))}) 
        						)
        
        
        					) (nextPhase (= (% (+ (count Moves) 1) 5) 4) "Trick")
        				)
        
        				(phase "Trick"
        					(play (move Pass (then "Switch")))
        					(end {(if ("HandEmpty" Next) (byScore))})
        					(nextPhase "Lead")
        				)
        
        
        			}
        		)
        	)
        
        
        
        		(metadata    
        			(graphics {
        				(piece Foreground "Square1"  image:"2"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square2"  image:"2"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square3"  image:"2"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square4"  image:"2"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square5"  image:"3"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square6"  image:"3"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square7"  image:"3"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square8"  image:"3"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square9"  image:"4"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square10" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square11" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square12" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square13" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square14" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square15" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square16" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square17" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square18" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square19" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square20" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square21" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square22" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square23" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square24" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square25" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square26" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square27" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square28" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square29" image:"9"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square30" image:"9"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square31" image:"9"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square32" image:"9"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square33" image:"X"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square34" image:"X"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square35" image:"X"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square36" image:"X"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square37" image:"J"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square38" image:"J"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square39" image:"J"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square40" image:"J"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square41" image:"Q"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square42" image:"Q"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square43" image:"Q"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square44" image:"Q"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square45" image:"K"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square46" image:"K"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square47" image:"K"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square48" image:"K"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(piece Foreground "Square49" image:"A"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:0.6)
        				(piece Foreground "Square50" image:"A"  fillColour:(colour Hidden) edgeColour:(colour Pink) scale:0.6)
        				(piece Foreground "Square51" image:"A"  fillColour:(colour Hidden) edgeColour:(colour Blue)   scale:0.6)
        				(piece Foreground "Square52" image:"A"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:0.6)
        				(show Edges Hidden)                                                  
        				(show Symbol "Hexagon" (sites Board) fillColour:(colour 185 130 85) edgeColour:(colour 225 182 130)) 
        				(board Placement scale:0.8)
        				(stackType None)
        				(board Background image:"square.svg" fillColour:(colour 185 130 85) edgeColour:(colour 185 130 85) scale:1.45)
        			})
        		)
        
        
*The Die here cannot be rolled: merely manipulated as a 3d object. A more fitting name might be "polyhedron".
The Dice can be rolled in-game, but only as specified. Thus, any stochastic object is a container at present.
