---
title: RuneScape & Old School RuneScape
date: 2021-07-24 12:00:00 +0000
icon: assets/thumbnails/2021/runescape.png
alttext: Some work I did
categories: [Work, RuneScape]
tags: [RuneScape, Old-School-RuneScape] # TAG names should always be lowercase
portfolio: true
hidden: true
---

![animation-reference]({{site.url}}/assets/posts/2021-07-24-runescape/banner.jpg)

Released in 2001, RuneScape has been one of the largest names in the MMORPG space for over 20 years. It sees players travel the medieval fantasy world of Gielinor, completing quests, slaying monsters, and training skills to strengthen their character. It was one of my favourite games growing up, so I'm glad to be a part of its never-ending legacy.

In 2013 a separate, older version of the game called Old School RuneScape was restored from a backup and released, as an exact mirror of RuneScape as it was in August 2007, in response to community backlash against the direction that RuneScape was going. Since then, both games have operated in parallel and receive their own weekly content updates.

Both games are available cross-play on PC, Mac, Linux, Android, and iOS using our cross-platform C++ client tech.

---

## RuneScape

![animation-reference]({{site.url}}/assets/posts/2021-07-24-runescape/rs3.jpg)

RuneScape is the modern version of the game, updated with new graphical features and content, but still with player characters dating back to 2001. I joined the game client team shortly after the release of the modern C++ client in 2016, where I worked on:

- Modernisation
- Android/iOS features and fixes
- New features to support weekly content updates
- Continuous integration improvements
- Bug fixes
- Polish

I also spent some time working on modernisation investigations for the game server, written in Java, including:

- Pathfinding optimisations
- Script engine improvements

Check out an official blogpost I wrote for the 'smooth movement' update [here](https://secure.runescape.com/m=news/a=135/dev-blog---movement-in-runescape).

---

## Old School RuneScape

![animation-reference]({{site.url}}/assets/posts/2021-07-24-runescape/osrs.jpg)

Old School RuneScape is a game that promises to stick to its roots, to provide a retro medieval fantasy MMORPG that stays true to RuneScape's original game philosophy. I joined the team in 2020 to work on the C++ port of the game client, which was nearing parity with the original Java client. Since then, we have focused a huge effort on adding modern quality-of-life features to the new client using new tech alongside our in-house scripting language known as "RuneScript", for which I have become familiar with the compiler and interpreter.

- Build & deployment plans using Atlassian Bamboo
- Steam deployment and integration
- Support for remote-working with the new client
- Entity highlights and overlays (in the most bizarre software/hardware renderer you'll ever see)
- Many script engine additions.
