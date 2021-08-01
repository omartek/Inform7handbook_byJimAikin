## The Skein {#the-skein}

As explained in **pages 1.7 and 1.8** of _Writing with Inform_ (“The Skein” and “A short Skein tutorial”), the Skein and Transcript panels are used to replay series of commands and examine the output. All the time you’re testing your game in the Game panel, Inform is quietly recording all of your commands and all of the output from the game. The commands are added to the Skein.

At any time you can switch to the Skein and double-click on a node (the Documentation calls them “knots”), and the game will first be compiled and then replayed from the start up to that knot. The Skein is like the Replay button in the top toolbar, except that it can replay _any_ playing session, not just the most recent one.

As you work on your game, the Skein can get pretty crowded with branches (Inform calls them “threads”) that are no longer needed. You can trim these out by right-clicking (Mac: Ctrl-clicking) and choosing “Delete all below”. But you won’t be allowed to delete the currently active thread — the one that represents the game that’s currently in progress in the Game panel. If you want to do that, you first need to click the Stop button in the toolbar. If you want to delete everything in the Skein, you can use the Trim button. This will get rid of everything except the currently active thread and any threads that you have locked.

As your game gets closer to being finished, the Skein will become quite useful. You can play the entire game manually from start to finish — until you get to the ***** You have won ***** message (or whatever message you’re using to indicate victory) — and then open the Skein and _lock_ the thread you’ve just created. Locking a thread will stop Inform from deleting it, even if you use the Trim button. In the Transcript panel, you can _bless_ the output transcript for this thread. Blessing tells Inform that this is the output you hope to see in the final, released version of the game. Blessing has no actual effect on the game itself; it’s just a record-keeping function. It stores a transcript so that you can look at it later.

After making further changes in your game (to eliminate bugs, for instance, or to add features suggested by your testers), you can run the game again using the locked thread, as a quick check to make sure you haven’t done anything that makes the game unwinnable. You can then inspect the output in the Transcript to see what changes have appeared. Using the “Next diff” and “Prev diff” buttons, you can step through any changes that your recent work has made in the transcript.

There’s more to using the Skein and Transcript than we’ve covered in this brief overview.After you’ve been writing in Inform for a while, you’ll start to see how useful these features can be.