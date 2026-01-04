## Rulebooks &amp; “Stop the Action”

When a game is running, Inform processes actions using **rulebooks**. A rulebook is, as you might guess, a collection of rules. You can create your own rulebooks for any purpose at all if you want to, but that’s an advanced programming concept, and it isn’t covered in this _Handbook_. For many games, you won’t need to. The rulebooks built into Inform can handle most types of action.

When your game is compiled, all of the Before rules — in your own code, in Inform’s built-in library, and in any extensions that you’re using — are packed into the Before rulebook. Likewise, all of the Instead rules are packed into an Instead rulebook. (For now, we’re not going to get into the question of how Inform decides what order to put the rules in. If you want to know more about this, you can read **Chapter 19** of _Writing with Inform_, “Rulebooks.”) And all of the After rules are in a single After rulebook. But each action (for example, taking, looking, and going) has its own Check, Carry Out, and Report rulebooks.

Every rule in every rulebook ends in one of three ways: success, failure, or no decision. As an action is being processed, Inform runs through the rulebooks — Before, Instead, Check, Carry Out, After, and Report — consulting all of the rules in the order that they’re listed in the rulebook. It goes through the rules _until one of the rules ends in success or failure_. At that point, Inform knows that the action has been handled, and it stops.

Every rulebook has a _default outcome_ for the rules that are in the rulebook. This is one of the key concepts in Inform authorship. If you write a new Instead, Before, After, Check, Carry Out, or Report rule and don’t tell Inform what outcome your specific new rule will have, it will use the default outcome for that rulebook. This is important, because it affects how you write new rules.

The default for the Before, Check, and Carry Out rulebooks is to make no decision. So if you write a Before rule and don’t tell Inform that the rule has succeeded or failed, Inform will look at your rule, do whatever it says to do, and then go right on processing the action. The After rulebook, however, has a default of success, and the Instead rulebook has a default of failure. So when you write a new Instead or After rule, if you don’t say otherwise, your rule will cause action processing to stop.

The phrase “continue the action” is one of the first phrases I learned when I started writing new rules. It means exactly the same thing as “make no decision”. You can use either of these two phrases. Here’s a simple example:

```inform7
Instead of taking the live wire:
        if the player does not wear the insulated gloves:
                end the story saying “You have been electrocuted!”;
        otherwise:
                continue the action.
```

If the player is not wearing the insulated gloves, the outcome of the Instead rule will be failure (though in this case it hardly matters, as the game-ending rules will take over). If the player _is_ wearing the gloves, this Instead rule will make no decision, and action processing will continue — most likely with the rules in the Check Taking rulebook. (Since the live wire is probably attached to something at its other end, and thus can’t be carried around, this example doesn’t actually make a lot of sense, but we’ll ignore that.)

The opposite of “continue the action” is “stop the action”. I’ve been in the habit of using this — but I’m going to have to get out of the habit. Most of the time, it works just fine, but once in a while it can get you in trouble. Let’s look at why.

Here’s a simple game, for testing, that shows what can happen. I like putting a red button in the room in a test game so that I can test anything I like using the command PUSH BUTTON. In a real game, the rule shown below for taking the sponge would be more complex; there’s no need to ever do it as shown here. But the result (when the game is being played) might be similar to what you’re seeing if you have a mysterious bug in your code.

```inform7
The Lounge is a room.

A sponge is in the Lounge. A red button is in the Lounge.

Before taking the sponge:
        now the player carries the sponge;
        say "You grab the sponge.";
        rule succeeds.
        [stop the action.]

Instead of pushing the red button:
        if we have taken the sponge:
                say "Yep -- you took the sponge.";
        otherwise:
                say "Nope -- you never took the sponge."

test me with "take sponge / i / press red button"
```

For technical reasons, it’s necessary to test using the odd phrase “we have taken the sponge”. Testing “if the player has taken the sponge” won’t compile. But the point is this: If the Before rule (“Before taking the sponge”) ends with “rule succeeds”, as shown above, running the test by typing TEST ME will reveal that we have indeed taken the sponge. On the other hand, if you comment out “rule succeeds.” (by putting square brackets around it) and uncomment “stop the action.” (by removing the brackets), when you run TEST ME you’ll see that apparently the player has _not_ taken the sponge, even though it’s now in the player’s inventory.

That, in a nutshell, is why using “stop the action” is a bad idea. When you use “rule succeeds” or “rule fails” Inform does a little housekeeping behind the scenes. In this case, the housekeeping includes making note of the fact that the sponge has been taken. If you use “stop the action” the housekeeping never happens, so Inform’s record-keeping will include a mistake.

Still not convinced? Here’s another quick example.

```inform7
The Meadow is a room.

The unicorn is an animal in the Meadow. The unicorn can be angry. The unicorn is not angry. The description of the unicorn is "The unicorn is [if angry]stamping his hooves and brandishing his wickedly sharp horn at Steve[else]peacefully cropping daisies[end if]."

Steve is a man in the Meadow.

A pitchfork is in the Meadow. Understand "fork" as the pitchfork.

Persuasion rule for asking Steve to try taking the pitchfork:
persuasion succeeds.

Instead of Steve taking the pitchfork:
        now Steve carries the pitchfork;
        now the unicorn is angry;
        say "Steve scoops up the pitchfork. This evidently makes the unicorn very uneasy."
```

If all we want is for Steve to pick up the pitchfork, we don’t need an Instead rule. The Persuasion rule will cause him to carry out the action. In this example I’ve added a line about an imaginary unicorn, because it’s a quick example of why you might want to use an Instead rule rather than letting the standard library handle Steve’s action. The code seems very straightforward, but there’s a problem. Inform thinks that the action of Steve taking the sponge _failed,_ because failure is the default outcome of the Instead rulebook. Here’s the output in the game:

```
>**steve, take the pitchfork**
Steve scoops up the pitchfork. This evidently makes the unicorn very uneasy.

Steve is unable to do that.
```

Steve is now carrying the pitchfork, but the game has printed out an extra message giving the player bad information. To avoid this, we need to make sure the Instead rule succeeds:

```inform7
Instead of Steve taking the pitchfork:
        now Steve carries the pitchfork;
        now the unicorn is angry;
        say "Steve scoops up the pitchfork. This evidently makes the unicorn very uneasy.";
        rule succeeds.
```

When we add “rule succeeds”, Inform knows that the action succeeded, so no default message about the action’s failure will be printed out. These examples show that the action processing rulebooks have default outcomes, and that it’s important to know what the default outcomes are.
