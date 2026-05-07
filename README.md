# Thrustmaster Ferrari 458 Spider -- PC Compatibility

A small Windows tool that makes the Thrustmaster Ferrari 458 Spider Racing Wheel (and other "Xbox-only" racing wheels) usable in PC games.

It reads the wheel through Windows' native Xbox racing-wheel API and exposes it to games as a standard virtual Xbox 360 controller (via [ViGEmBus](https://github.com/nefarius/ViGEmBus)). Any game that supports an Xbox controller will then see your wheel.

## Requirements

- Windows 10 (22H2 or newer) or Windows 11
- [.NET 6 Desktop Runtime (x64)](https://dotnet.microsoft.com/download/dotnet/6.0)
- [ViGEmBus driver](https://github.com/nefarius/ViGEmBus/releases) (one-time install)
- An Xbox racing wheel that Windows recognises out of the box. Make sure any third-party Thrustmaster / vendor drivers are **uninstalled** — Windows' built-in driver is what this tool relies on.

## Install

1. Install the ViGEmBus driver from the link above and reboot if it asks.
2. Download `WheelCompatibilityConfigurator.exe` from the [Releases page](https://github.com/cmumme/XboxWheelCompatibility/releases).
3. Plug in your wheel.
4. Run the exe.

That's it — there is no installer and no Windows service.

## Usage

- The window shows whether the wheel is detected and whether the virtual gamepad is active. A live readout tells you the steering / throttle / brake values it's reading from the wheel.
- **Keep the window open the whole time you're playing.** Closing it disconnects the virtual gamepad.
- The first time you launch a new game, you'll need to bind controls to the new "Xbox 360 Controller" device. Most games auto-detect it; some (notably **BeamNG.drive**) require you to map every axis and button manually under Options → Controls.

## Mapping

| Wheel input          | Virtual Xbox 360 input |
|----------------------|-----------------------|
| Steering             | Left thumbstick X     |
| Throttle pedal       | Right trigger         |
| Brake pedal          | Left trigger          |
| Right paddle / Next gear | RB                |
| Left paddle / Prev gear  | LB                |
| Button 1 / 2         | Start / Back          |
| Button 3 / 4 / 5 / 6 | A / B / X / Y         |
| D-pad                | D-pad                 |

## Notes / known limitations

- Because games see the wheel as an Xbox 360 *gamepad* (not a generic DirectInput wheel), some sims apply gamepad-style steering filters. If steering feels too sensitive or clamped, look for a "direct" or "raw" steering option in your game's controller settings.
- No force feedback. The Xbox-wheel HID protocol used here doesn't expose FFB.
- Only one wheel is supported at a time. The most-recently-added wheel wins.

## Tested with

- Thrustmaster Ferrari 458 Spider (Windows 10 22H2, Windows 11)

## Building from source

Requires .NET 6 SDK.

```
git clone https://github.com/cmumme/XboxWheelCompatibility
cd XboxWheelCompatibility/WheelCompatibilityConfigurator
dotnet publish -c Release -r win-x64 --self-contained false -p:PublishSingleFile=true -p:IncludeNativeLibrariesForSelfExtract=true
```

The output exe lands in `bin/Release/net6.0-windows10.0.22000.0/win-x64/publish/`.

Pass `--self-contained true` instead if you want a build that doesn't require the .NET runtime to be installed (larger file).

## Credits

This project is built on top of [Xbox Wheel Compatibility](https://github.com/cmumme/XboxWheelCompatibility) by [@cmumme](https://github.com/cmumme), which proved the original concept of using `Windows.Gaming.Input` to read Xbox-only racing wheels on PC. The current version replaces the input-injection layer with [ViGEmBus](https://github.com/nefarius/ViGEmBus), drops the Windows-service architecture in favour of a single user-session app, and adds the configurator UI (button mapping, axis tuning, overlay mode).

## License

MIT — see [LICENSE](LICENSE). Original copyright © 2022 Camren Mumme; modifications by subsequent contributors.
