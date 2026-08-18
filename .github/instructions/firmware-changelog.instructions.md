---
description: "Maintain the ESP32 CP400 firmware change log whenever firmware behavior, build output, configuration, or user-facing features change."
applyTo: "software/esp32_cp400_emulator/**"
---

# Firmware Change Log

For every incorporated firmware change, update `software/esp32_cp400_emulator/docs/log.md` in the same change set.

- Use the firmware version defined by `FW_VERSION` in `software/esp32_cp400_emulator/Makefile`, formatted as one major digit and two minor digits, for example `1.10`.
- Add concise, user-visible entries under `Added`, `Changed`, `Fixed`, or `Removed` as appropriate.
- Create a new version section when `FW_VERSION` changes; do not rewrite previously released sections.
- Include the release date when it is known. Use `Unreleased` when work targets a version that has not been released yet.
- Do not add entries for formatting-only edits or internal refactors with no firmware, build, configuration, or user-visible effect.