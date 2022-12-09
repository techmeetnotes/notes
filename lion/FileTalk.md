## 🗃️ FileTalk 🗃️

2022-12-07

So, last I was talking with y'all, I was switching from FileTalk to using RabbitMQ as a communications system.

What I found as I was working more and more with it, was that: "This is really a poor fit for what I'm trying to do."
Most of the time, I am focused on three modes of communication:
1. Publishing
2. Broadcast
3. RPC
... via 4. Linked JSON Documents

RabbitMQ's structure is capable of doing these three (four) things, but it makes it awkward to do so.  The setup of queues and exchanges doesn't really match what I'm doing.  I found myself creating file documents on the disk, and then using RabbitMQ awkwardly, to point to these file resources on disk.  I found that I was writing FileTalk-like code, anyways, on top of the RabbitMQ system.

This led me to realize that the primary reason I was using RabbitMQ, was to make it so that one process could alert another process to "pay attention, you have incoming mail."
But I also realized in the same step that if I had just that functionality, I would have everything that I really needed from RabbitMQ.

I also am always constantly fearing the burden of RabbitMQ.  Namely, for somebody to use it, they have to install it, and configure it, which means that everybody I want to share my stuff with, has to commit to installing RabbitMQ and using it.
It just doesn't seem necessary, to me.
There are several means of having one process shake sense into another process.
Note that this doesn't really mean "communication" -- sending elaborate, structured data from process to process.
All that's needed, is a mechanism for a process to say, "Hey, please, pay attention now.  Something is in the mail for you."

I call this mechanism "Rattle."  In Lion's Technical Agenda 2023, it corresponds to entry M5.
There are several mechanisms that are low-budget that can be used to implement a Rattle.

1. TCP/IP  -- the most obvious, and most performant, due to "select", but irritating to write
2. Polling Files on Disk -- also very obvious, though annoying for entirely different reasons [note-2]
3. Polling Shared Memory -- performance hit from regular polling
4. Signals -- though Signals have their own problems [note-4]

[note-2]: the performance sucks, especially as you scale up -- either you poll less frequently (leading to mounting latency when notifications daisy-change,) or you suck up more CPU time polling what, for the most part, will show up nothing
[note-4]: there are only a tiny number of them, most of them are already used for process manipulation by the OS, and it can be OS dependent -- and ecology-dependent! -- about what they do

