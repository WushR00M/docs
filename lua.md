# Introduction

Sunrise v2026.5.0 introduces a fully-integrated modding system in the game, using the Lua programming language.
Specifically, Sunrise's Lua integration was built on the foundation of [Lua 5.4.8](https://www.lua.org/versions.html#5.4), and as such, some features available in Lua 5.5 and above are not currently available.

## Getting Started

When opening Sunrise for the first time, the game will create three directories in the game's save folder (whether that be `%appdata%\Sunrise` or something else, it mostly depends on your system's configuration, and whether or not the game is being ran on Windows or emulated through Wine or Valve's Proton emulator).

The one you'll need to look through is the "mods" directory. This is where you can find, store, and create mods for Sunrise that are instantly detected and loaded upon game boot. **Please be aware: the game WILL load ANY Lua file placed in there.**

*Sunrise is also not compatible with Luau, the variation of Lua used by Roblox.*

To start, simply create a .lua file in the folder. For this example, the code below will print to the `latest_log.txt` file present in the root of the game's save folder:
```game_call('PrintToLog', string.format("Hello World!"))```.

Save it and run the game. You should find `LUA: Hello World!` sent to the output in the file amongst the rest of the game's normal logging.
This file will also be recognized in the game's "Mods" menu, which can be accessed below the Settings on the Main Menu.
