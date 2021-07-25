---
title: Porting a 40-Year-Old Board Game to Unity
flair: C#
date: 2021-07-25 13:00:00 +0000
icon: assets/thumbnails/2021/darktower.png
alttext: Porting the 40-Year-Old Dark Tower Board Game to Unity
categories: [Unity]
tags: [Unity, C#] # TAG names should always be lowercase
blog: true
---

![Dark Tower]({{site.url}}/assets/posts/2021-07-25-dark-tower/darktower.jpg)

A few years ago I was introduced to the 1981 board game [Dark Tower](<https://en.wikipedia.org/wiki/Dark_Tower_(game)>), developed by Milton Bradley, which describes itself as "a fantasy adventure born of electronic wizardry". In the game, up to four players must traverse around the game board to collect three keys, with which to unlock the namesake Dark Tower in the centre of the game board. Once inside, the brave player and their amassed army must defeat an onslaught of Brigands to claim the magical scepter atop the tower and win. The Dark Tower itself is a large plastic structure with a motorised carousel, three lightbulbs, two 7-segment displays, and a keypad.

I absolutely adore this game, from the delightfully retro fantasy art adorning the box and manual, to the charming 1-bit tunes that beep through the speaker. As a game it's really quite lacking by today's standards, with very little opportunity to interact with other players and generally unintuitive controls that require you to have the whole instruction book memorised, but somehow I still find myself deeply immersed in a vast land of forests, markets, tombs, and ruins whenever I play. Back in 2016, I decided to port it to Unity.

## Rarity

---

Dark Tower is a rare game for two reasons: First, the motorised carousel and lightbulbs inside the tower are 40 years old at this point, and many won't be functioning any more. Second, the game was in print for only a short time, after claims against Milton Bradley for intellectual property theft. Because of this rarity, full boxed versions of the game can go at auction for almost £300, and a working tower alone accounts for at least half of that. And, with the tower slowly wearing itself out after each use, they're only going to become more scarce.

I was inspired, then, by the rarity of the tower as well as my utter adoration for the game and its presentation, to see if I could bring Dark Tower back to life in a way that could stand the test of time, in the form of an Android app. A cool £250 later I had myself a copy of the game to sit down with and reverse-engineer. I turned the tower on and started playing single-player and multi-player games by myself in the living room to find out exactly how the game works.

## How the Game Works

---

The tower acts as a kind of 'dungeon master' for the game. Players take turns telling the tower what they wish to do via the keypad, and the tower shows you the story as it happens by flashing a light bulb on behind a carousel of translucent cells, and displaying a number on the two 7-segment displays above the tower. You hear the laboured whir of the motor whenever the carousel rotates, followed by a sharp beep as one of the filament bulbs turns on to illuminate an image.

![Dark Tower]({{site.url}}/assets/posts/2021-07-25-dark-tower/warriors.jpg)

Your character can move one space on the game board per turn, which you communicate to the tower by pressing the key corresponding to the space you have moved to. As an example, you may move your character onto a Tomb, where you press the "Tomb/Ruin" key. The tower then plays sound of a door creaking open as your character steps into the darkness... The tower starts spinning its carousel, eventually landing on an image of Gold with the number "42" displayed, as your character comes across some treasure and adds it to their coin pouch - Your total gold is now 42! Your turn ends, and you rotate the tower to the next player.

The tower keeps track of a number of variables, including gold, warriors, food, keys collected, and more. The players needs to remember these numbers themselves using a pegboard, or else they must waste a turn asking the tower to relay their inventory back to them. The tower also runs battles, plays the role of shopkeeper, distributes loot, summons dragons, and ensures the player can't progress until they have completed their objective (usually obtaining a key).

## Unity Port

---

For the port I wanted to stay as true to the original as possible, and that includes the whirs and beeps of the tower, as well as watching the dimmed images scroll pass whilst the carousel rotates. With Unity, I could literally make this a textured 3D cylinder that rotates, and play a sound effect as the tower spins. This meant recording all of the sounds that the tower makes, playing them back at the appropriate time, and working out how fast the real tower spins so I could match it in software (it also only spins anticlockwise!).

![The tower carousel]({{site.url}}/assets/posts/2021-07-25-dark-tower/carousel.png)

As the game itself is relatively simple, I kept most of the code in a single file. A Player struct holds all the variables per player, and a number of Unity button elements represent the keypad and call into the Tower script. The script makes heavy use of coroutines to keep the code linear whilst still waiting for the tower to spin and beeps to play. The Tower script also keeps record of any fields that were just displayed, so that the 'repeat' button can function.

```c#
// Flash the player's warrior count
yield return new WaitForSeconds(spin.to(Column.C_SHOP_1));
lights.on(1);
setText(player.warriors);
yield return new WaitForSeconds(audio.play(Sounds.BEEP));
lights.off();
clearText();
```

And that's... kind of it! As I said before, the game logic is pretty simple. All that's left is to work out the odds of things happening - the game makes a lot of random rolls within certain ranges to determine things like how much gold you find, whether you win in battle, or whether your army gets lost. The method here was to just repeat actions over and over until I was confident on the odds. I've entered the Tomb/Ruins a LOT of times...

And I'm very happy with the result:

<iframe width="480" height="270" src="https://www.youtube.com/embed/EYoXfLeXsU4" frameborder="0" allowfullscreen></iframe>

(Although from the comments on that video, looks like there's a couple bugs to iron out...)

## Enhancements

---

I mentioned at the beginning of this article that the game itself is a bit lacking in player interaction, as well as some more intuitive gameplay, so I took this opportunity to add a couple of optional enhancements to the game too. The first is the ability to visit the Dark Tower outside of your own region to summon the wizard, who can be paid to curse another player to steal some warriors and gold from them. If one of your opponents is out way ahead of the rest of the group, you can use this to reel them in a bit. I also added some customisation options, such as the tower spin speed, initial warrior/gold/food counts, and enabling/disabling certain events like getting lost or receiving extra warriors from your healer.

I'd love to revisit this project some day to tidy up some of the rough edges and add some more quality-of-life features, but I'm very happy with how this turned out for now! The final piece of the puzzle, I think, is to 3D-print an actual tower to place your phone in, for the true Dark Tower experience. As I'm using the original assets I can't upload this to Google Play, but you can download an APK to install to your Android device at [this link](https://github.com/Nin1/dark-tower/releases/download/v1.3/Dark-Tower-1.3.apk).
