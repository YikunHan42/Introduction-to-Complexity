Beginner Option

1.  Since $x_t(1-x_t) \leq 0.25$, to guarantee $x_{t+1}=R[x_t(1-x_t)] \leq 1$, hence $R \leq 4$
   
2.  From $n_{t+1}=(birthrate-deathrate)*[n_t-\frac{n_t^2}{maxpopulation}]$, the population is always decreasing, hence $x$ will always approaches $0$
   
3. + `ask one-patch [sprout-bunnies initial-population [set old false set shape "bunny2" set color white set size 4 disperse]]` to `ask one-patch [sprout-bunnies initial-population [set old false set shape "fish" set color white set size 4 disperse]]`
   + add `ask patches [set pcolor violet]`
   
4. + `[set pcolor white]` -> `[set pcolor green]`
   + add `set label (word x-current ", " x-new)` and `set label (word x-current' ", " x-new')`

5. ~~I don't know~~  remove or comment`set label food-eaten`

6. ```netlogo
   to go
     if not any? patches with [pcolor = green] [stop]
     ask turtles
     [
       ifelse coin-flip? [right random max-turn-angle][left random max-turn-angle]  ; if coin-flip? is true, turn right else turn left
       forward random max-step-size
       if pcolor = green ; if the turtle is located on a green patch
       [
         set pcolor black
         set food-eaten (food-eaten + 1)
         ; set label food-eaten
       ]
       if (food-eaten > 2) [set color blue]
       if (food-eaten > 4) [set color yellow]
     ]
   
     tick
   end
   ```

![6](./6.gif)



Intermediate Option

1. 8
2. $a=2(a-a^2)$, then $a=0$ or $a=0.5$, the fixed point is 0.5

3. 

```netlogo
  set x-current'' x0''
  set x-new'' (R * x-current'' * (1 - x-current''))  ; logistic map

  create-turtles 1; this turtle will plot x_{t+1} vs x_t using initial condition x0'
    [
      set color red
      set xcor (x-current'' * max-pxcor)
      set ycor (x-new'' * max-pycor)
      set shape "dot"
      set size 3
    ]
  set num-turtles-created num-turtles-created + 1
  set turtlex0''-who num-turtles-created  ; "id number" for this turtle
```



```netlogo
  set x-current'' x-new''
  set x-new'' (R * x-current'' * (1 - x-current''))  ; one iteration of logistic map
  ask turtle turtlex0''-who
    [
      set xcor (x-current'' * max-pxcor)  ; update coordinates for turtle representing second initial condition
      set ycor (x-new' * max-pycor)
    ]
```



```netlogo
globals [x-current x-new x-current' x-new' x-current'' x-new'' num-turtles-created turtlex0-who turtlex0'-who turtlex0''-who]
```



Advanced Option

1. 
