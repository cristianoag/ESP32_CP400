# Firmware Change Log

This log tracks user-visible changes to the ESP32 CP400 emulator firmware.
Versions use one major digit and two minor digits, for example 1.10 and 1.11.

## 1.11 - 2026-08-19

### Added

- Added a progress gauge that runs while a firmware update is validated and copied.

### Changed

- Moved the USB keyboard port to GPIO11 and GPIO12, joystick 1 to GPIO15 and GPIO16, and joystick 2 to GPIO17 and GPIO18, to simplify cable routing on the board.
- Gave every F12 screen the same layout, with the product name and screen title in the header, matching separator lines, and key hints in the footer. The disk image picker, firmware update screens, and error messages previously used their own styles.
- Renamed the "Joystick calibration" menu entry to "Joystick setup".
- Turned the joystick Serial debug output off by default.
- Listed only files with a matching extension when choosing a firmware or disk image, hiding folders such as System Volume Information that could not be opened anyway.

## 1.10 - 2026-08-18

### Added

- Added independent joystick 1 and joystick 2 control mapping from the F12 menu.
- Added guided capture for joystick neutral, up, down, left, right, button 1, and button 2 states.
- Added persistent raw HID joystick mappings to support controllers with different report layouts.
- Added a Serial joystick debug mode in the F12 menu that logs raw USB reports, decoded controls, and stored mappings at 115200 baud.
- Added a Makefile workflow that builds, packages, and verifies `.FLH` firmware updates for the F12 menu.
- Added firmware version control through the Makefile and versioned update-package filenames.

### Fixed

- Stored the PIA control registers at $FF01 and $FF03 on write, so the joystick multiplexer select bits change as the machine requests. They were never updated, which froze the multiplexer on a single channel and made every axis read return the same value.
- Restored the CP400 PIA joystick multiplexer order, which had been changed incorrectly and routed joystick 1's horizontal axis to the vertical channel while the horizontal channel read the unused second joystick.
- Centred the default joystick axis values, so a disconnected or silent joystick no longer reads as held fully left and up.
- Prevented an empty comparison mask from matching every direction at once, which cancelled horizontal and vertical movement and left the pointer centred.
- Moved joystick tracing to a dedicated task on the free core, so the video task cannot starve it and Serial output cannot disturb the bit-banged USB timing.
- Routed joystick tracing to the UART0 console used by the rest of the boot output, instead of the separate native USB serial port where it was invisible.
- Mirrored a single connected joystick's axes onto both CP400 joystick channels, so games work regardless of which joystick they read or which USB port the controller uses.
- Logged USB device enumeration with port number and vendor/product IDs, and the stored joystick mappings at boot.
- Replaced whole-report comparison with per-control bit masks so hat switches, buttons, and analog axes are matched independently.
- Excluded report bytes that change while the joystick is at rest, so counters and noisy axes no longer block button detection.
- Mapped either physical joystick button to the CP400's single fire input for that joystick.
- Added automatic detection of the standard Windows PlatformIO CLI installation.
- Corrected `.FLH` package checksums to match the F12 firmware validator.
- Fixed F12 firmware staging so stale or partial update files are replaced and verified before rebooting.

## 1.00 - 2026-08-17

### Added

- First tracked firmware release for the ESP32 CP400 emulator.
- Added CP400/CoCo 2 emulation with VGA output, USB keyboard and joystick input, SD-backed disk images, the F12 system menu, and SD-card firmware updates.