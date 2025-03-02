# GodotSteam for Godot Engine
Steam API for the Godot game engine (version 3.x). For the Windows, Linux, and Mac platforms. 

Additional flavors include:
- [Godot 2.x](https://github.com/Gramps/GodotSteam/tree/godot2)
- [Godot 4.x](https://github.com/Gramps/GodotSteam/tree/godot4)
- [Server](https://github.com/Gramps/GodotSteam/tree/server)
- [GDNative](https://github.com/Gramps/GodotSteam/tree/gdnative)

Documentation
----------
[Documentation is available here](https://gramps.github.io/GodotSteam/) and [there is the project's Wiki page here](https://github.com/Gramps/GodotSteam/wiki).

You can also check out the Search Help section inside Godot Engine after compiling it with GodotSteam.

Feel free to chat with us about GodotSteam on the [CoaguCo Discord server](https://discord.gg/SJRSq6K).

Current Build
----------
You can [download pre-compiled versions _(currently v3.10.4)_ of this repo here](https://github.com/Gramps/GodotSteam/releases).

**Version 3.11 Changes**
- Added: server branch merged into master
- Changed: spacing in default arguments in godotsteam.h
- Changed: renamed STEAM_GAMESERVER_CALLBACK as STEAM_CALLBACK
- Removed: SteamGameServer_RunCallbacks function

**Version 3.10.5 Changes**:
- Added: more helper functions for newer networking classes, translations for steamnetworkingtypes
- Fixed: lots of compiler warnings on Linux, _thanks to gregcsokas_

**Version 3.10.4 Changes**
- Added: new helper functions for newer networking classes, translations for steamnetworkingtypes
- Fixed: default argument inputInit function, _thanks to hhyyrylainen_

**Version 3.10.3 Changes**
- Changed: various internal variable / arguments names for clarity, will affect signal-returned dictionaries

**Version 3.10.2 Changes**
- Removed: not logged in as error condition in steamInit function

**Version 3.10.1 Changes**
- Changed: various compilation errors for OSX, _thanks to SapphireMH_
- Removed: receiveRelayAuthTicket, findRelayAuthTicketForServer, getHostedDedicatedServerAddress, and getGameCoordinatorServerLogin as they rely on datagram header that was removed from base SDK

**Version 3.10 Changes**
- Added: various Steam Deck specific functions, _thanks to EIREXE_
- Added: new AppLists class of functions and callbacks
- Added: new or missing App functions, callbacks, and enums
- Added: OverlayToWebPageMode enum and unread_chat_messages_changed callback for Friends class
- Added: new Input functions and callbacks
- Added: new Parental Settings fuctions, callback, and enums
- Added: new Remote Storage functions, callback, and enums
- Added: new UGC functions, callbacks, and enum
- Added: memory allocation corrections
- Changed: updated various Input class functions
- Changed: lots of argument names internally, has no effect on usage
- Fixed: some enum names
- Fixed: various server list filter functions in Matchmaking Servers class
- Fixed: receivedRelayAuthTicket, getGameCoordinatorServerLogin, FindRelayAuthTicketForServer in Networking Sockets class
- Removed: second call for steam_api.h in godotsteam.cpp

Known Issues
----------
- **Using MinGW causes crashes.** I strongly recommend you **do not use MinGW** to compile at this time.

Quick How-To
----------
- Download this repository and unpack it.
- Download and unpack the [Steamworks SDK 1.52](https://partner.steamgames.com); this requires a Steam developer account.
- Download and unpack the [Godot source 3.x](https://github.com/godotengine/godot).
- Move the following to godotsteam/sdk/ folder:
````
    sdk/public/
    sdk/redistributable_bin/
````
- The repo's directory contents should now look like this:
````
    godotsteam/sdk/public/*
    godotsteam/sdk/redistributable_bin/*
    godotsteam/SCsub
    godotsteam/config.py
    godotsteam/godotsteam.cpp
    godotsteam/godotsteam.h
    godotsteam/register_types.cpp
    godotsteam/register_types.h
````
- Now move the "godotsteam" directory into the "modules" directory of the unpacked Godot Engine source.
  - You can also just put the godotsteam directory where ever you like and just apply the ````custom_modules=.../path/to/dir/godotsteam```` flag in SCONS when compiling.  Make sure the ````custom_modules=```` flag points to where godotsteam is located.
- Recompile for your platform:
  - **NOTE:** use SCONS flags ````production=yes tools=yes target=release_debug```` for your editor and ````production=yes tools=no target=release```` for your templates.
  - Windows ( http://docs.godotengine.org/en/stable/reference/compiling_for_windows.html )
  - Linux ( http://docs.godotengine.org/en/stable/reference/compiling_for_x11.html )
    - If using Ubuntu 16.10 or higher and having issues with PIE security in GCC, use LINKFLAGS='-no-pie' to get an executable instead of a shared library.
  - MacOS ( http://docs.godotengine.org/en/stable/reference/compiling_for_osx.html )
    - When creating templates for this, please refer to this post for assistance as the documentation is a bit lacking ( http://steamcommunity.com/app/404790/discussions/0/364042703865087202/ ).
- When recompiling the engine is finished do the following before running it the first time:
  - Copy the shared library (steam_api), for your OS, from sdk/redistributable_bin/ folders to the Godot binary location (by default in the godot source /bin/ file but you can move them to a new folder).
  - Create a steam_appid.txt file with your game's app ID or 480 in this folder.  Necessary if the editor or game is run _outside_ of Steam.

- The finished hierarchy should look like this (if you downloaded the pre-compiles, the editor files go in place of these tools files, which are the same thing):
  - Linux 32/64-bit
  ```
  libsteam_api.so
  steam_appid.txt
  ./godot.linux.tools.32 or ./godot.linux.tools.64
  ```
  - MacOS
  ```
  libsteam_api.dylib
  steam_appid.txt
  ./godot.osx.tools.32 or ./godot.osx.tools.64
  ```
  - Windows 32-bit
  ```
  steam_api.dll
  steam_appid.txt
  ./godot.windows.tools.32.exe
  ```
  - Windows 64-bit
  ```
  steam_api64.dll
  steam_appid.txt
  ./godot.windows.tools.64.exe
  ```
- Lack of the Steam API DLL/SO/DyLib (for your respective OS) or the steam_appid.txt will cause it fail and crash when testing or running the game outside of the Steam client.
  - **NOTE:** For MacOS, the libsteam_api.dylib and steam_appid.txt must be in the Content/MacOS/ folder in your application zip or the game will crash.
  - **NOTE:** For Linux, you may have to load the overlay library for Steam overlay to work:
  ```
  export LD_PRELOAD=~/.local/share/Steam/ubuntu12_32/gameoverlayrenderer.so;~/.local/share/Steam/ubuntu12_64/gameoverlayrenderer.so
  
  or 
  
  export LD_PRELOAD=~/.local/share/Steam/ubuntu12_32/gameoverlayrenderer.so;
  export LD_PRELOAD=~/.local/share/Steam/ubuntu12_64/gameoverlayrenderer.so;
  ```
  This can be done in an .sh file that runs these before running your executable.  This issue may not arise for all users and can also just be done by the user in a terminal separately.

From here you should be able to call various functions of Steamworks. You should be able to look up the functions in Godot itself under the search section. In addition, you should be able to read the Steamworks API documentation to see what all is available and cross-reference with GodotSteam.

- When uploading your game to Steam, you *must* upload your game's executable and Steam API DLL/SO/DyLIB (steam_api.dll, steam_api64.dll, libsteam_api.dylib, and/or libsteam_api.so).  *Do not* include the steam_appid.txt or any .lib files as they are unnecessary.

Donate
-------------
Pull-requests are the best way to help the project out but you can also donate through [Patreon](https://patreon.com/coaguco) or [Paypal](https://www.paypal.me/sithlordkyle)!

License
-------------
MIT license
