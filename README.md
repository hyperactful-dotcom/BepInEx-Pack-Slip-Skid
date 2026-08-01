# BepInEx Pack for Slip & Skid

BepInEx 6 (Unity.Mono), build `6.0.0-be.697`, configured for Slip & Skid.

Copy everything here into the folder containing `Slip & Skid.exe`. Launch the
game normally afterwards — BepInEx hooks itself through `winhttp.dll`, so there
are **no launch options to set**. To disable it without uninstalling, set
`enabled = false` in `doorstop_config.ini`.

## Why the game needs its own pack

Slip & Skid is aggressively managed-stripped. Its `mscorlib.dll` is 2.7 MB
against an unstripped 4.6 MB, and the missing surface includes members BepInEx's
preloader calls — a stock pack fails during preload with
`MissingMethodException: Module.GetPEKind`.

## What differs from stock BepInEx

Two things, and nothing else:

1. **`unstripped_corlib/`** is added, holding `mscorlib.dll`, `System.dll` and
   `System.Core.dll` from a matching Mono build.
2. **`doorstop_config.ini`** sets
   `dll_search_path_override = "BepInEx\core;unstripped_corlib"`, so those
   resolve ahead of the game's stripped copies. Stock leaves it empty.

`BepInEx.cfg` is stock apart from raised logging (`LogLevels = All`,
`WriteUnityLog = true`), which matters because this build strips Unity's
uncaught-exception logging — without it, plugin failures are completely silent.

No BepInEx binaries are patched or rebuilt.

## If you are configuring this for a mod manager

Mod managers do not read the game folder's `doorstop_config.ini` — they inject
Doorstop themselves and configure it through environment variables. The search
path override above has to be carried into that launch configuration, or a
manager-launched game dies at preload even though a manual install works.

## Licence

BepInEx is by the BepInEx team, licensed under **LGPL-2.1** — see
`LICENSE_BepInEx.txt`. Source: https://github.com/BepInEx/BepInEx

The bundled corlib assemblies are Mono class libraries as redistributed by Unity.
