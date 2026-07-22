# Introduction

Sunrise v2026.5.0 introduces a fully-integrated modding system in the game, using the Lua programming language.
Specifically, Sunrise's Lua integration was built on the foundation of [Lua 5.4.8](https://www.lua.org/versions.html#5.4), and as such, some features available in Lua 5.5 and above are not currently available.

## Getting Started

When opening Sunrise for the first time, the game will create three directories in the game's save folder (whether that be `%appdata%\Sunrise` or something else, it mostly depends on your system's configuration, and whether or not the game is being ran on Windows or emulated through Wine or Valve's Proton emulator). The one you'll need to look through is the "mods" directory. This is where you can find, store, and create mods for Sunrise that are instantly detected and loaded upon game boot. **Please be aware: the game WILL load ANY Lua file placed in there.**

*Sunrise is also not compatible with Luau, the variation of Lua used by Roblox.*

To start, simply create a .lua file in the folder. For this example, the code below will print to the `latest_log.txt` file present in the root of the game's save folder:
```game_call('PrintToLog', string.format("Hello World!"))```.

Save it and run the game. You should find `LUA: Hello World!` sent to the output in the file amongst the rest of the game's normal logging.
This file will also be recognized in the game's "Mods" menu, which can be accessed below the Settings on the Main Menu.

## Limitations

There are a few important things to remember when developing in Lua for Sunrise:
1. Sunrise is **sandboxed.**. This means any file operations ran can only apply within the game's save folder, with operations outside of the folder returning with "Access Denied".
2. Lua's native `print()` function will print to the game's output, but can only be seen if running the game within the GameMaker IDE, which obviously isn't possible.
3. If playing through Steam, all abilities to earn achievements will be disabled whenever ANY mod is active.
4. When interacting with objects, the objects specified MUST exist, otherwise the code will crash Sunrise.

## Interacting with Sunrise

As you saw above, `game_call` is your key to interacting with the game and the main function you'll be using often. There are a number of built-in functions associated with `game_call`.
The list, as of v2026.5.0, includes:

### PrintToLog
Allows for the script to print directly to the game's log file, serves as the replacement to Lua's native `print()` function.
```
game_call('PrintToLog', string.format("Hello World!"))
```

The code above will print "Hello World!" prefixed with `LUA:` to the game's logs.

### GetCurrentRoom
Gets the current room the game is in.
```
game_call('GetCurrentRoom', string.format(""))
```

The code above is self-explanatory. For example, if you were in the Main Menu, this would return with `rm_menu`.
This can be hooked onto a variable:
```
room = game_call('GetCurrentRoom', string.format(""))
print(tostring(room))
```

The code above will store the current room to a variable titled `room`, then print it to the IDE output.

### SetCurrentRoom
Switches the game's current room.
```
game_call('SetCurrentRoom', string.format("rm_menu"))
```

The code above will set the current room to `rm_menu`, which is the Main Menu.
This can also be hooked onto a variable, where the room you switch to will also be the result:
```
room = game_call('SetCurrentRoom', string.format("rm_menu"))
print(tostring(room))
```

The code above switches the room to `rm_menu` and stores that into the `room` variable, then prints it to the IDE output.

### SpawnObject
Creates an object on a depth level of zero.
```
game_call('SpawnObject', string.format("obj_solid_object, 64, 32"))
```

The code above will create a solid collision object at X: 64, Y: 32 in the current room.
This can be hooked onto variables, where the resulting object created will be the result:
```
object = game_call('SpawnObject', string.format("obj_solid_object, 64, 32"))
print(tostring(object))
```

The code above will store the created `obj_solid_object` to the `object` variable, then print it to the IDE output.

### GetObjectVariable
Gets the value of a variable present in an object. Returns -1 if said variable doesn't exist in the object specified.
```
valueofvar = game_call('GetObjectVariable', string.format("obj_col1, collision_flag"))
```

The code above will store the value of `obj_col` (collision tag)'s `collision_flag` game object variable in a `valueofvar` Lua variable.
In this case, it would return `true`.

### SetObjectVariable
Allows for changing existing variables in an object. If it doesn't exist, it simply returns as -1, otherwise it returns the new value of the variable you just set.
```
valueofvar = game_call('SetObjectVariable', string.format("obj_col1, collision_flag, false"))
```

The code above will set the `collision_flag` game object variable in `obj_col1` to `false`, which makes the player (if in the room) fall through the objects, then store the new value just set in the `valueofvar` Lua variable.

## Checks and Loops

Lua for Sunrise supports `while true do` loops. These are important if you want to constantly check if, per say, the user enters a specific room:
```
while true do
  current_room = game_call('GetCurrentRoom', string.format(""))

  if current_room == "rm_menu" then
    game_call('PrintToLog', string.format("In the menu! This will output every frame you're in the menu."))
  elseif current_room == "rm_play" then
    game_call('PrintToLog', string.format("In the gamemodes menu! This will output every frame you're in THIS room as well."))
  end
end
```

The code above results in the `latest_logs.txt` file looking something like this:
```
Room switch: ref room rm_menu
LUA: In the menu! This will output every frame you're in the menu.
LUA: In the menu! This will output every frame you're in the menu.
LUA: In the menu! This will output every frame you're in the menu.
LUA: In the menu! This will output every frame you're in the menu.
LUA: In the menu! This will output every frame you're in the menu.
Room switch: ref room rm_play
LUA: In the menu! This will output every frame you're in the menu.
LUA: In the gamemodes menu! This will output every frame you're in THIS room as well.
LUA: In the gamemodes menu! This will output every frame you're in THIS room as well.
LUA: In the gamemodes menu! This will output every frame you're in THIS room as well.
LUA: In the gamemodes menu! This will output every frame you're in THIS room as well.
LUA: In the gamemodes menu! This will output every frame you're in THIS room as well.
```

Another thing to mention: `while true do` runs *every frame*, excluding frames of any lag.
