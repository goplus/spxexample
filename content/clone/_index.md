---
title: "Clone"
menutitle: "03-Clone"
date: 2021-10-12T16:42:21+08:00
weight: 3
tags: []
description: "Through this example you can learn how to clone sprites and destory them."
---

<center style="width:100%; height:360px">
<iframe src="../clone.html" style="width: 480px;height:360px; border:0" allow="autoplay"></iframe>
</center>

Through this example you can learn:
* Clone sprites and destory them.
* Distinguish between sprite variables and shared variables that can access by all sprites.

Here are some codes in [Calf.spx](tutorial/03-Clone/Calf.spx):

```go
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

When we click the sprite `Calf`, it receives an `onClick` event. Then it calls `clone` to clone itself. And after cloning, the new `Calf` sprite will receive an `onCloned` event.

In `onCloned` event, the new `Calf` sprite uses a variable named `gid`. It doesn't define in [Calf.spx](tutorial/03-Clone/Calf.spx), but in [index.gmx](tutorial/03-Clone/index.gmx).


Here are all the codes of [index.gmx](tutorial/03-Clone/index.gmx):

```go
var (
	Arrow Arrow
	Calf  Calf
	gid   int
)

run "res", {Title: "Clone and Destory (by Go+)"}
```

All these three variables in [index.gmx](tutorial/03-Clone/index.gmx) are shared by all sprites. `Arrow` and `Calf` are sprites that exist in this project. `gid` means `global id`. It is used to allocate id for all cloned `Calf` sprites.

Let's back to [Calf.spx](tutorial/03-Clone/Calf.spx) to see the full codes of `onCloned`:

```go
onCloned => {
	gid++
	id = gid
	step 50
	say id, 0.5
}
```

It increases `gid` value and assigns it to sprite `id`. This makes all these `Calf` sprites have different `id`. Then the cloned `Calf` moves forward 50 steps and says `id` of itself.

Why these `Calf` sprites need different `id`? Because we want destory one of them by its `id`.

Here are all the codes in [Arrow.spx](tutorial/03-Clone/Arrow.spx):

```go
onClick => {
	broadcast "undo", true
	gid--
}
```

When we click `Arrow`, it broadcasts an "undo" message (NOTE: We pass the second parameter `true` to broadcast to indicate we wait all sprites to finish processing this message).

All `Calf` sprites receive this message, but only the last cloned sprite finds its `id` is equal to `gid` then destroys itself. Here are the related codes in [Calf.spx](tutorial/03-Clone/Calf.spx):

```go
onMsg "undo", => {
	if id == gid {
		destroy
	}
}
```
