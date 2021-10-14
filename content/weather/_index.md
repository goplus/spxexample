---
title: "Weather"
menutitle: "01-Weather"
description: "Through this example you can learn how to listen events and do somethings."
weight: 1
---
<center style="width:100%; height:360px">
<iframe src="../weather.html" style="width: 480px;height:360px; border:0" allow="autoplay"></iframe>
</center>

Through this example you can learn how to listen events and do somethings.

Here are some codes in [Kai.spx](tutorial/01-Weather/Kai.spx):

```go
onStart => {
	setCostume "kai-a"
	play recordingWhere
	say "Where do you come from?", 2
	broadcast "1"
}

onMsg "2", => {
	play recordingCountry
	say "What's the climate like in your country?", 3
	broadcast "3"
}

onMsg "4", => {
	play recordingBest
	say "Which seasons do you like best?", 3
	broadcast "5"
}
```

We call `onStart` and `onMsg` to listen events. `onStart` is called when the program is started. And `onMsg` is called when someone calls `broadcast` to broadcast a message.

When the program starts, Kai says `Where do you come from?`, and then broadcasts the message `1`. Who will recieve this message? Let's see codes in [Jaime.spx](tutorial/01-Weather/Jaime.spx):

```go
onMsg "1", => {
	play recordingComeFrom
	say "I come from England.", 2
	broadcast "2"
}

onMsg "3", => {
	play recordingMild
	say "It's mild, but it's not always pleasant.", 4
	# ...
	broadcast "4"
}
```

Yes, Jaime recieves the message `1` and says `I come from England.`. Then he broadcasts the message `2`. Kai recieves it and says `What's the climate like in your country?`.

The following procedures are very similar. In this way you can implement dialogues between multiple actors.
