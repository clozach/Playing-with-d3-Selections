# Purpose of this small project

In an effort to up my animation game while lowering the overall complexity of my code, I've been learning how to combine [Svelte](https://svelte.dev) with [d3 Force layouts](https://github.com/d3/d3-force).

As it turns out, it's hard to use `force` without understanding [d3 Selections](https://bost.ocks.org/mike/selection/), especially if the goal is to add- and remove shapes in response to user events. This is true even though there are no direct dependencies between the two projects; all the sample code I could find on animating objects in- and out of a `force` layout _use_ Selections, and those examples don't clearly delineate the boundaries between `force` and `selection`. You just kinda have to know. Which means learning. Which is hard to do without playing around with actual code. Hence: this project.

My next project will attempt to combine what I've learned here about Selections with what I learned [here](https://github.com/clozach/Playing-with-d3-Force-Layouts-in-Svelte) about `d3` Force.