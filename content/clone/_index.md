---
title: "03-Clone"
date: 2021-10-12T16:42:21+08:00
weight: 3
tags: []
---

### tutorial/03-Clone

<div style="width=100%; height:500px">
<iframe src="../clone.html" style="width: 100%;height:100%;" allow="autoplay"></iframe>
</div>

Through this example you can learn:
* Clone sprites and destory them.
* Distinguish between sprite variables and shared variables that can access by all sprites.

Here are some codes in [Cat.spx](tutorial/03-Clone/Cat.spx):

```coffee
var (
	id int
)

onClick => {
	clone
}

onCloned => {
	gid++
	...
}
```

When we click the sprite `Cat`, it receives an `onClick` event. Then it calls `clone` to clone itself. And after cloning, the new `Cat` sprite will receive an `onCloned` event.

In `onCloned` event, the new `Cat` sprite uses a variable named `gid`. It doesn't define in [Cat.spx](tutorial/03-Clone/Cat.spx), but in [index.gmx](tutorial/03-Clone/index.gmx).


Here are all the codes of [index.gmx](tutorial/03-Clone/index.gmx):

```coffee
var (
	Arrow Arrow
	Cat   Cat
	gid   int
)

run "res", {Title: "Clone and Destory (by Go+)"}
```

All these three variables in [index.gmx](tutorial/03-Clone/index.gmx) are shared by all sprites. `Arrow` and `Cat` are sprites that exist in this project. `gid` means `global id`. It is used to allocate id for all cloned `Cat` sprites.

Let's back to [Cat.spx](tutorial/03-Clone/Cat.spx) to see the full codes of `onCloned`:

```coffee
onCloned => {
	gid++
	id = gid
	step 50
	say id, 0.5
}
```

It increases `gid` value and assigns it to sprite `id`. This makes all these `Cat` sprites have different `id`. Then the cloned `Cat` moves forward 50 steps and says `id` of itself.

Why these `Cat` sprites need different `id`? Because we want destory one of them by its `id`.

Here are all the codes in [Arrow.spx](tutorial/03-Clone/Arrow.spx):

```coffee
onClick => {
	broadcast "undo", true
	gid--
}
```

When we click `Arrow`, it broadcasts an "undo" message (NOTE: We pass the second parameter `true` to broadcast to indicate we wait all sprites to finish processing this message).

All `Cat` sprites receive this message, but only the last cloned sprite finds its `id` is equal to `gid` then destroys itself. Here are the related codes in [Cat.spx](tutorial/03-Clone/Cat.spx):

```coffee
onMsg "undo", => {
	if id == gid {
		destroy
	}
}
```
