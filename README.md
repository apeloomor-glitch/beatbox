# Beatbox - in-game music menu (Fabric, Minecraft 26.2)

Play your own MP3 / WAV files from inside Minecraft: library with search, queue,
shuffle, repeat, volume, four colour themes. Client-side only.

## Build
1. Install JDK 25 and Gradle 9.5.1 (or copy `src/`, `build.gradle`, `gradle.properties`,
   `settings.gradle` into a fresh Fabric example mod project, which already has the Gradle wrapper).
2. In this folder run:  `gradle build`   (or `./gradlew build` with the wrapper)
3. Your mod is `build/libs/beatbox-1.0.0.jar`. Put it in `.minecraft/mods/` together with
   Fabric API 0.161.0+26.2 (Fabric Loader 0.19.5+).

## Use
- Press **B** in game (rebindable under Controls > Beatbox).
- Put `.mp3` / `.wav` files in `.minecraft/beatbox/music/` (subfolders work), then press Rescan.
- Library: click = play the list from there, right-click = add to queue.

## If the build fails
This was written without access to the 26.2 jars, so a compile error or two is possible.
The lines most likely to need a tweak, in order:
1. `BeatboxClient.java` import `net.fabricmc.fabric.api.client.keymapping.v1.KeyMappingHelper` (package name)
2. `BeatboxClient.showScreen` -> `client.gui.setScreen(screen)` (may be `client.setScreenAndShow(screen)`)
3. `BeatboxScreen.extractBackground` (the override that draws the panel behind the buttons)
4. `BeatboxScreen`: `event.button()` / `event.x()` on `MouseButtonEvent`, `Util.getPlatform().openPath`
5. `build.gradle`: `include "javazoom:jlayer:1.0.1"` (bundles the MP3 decoder)
Paste the error text back and it's a quick fix.

## What was tested
The audio engine (library scan, MP3 + WAV decoding, queue, next/previous, pause/resume,
repeat, error handling, volume) was run against real MP3/WAV files with a fake audio output.
The menu and Minecraft hooks were only compiled against hand-written stand-ins for the 26.2 API.

## No JDK? Build it online for free
1. Create a new repository on github.com and upload the contents of this folder
   (including the hidden `.github` folder).
2. Open the **Actions** tab -> the "Build jar" run -> download the **beatbox-jar** artifact.
   If it fails, open the run log and send me the red error lines.
