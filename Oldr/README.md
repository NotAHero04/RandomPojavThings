Hey, it's me - the "workaround finder" - again! When I first touched PojavLauncher, I was surprised by some oddballs that happened on the iOS version: not any Minecraft version 1.2.x, but 1.2.5, worked. That's kinda strange since usually there's little change between minor versions right? (Oh, I heard you screaming, 1.18.2.)

If you're trying to launch them now, you'll see some beautiful lines in the log says:

```
 FATAL ERROR in native method: Thread[Minecraft main thread,10,main]: No context is current or a function that is not available in the current context was called. The JVM will abort execution.
	at org.lwjgl.opengl.GL11C.nglGetString(Native Method)
	at org.lwjgl.opengl.GL11C.glGetString(GL11C.java:975)
	at org.lwjgl.opengl.GL11.glGetString(GL11.java:3375)
	at uq.<init>(SourceFile:66)
	at hn.a(SourceFile:2079)
	at net.minecraft.client.Minecraft.b(SourceFile:156)
	at net.minecraft.client.Minecraft.run(SourceFile:653)
	at java.lang.Thread.run(Thread.java:748)
```
(example taken from 1.2.1)

Let's take a look at where this crash started happening, and... (Do you mean to say "the way to fix it"? Nah, at least not now.)

### A Brief Look

After some research, I got the first ever version which suffers this kind of crash: 12w04a. The stack trace is quite similar to the one above. With a decompiler [like this one](https://jdec.app), I was able to compare between 12w03a and 12w04a. We'll take a look at the `net.minecraft.client.Minecraft` class, since that class appeared at the bottom of the stack trace, and also the most sus[picious] one.

There was an additional import to `org.lwjgl.Sys`, but it only helps finding the LWJGL version and print it out, at line 1666. But the big surprise happened.

Not too far from that, at line 1643, the code was:

```
AWTGLCanvas var6 = new AWTGLCanvas();
```

which was a change from 12w03a, at line 1642:

```
Canvas var6 = new Canvas();
```

So, could this be a problem, since we're dealing with "no OpenGL context" (and the canvas thing in 12w04a has "GL" in its name I think)? Let's find out.

### Modifying bytecode

Recompiling the decompiled code was a pain, mainly because: 

> It is a compile time error to import a type from the unnamed package.

It was unsuccessful anyway, so modifying bytecode is the only way to go.

**WARNING. PLEASE, READ EVERY WORD.**

Before trying all of the stuff below, you acknowledged that redistributing Minecraft, whether it's modified or not modified, is prohibited. Therefore, you must not share your work (yes, I have to say "your work", not mine) on modifying Minecraft code with other people! If you agree with this, let's continue.

This trial was made in the version 12w04a.

First, install one hexadecimal editor from anywhere: Google Play, APKPure...

Take the `Minecraft.class` file from the game archive (located in net/minecraft/client).

The goal is simple: properly replace all occurrences of `org/lwjgl/OpenGL/AWTGLCanvas` by `java/awt/Canvas`.

It turns out there's only one place in the bytecode that we'll need to pay attention; search for it using the built-in search feature.

The bytecode will look like this:

```
3010 | 01 00 1c 6f 72 67 2f 6c | . . . o r g / l
3018 | 77 6a 67 6c 2f 6f 70 65 | w j g l / o p e 
3020 | 6e 67 6c 2f 41 57 54 47 | n g l / A W T G 
3028 | 4c 43 61 6e 76 61 73 01 | L C a n v a s .
```

(again, this is taken from 12w04a, so don't expect the absolute byte position to be the same if you are dealing with other versions.)

The byte at position 3012 turns out to be the "length" of the following string (`1c` means 28 in decimal). The sequence `01 00` seems to act like a signature for something that I don't know, so I'll keep it.

After modifying that piece of bytecode, it should become
```
3010 | 01 00 0f 6a 61 76 61 2f | . . . j a v a /
3018 | 61 77 74 2f 43 61 6e 76 | a w t / C a n v
3020 | 61 73 01 00 18 6f 72 67 | a s . . . o r g
```

Don't forget to delete `META-INF` in the game archive since we are applying **mods** (or "modifications" if you are confusing).

Give it a test run, and...

[](https://media.discordapp.net/attachments/907981794303934544/963450859566551040/Screenshot_20220412-214806.png)

It definitely works! You can do these steps to modify any client that's affected by this issue' from 12w04a to 1.2.4 (other ones are untested).

That's it for today! Thanks for reading this guide and trying to become a Java pro (since bytecode manipulation is not an easy thing, at least).
