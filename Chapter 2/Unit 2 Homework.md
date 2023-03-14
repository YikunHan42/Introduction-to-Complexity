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

similar to functional programming in Python

```netlogo
turtles-own [food-eaten return-to-nest?]

patches-own [nest?]

to setup
  clear-all
  reset-ticks
  create-turtles population

  ask turtles
  [
    set return-to-nest? false ; flag
    set shape "bug"
    set size 1
    set color red
    set food-eaten 0
  ]
  grow-food
  set-up-nest
end

to set-up-nest
  ask patches[
    set nest? (distancexy 0 0) < 2
    if nest? [set pcolor orange]
  ]
end

to go
  if not any? patches with [pcolor = green] [stop]
  ask turtles
  [ ifelse return-to-nest? = false
    [look-for-food]
    [return-to-nest]
  ]

  tick
end

to return-to-nest
  face patch 0 0 fd 1
  if pcolor = orange
  [set return-to-nest? false]
end

to look-for-food
    ifelse coin-flip? [right random max-turn-angle][left random max-turn-angle]  ; if coin-flip? is true, turn right else turn left
    forward random max-step-size
    if pcolor = green ; if the turtle is located on a green patch
    [
      set pcolor black
      set food-eaten (food-eaten + 1)
      set label food-eaten
      set return-to-nest? true
    ]
end
```





![7](./7.gif)



Advanced Option

1. Set plabel for pheromone

```netlogo
patches-own [nest? pheromone]

to setup
  clear-all
  reset-ticks
  create-turtles population

  ask turtles
  [
    set return-to-nest? false ; flag
    set shape "bug"
    set size 1
    set color red
    set food-eaten 0
  ]
  grow-food
  set-up-patches
end

to set-up-patches
  ask patches[
    set pheromone 0
    set nest? (distancexy 0 0) < 2
    if nest? [set pcolor orange]
  ]
end

to return-to-nest
  face patch 0 0 fd 1
  set pheromone pheromone + 1
  set plabel pheromone 
  if pcolor = orange
  [set return-to-nest? false]
end
```

![8](./8.gif)

2. Let pheromone evaporate

```netlogo
to go
  if not any? patches with [pcolor = green] [stop]
  ask turtles
  [ ifelse return-to-nest? = false
    [look-for-food]
    [return-to-nest]
  ]
  evaporate-pheromone
  tick
end

to evaporate-pheromone
  let x random-float 1
  ask patches with [pheromone > 0]
  [if x < probability-to-evaporate
    [set pheromone pheromone - 1]
    set plabel pheromone]
end
```

![9](./9.gif)

3. Follow the pheromone

```netlogo
turtles-own [food-eaten return-to-nest?]

patches-own [nest? pheromone]

to setup
  clear-all
  reset-ticks
  create-turtles population

  ask turtles
  [
    set return-to-nest? false ; flag
    set shape "bug"
    set size 1
    set color red
    set food-eaten 0
  ]
  grow-food
  set-up-patches
end

to set-up-patches
  ask patches[
    set pheromone 0
    set nest? (distancexy 0 0) < 2
    if nest? [set pcolor orange]
  ]
end

to go
  if not any? patches with [pcolor = green] [stop]
  ask turtles
  [ ifelse return-to-nest? = false
    [look-for-food]
    [return-to-nest]
  ]
  evaporate-pheromone
  tick
end

to return-to-nest
  face patch 0 0 fd 1
  set pheromone pheromone + 1
  set plabel pheromone 
  if pcolor = orange
  [set return-to-nest? false]
end

to look-for-food
  let pheromone-ahead? scent-at-angle 0
  let pheromone-right? scent-at-angle 45
  let pheromone-left? scent-at-angle 45 ; selected range
  
  ifelse (pheromone-right? > pheromone-ahead? or pheromone-left? > pheromone-ahead?)
  [ifelse pheromone-right? > pheromone-left?
    [rt 45]
    [lt 45]]
  
    [if pheromone-ahead? = 0
    [ifelse coin-flip? [right random max-turn-angle][left random max-turn-angle]]  ; if coin-flip? is true, turn right else turn left
    forward random max-step-size
    if pcolor = green ; if the turtle is located on a green patch
    [
      set pcolor black
      set food-eaten (food-eaten + 1)
      set label food-eaten
      set return-to-nest? true
    ]
  ]
end

to-report coin-flip?     ; returns true or false at random
  report random 2 = 0
end

to grow-food
  ;; patch procedure
  ;; setup food source one on the right
  ask patches [if (distancexy (0.6 * max-pxcor) 0) < 5
  [ set pcolor green ]
  ;; setup food source two on the lower-left
  if (distancexy (-0.6 * max-pxcor) (-0.6 * max-pycor)) < 5
  [ set pcolor green ]
  ;; setup food source three on the upper-left
  if (distancexy (-0.8 * max-pxcor) (0.8 * max-pycor)) < 5
  [ set pcolor green ]
  ]
end

to evaporate-pheromone
  let x random-float 1
  ask patches with [pheromone > 0]
  [if x < probability-to-evaporate
    [set pheromone pheromone - 1]
    set plabel pheromone]
end
; to, similar to def

to-report scent-at-angle [angle]
  let p patch-right-and-ahead angle 1
  if p = nobody [report 0]
  report [pheromone] of p
end
; usage: find the required signle patch that is the given distance from this turtle
; to-report, return numerical data, similar to int, float, double, etc.
; [pheromone] of p is similar to p.pheromone
```

![10](./10.gif)
