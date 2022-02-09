Hi there! If you are here, you are probably facing issues launching Minecraft Forge versions in the range of 1.3.2-1.7.2.

I raised this question to the development team two months ago (at the time of this writing, February 9), but neither they nor I have an idea to fix it, until...

I looked back at the error log from Forge 1.7.2, and found something interesting: (This is actually taken from an error log with Forge 1.6.4, but they should be identical)
> 2021-12-30 15:52:06 [SEVERE] [ForgeModLoader] Unable to launch
> 
> java.lang.reflect.InvocationTargetException
>
>	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
>
>	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
>
>	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
>
>	at java.lang.reflect.Method.invoke(Method.java:498)
>
>	at net.minecraft.launchwrapper.Launch.launch(Launch.java:131)
>
>	at net.minecraft.launchwrapper.Launch.main(Launch.java:27)
>
> Caused by: java.lang.NoClassDefFoundError: net/minecraft/client/Minecraft
>
>	at net.minecraft.client.main.Main.main(SourceFile:37)
>
>	... 6 more
>
> Caused by: java.lang.ClassNotFoundException: net.minecraft.client.Minecraft
>
>	at net.minecraft.launchwrapper.LaunchClassLoader.findClass(LaunchClassLoader.java:186)
>
>	at java.lang.ClassLoader.loadClass(ClassLoader.java:418)
>
>	at java.lang.ClassLoader.loadClass(ClassLoader.java:351)
>
>	... 7 more
>
> **Caused by: java.lang.IllegalArgumentException**
>
>	**at org.objectweb.asm.ClassReader.<init>(Unknown Source)**
>
>	**at org.objectweb.asm.ClassReader.<init>(Unknown Source)**
>
>	**at cpw.mods.fml.common.asm.transformers.deobf.FMLDeobfuscatingRemapper.findAndMergeSuperMaps(FMLDeobfuscatingRemapper.java:378)**
>
>	**at cpw.mods.fml.common.asm.transformers.deobf.FMLDeobfuscatingRemapper.getMethodMap(FMLDeobfuscatingRemapper.java:355)**
>
>	**at cpw.mods.fml.common.asm.transformers.deobf.FMLDeobfuscatingRemapper.mapMethodName(FMLDeobfuscatingRemapper.java:328)**
>
>	**at org.objectweb.asm.commons.RemappingMethodAdapter.visitMethodInsn(Unknown Source)**
>
>	**at org.objectweb.asm.ClassReader.a(Unknown Source)**
>
>	**at org.objectweb.asm.ClassReader.b(Unknown Source)**
>
>	**at org.objectweb.asm.ClassReader.accept(Unknown Source)**
>
>	**at org.objectweb.asm.ClassReader.accept(Unknown Source)**
>
>	**at cpw.mods.fml.common.asm.transformers.DeobfuscationTransformer.transform(DeobfuscationTransformer.java:39)**
>
>	**at net.minecraft.launchwrapper.LaunchClassLoader.runTransformers(LaunchClassLoader.java:274)**
>
>	**at net.minecraft.launchwrapper.LaunchClassLoader.findClass(LaunchClassLoader.java:172)**
>
>	**... 9 more**
>
> Java Exit code: 0

Yes, the actual reason here is the incorrect handling of the ASM library!

With that in mind, I tried to swap ASM libraries between 1.7.2 and the nearest working one, 1.7.10, and it worked nicely.

I talked to [@Aerolome](https://github.com/Aerolome) about that discovery, and he quickly brought it to other buggy versions (1.3.2, 1.4.2, 1.5.1, 1.5.2, 1.6.2, 1.6.4).

So I'm here to share you all the works from us. The patches modify the JSON data of corresponding Forge instances to always download ASM library version 5.2, which is also tested and verified to be working.

For Forge 1.7.2, you may also want to look at [this nice article from Minecraft Forum](https://www.minecraftforum.net/forums/mapping-and-modding-java-edition/minecraft-mods/2206446-forge-1-6-4-1-7-2-java-8-compatibility-patch).

That's all you want to get Forge working in these versions. Have a good mining day!
