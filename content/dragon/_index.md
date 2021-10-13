---
title: "Dragon"
menutitle: "02-Dragon"
date: 2021-10-12T13:02:41+08:00
weight: 2
tags: []
description: "Through this example you can learn how to define variables and show them on the stage."
---

<center style="width:100%; height:360px">
<iframe src="../dragon.html" style="width: 480px;height:360px; border:0" allow="autoplay"></iframe>
</center>

Through this example you can learn how to define variables and show them on the stage.

Here are all the codes of [Dragon](tutorial/02-Dragon/Dragon.spx):

```coffee
var (
	score int
)

onStart => {
	score = 0
	for {
		turn rand(-30, 30)
		step 5
		if touching("Shark") {
			score++
			play chomp, true
			step -100
		}
	}
}
```

We define a variable named `score` for `Dragon`. After the program starts, it moves randomly. And every time it touches `Shark`, it gains one score.

How to show the `score` on the stage? You don't need write code, just add a `stageMonitor` object into [res/index.json](tutorial/02-Dragon/res/index.json):

```json
{
  "zorder": [
    {
      "type": "stageMonitor",
      "target": "Dragon",
      "val": "getVar:score",
      "color": 15629590,
      "label": "score",
      "mode": 1,
      "x": 5,
      "y": 5,
      "visible": true
    }
  ]
}
```