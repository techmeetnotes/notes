## What Lion's up to

2022-12-07

[Technical\_Agenda\_2023.pdf](lion/202221207-Technical_Agenda_2023.pdf)

That's "Technical Agenda 2023."  It's primarily a listing of major projects that I'd like to program, with a 5-10 year time horizon from today.  That is, through my being 50-55 years old.  I have no expectation that I will implement all, or even most of these.  My expectation is that I will implement some of them.  It is also my expectation that the list will get longer, and longer, and longer.  And then at a certain point, it'll stop, because I'll be writing "Technical Agenda 2024."  Or, as per M26, it will become "computerized."  That is, stored in a database that I interact with through a GUI, and publish to the web somehow.

If I were to draw attention to specific entries, they would include:
* M4 -- [FileTalk](lion/FileTalk.md)
* M6 -- Tk Framework (GUI framework I've been developing) -- because as Ciprian pointed out to me a year ago, the GUI environment is absolutely critical
* M10 -- Graphical Python Editing Environment
* M15 -- Cross-Network FileTalk Bridge -- because I want to be able to collaborate with y'all more easily!
* M30 - M32 -- various stages of integrating Kartik's graphics + text system
* M42 -- "Vision of Computing" Document

I've also started collecting a document that I call "Programming Problems and Approaches."  I don't know if it has a future, but I think it's very interesting to me, in the present.  I keep notes on programming problems and approaches in my paper notebooks, too.  But I want to see if computerizing records of reasoning will be valuable.  I'll upload it here in a moment.

[Programming\_Problems\_and\_Approaches.pdf](lion/202221207-Programming_Problems_and_Approaches.pdf)

I think -- I realized that there are certain problems that I come back to over and over again, and the reasoning through the logic of it is as important to me as the code that embodies a particular line of reasoning.  I'm often going back to old decisions, old approaches, and finding fresh paths through them.  But some times I look through old code, and I think, "Oh, wow, that's very clever what I did there -- and I'd completely forgotten about that way of doing things."  I want to see if taking the "conversation" back to the conceptual plane, and organizing it from the conceptual plane, away from the "code" plane, (where I have to decipher what I did,) will have an impact on my development.

This document is presently tiny, but I can easily imagine filling it up with 100s of design decisions, 100s of approaches.

There's another document I started collecting, but it's too fragmentary for me to feel good about sharing presently -- focusing on documenting my internal "framework" that I use when writing programs.  There are three (or four) types of programs that I write, in the main:  Command Prompt driven programs that are terminal driven, (often with some kind of integrated menu and help system,) Tk Based GUI programs (which require a lot of framework tooling,) "Headless" servicing programs (which maintain data collections and speak with other programs through some variation of FileTalk,) and for a fourth, the occasional SDL2-based program (interactive graphics.)

The focus here is on:
* how Python modules are arranged with respect to one another
* how processes communicate with one another, and how Python is used to communicate cross-process
* how disk-based data assets are acquired and when different types of data storage is used - with special attention to durability and transaction structures
* how an interactive loop is integrated, and competing interactive loops are managed
* how GUIs are composed and linked into the program space and interacted with
* how projects and repositories are managed

If any of you should even so much as skim the work I've posted here, I'd love to hear your feedback, whatever it is.  Parts of these documents are really rough, and parts of these documents don't even belong in them, so please expect and forgive the roughness.
