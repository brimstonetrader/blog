Ludii has hidden information, with "Hands" being used in dozens of implemented games in the language.
The one thing that has been keeping me from writing card games in the system, until now, has been the
inability to effectively "shuffle" a set of objects, or return a random permutation. The naive approach
wouldn't work for a few reasons, and for the longest time this problem eluded me. I tried using modular
multiplicative rings, several exploits of properties of factorials, maps, memory, arrays, and at one 
point just putting every card in an individual spot on the grid. Eventually, I found the 
PlaceCustomStack.java class. This allowed me to finally get random partitions to occur! 
This is probably the most boring card game I could imagine.


		(game "Shuffle" 
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
				(map "Entry" {1..35} {(value Random (range 1 34)) (value Random (range 1 33)) (value Random (range 1 32)) (value Random (range 1 31)) (value Random (range 1 30)) 
									(value Random (range 1 29)) (value Random (range 1 28)) (value Random (range 1 27)) (value Random (range 1 26)) (value Random (range 1 25)) 
									(value Random (range 1 24)) (value Random (range 1 23)) (value Random (range 1 22)) (value Random (range 1 21)) (value Random (range 1 20)) 
									(value Random (range 1 19)) (value Random (range 1 18)) (value Random (range 1 17)) (value Random (range 1 16)) (value Random (range 1 15)) 
									(value Random (range 1 14)) (value Random (range 1 13)) (value Random (range 1 12)) (value Random (range 1 11)) (value Random (range 1 10)) 
									(value Random (range 1  9)) (value Random (range 1  8)) (value Random (range 1  7)) (value Random (range 1  6)) (value Random (range 1  5)) 
									(value Random (range 1  4)) (value Random (range 1  3)) (value Random (range 1  2)) 1 0})
				(map "Loc" {(pair P1 "B1") (pair P2 "C1") (pair P3 "D1") (pair P4 "E1")})})    
			
			
			(rules
				(start {
				(place Stack items:{"Square1"  "Square2"  "Square3"  "Square4"  "Square5" 		                    
									"Square6"  "Square7"  "Square8"  "Square9"  "Square10"		                    
									"Square11" "Square12" "Square13" "Square14" "Square15"		                    
									"Square16" "Square17" "Square18" "Square19" "Square20"
									"Square21" "Square22" "Square23" "Square24" "Square25"		           
									"Square26" "Square27" "Square28" "Square29"	"Square30"		  
									"Square31" "Square32" "Square33" "Square34" "Square35"} (coord:"A1"))
		
				}) 
				phases:{
				(phase "Deal"
					(play (move Pass (then  
						(fromTo (from (coord:"A1") level:(value Random (range 1 (- 35 (count Moves))))) 
								(to   (handSite Mover (/ (- (count Moves) 1) 4))))))) 
					(nextPhase (= (count Moves) 24) "Lead"))
					
				(phase "Lead"
					(play
						(move  
							(from (sites Hand Mover))
							(to (mapEntry "Loc" (mover))))			
					)
				
				
				
				
				(end {
					(if 
						(= (count Moves) 127) 
						(result P1 Loss) 
					)
				}))}
			)
		)
		
		
		
		(metadata    
			(graphics {
				(piece Foreground "Square1"  image:"0"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square2"  image:"1"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square3"  image:"2"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square4"  image:"3"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square5"  image:"4"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square6"  image:"5"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square7"  image:"6"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square8"  image:"7"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square9"  image:"8"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square10" image:"0"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square11" image:"1"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square12" image:"2"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square13" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square14" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square15" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square16" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square17" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square18" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square19" image:"0"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square20" image:"1"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square21" image:"2"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square22" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square23" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square24" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square25" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square26" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square27" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square28" image:"0"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square29" image:"1"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square30" image:"2"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square31" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square32" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square33" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square34" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square35" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(show Edges Hidden)                                                  
				(show Symbol "Hexagon" (sites Board) fillColour:(colour 185 130 85) edgeColour:(colour 225 182 130)) 
				(board Placement scale:0.8)
				(stackType None)
				(board Background image:"square.svg" fillColour:(colour 185 130 85) edgeColour:(colour 185 130 85) scale:1.45)
			})
		)
		
		(metadata    
			(graphics {
				(piece Foreground "Square1"  image:"0"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square2"  image:"1"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square3"  image:"2"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square4"  image:"3"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square5"  image:"4"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square6"  image:"5"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square7"  image:"6"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square8"  image:"7"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square9"  image:"8"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square10" image:"0"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square11" image:"1"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square12" image:"2"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square13" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square14" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square15" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square16" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square17" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square18" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square19" image:"0"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square20" image:"1"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square21" image:"2"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square22" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square23" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square24" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square25" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square26" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square27" image:"8"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square28" image:"0"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square29" image:"1"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square30" image:"2"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square31" image:"3"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(piece Foreground "Square32" image:"4"  fillColour:(colour Hidden) edgeColour:(colour Black)  scale:1)
				(piece Foreground "Square33" image:"5"  fillColour:(colour Hidden) edgeColour:(colour Red)    scale:1)
				(piece Foreground "Square34" image:"6"  fillColour:(colour Hidden) edgeColour:(colour Yellow) scale:1)
				(piece Foreground "Square35" image:"7"  fillColour:(colour Hidden) edgeColour:(colour Blue)  scale:1)
				(show Edges Hidden)                                                  
				(show Symbol "Hexagon" (sites Board) fillColour:(colour 185 130 85) edgeColour:(colour 225 182 130)) 
				(board Placement scale:0.8)
				(stackType None)
				(board Background image:"square.svg" fillColour:(colour 185 130 85) edgeColour:(colour 185 130 85) scale:1.45)
			})
		)
