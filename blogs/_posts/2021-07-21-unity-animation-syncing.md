---
title: Unity - Animation-synced movement
date: 2021-07-21 09:00:00 +0000
icon: assets/thumbnails/2021/walkcycle.png
alttext: Synchonising character movement to sprite animation in Unity
categories: [Unity]
tags: [Unity, C#, animation] # TAG names should always be lowercase
blog: true
---

I've recently been working on a side project with a couple of friends using Unity. It's a hand-drawn game that takes place inside a cosy cafe in a small town populated by animals. It's slow-paced, at a 2.5D perspective, with a focus on atmosphere and narrative, so we want to pay a lot of attention to how the game looks and feels.

## Creating Our Walk Cycle

---

Once we had settled on a theme, one of the first things we wanted to work on was the look of the game. Our very talented artist Lark whipped up a cute idle animation for the player character, at 12 frames-per-second, and we knew immediately that this is what we're going for. The next step was a walk-cycle, which had a few pre-requisites.

First, we needed to decide on a movement speed, which in turn required a rough to-scale environment so we could properly feel it out. To keep things simple, the character moves at a fixed speed with no acceleration or deceleration One we were happy with the movement speed, we measured the distance moved in one second and divided it by twelve to get the distance moved per-animation-frame, assuming a 12fps animation. We then placed markers at intervals this distance apart, and used that as a reference for Lark to animate over.

![animation-reference]({{site.url}}/assets/posts/2021-07-21-unity-animation-sync/rough-animation.png)

Sweet! Our character animated in-time with the movement, so their feet shouldn't be sliding around the ground. Except, we weren't quite satisfied yet. The character entity is moving on Unity's Update loop which runs every time the game draws a frame, which on most machines this will be much more often than 12 times per second. If the game is running at 60fps, that means each animation frame is being displayed for 5 game-frames at a time, which still causes a very subtle 'sliding' effect.

<video muted autoplay loop>
    <source src="/assets/posts/2021-07-21-unity-animation-sync/sliding.webm" type="video/mp4">
</video>

## Fixing Our Walk Cycle

The solution might seem pretty obvious - the character entity should only move every 1/12th of a second, in-sync with the animation, so that the entity only moves when the animation frame changes. How to actually achieve this isn't so obvious, though. The character moves using a Unity CharacterController, and has a Rigidbody for collisions. This means that the movement is largely controlled by Unity's physics timestep, which takes away a lot of our control over the character's movement.

Instead, I made the sprite its own GameObject as a child of the character's main GameObject. On the sprite GameObject, I have a script that stores the position of its parent every 1/12th of a second, and each LateUpdate it will move itself back to the stored position. The script uses deltaTime, the time since the last frame, to count down to when to store a new position.

```c#
// Called every 1/12th of a second
void UpdatePosition()
{
    transform.localPosition = Vector3.zero;
    m_storedPos = transform.position;
}

void LateUpdate()
{
    transform.position = m_storedPos;
}
```

This very nearly works, but I don't trust that this will eventually de-sync from the animation slightly and cause something very strange-looking. What we really need is for the script to listen to the Animator component itself, so it knows exactly when the animation frame has changed.

```c#
// Add an event to every frame in every animation
foreach (AnimationClip clip in m_animator.runtimeAnimatorController.animationClips)
{
    // Get the framerate and number of frames
    int numFrames = Mathf.RoundToInt(clip.length * clip.frameRate);
    float secondsPerFrame = 1.0f / clip.frameRate;

    // Add an event on every frame of the animation
    for (int i = 0; i < numFrames; i++)
    {
        AnimationEvent evt = new AnimationEvent();
        evt.time = i * secondsPerFrame;
        evt.functionName = "UpdatePosition";
        clip.AddEvent(evt);
    }
}
```

This isn't actually too hard; you can set up animation events via script, so when the player character spawns, this script will add our "StorePosition" method as an event to each frame of the walk animation. I'm not too sure how the animation system ends up calling these events, but I can't imagine it will be particularly expensive even with an event every frame. The result is incredibly satisfting:

<video muted autoplay loop>
    <source src="/assets/posts/2021-07-21-unity-animation-sync/no-sliding.webm" type="video/mp4">
</video>

Look how firmly their feet plant on the ground! We can now really appreciate how wonderful this little animation is while we play the game. Physics and collisions will behave exactly as they did before, and we don't introduce any input lag as the animation itself can start ASAP. For a slow-paced game like this, the gameplay is not hurt by lowering the character's framerate, and it makes the movement feel incredibly polished.

---

There's a bit more to discuss on this topic, particularly when the camera starts moving, but for now we will call this a job well-done. If you think this walk cycle is as amazing as I do, I encourage you to check out more of Lark's work at [their Artstation page](https://www.artstation.com/larkmoerschell).
