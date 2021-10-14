---
title: "Bullet"
menutitle: "04-Bullet"
date: 2021-10-12T16:59:10+08:00
weight: 4
tags: []
description: "Through this example you can learn how to keep a sprite following mouse position and fire bullets."
---

<center style="width:100%; height:360px">
<iframe src="../bullet.html" style="width:202px; height:360px;border:0" allow="autoplay"></iframe>
</center>

Through this example you can learn:
* How to keep a sprite following mouse position.
* How to fire bullets.

It's simple to keep a sprite following mouse position. Here are some related codes in [MyAircraft.spx](tutorial/04-Bullet/MyAircraft.spx):


```go
onStart => {
	for {
		# ...
		setXYpos mouseX, mouseY
	}
}
```

Yes, we just need to call `setXYpos mouseX, mouseY` to follow mouse position.

But how to fire bullets? Let's see all codes of [MyAircraft.spx](tutorial/04-Bullet/MyAircraft.spx):

```go
onStart => {
	for {
		wait 0.1
		Bullet.clone
		setXYpos mouseX, mouseY
	}
}
```

In this example, `MyAircraft` fires bullets every 0.1 seconds. It just calls `Bullet.clone` to create a new bullet. All the rest things are the responsibility of `Bullet`.

Here are all the codes in [Bullet.spx](tutorial/04-Bullet/Bullet.spx):

```go
onCloned => {
	setXYpos MyAircraft.xpos, MyAircraft.ypos+5
	show
	for {
		wait 0.04
		changeYpos 10
		if touching(Edge) {
			destroy
		}
	}
}
```

When a `Bullet` is cloned, it calls `setXYpos MyAircraft.xpos, MyAircraft.ypos+5` to follow `MyAircraft`'s position and shows itself (the default state of a `Bullet` is hidden). Then the `Bullet` moves forward every 0.04 seconds and this is why we see the `Bullet` is flying.

At last, when the `Bullet` touches screen `Edge` or any enemy (in this example we don't have enemies), it destroys itself.

These are all things about firing bullets.
