# Changelog

Changes to the `upliftgames/satchel` fork.

## 1.7.0

### Added

- `Satchel:SetSlotDecorator(decorator)` hands slot visuals to game code while Satchel keeps the slot logic (fill/clear, drag, reorder, hotkeys, gamepad, search). See [Slot decorator](README.md#slot-decorator-upliftgames-fork).
- Exported `SlotState` and `SlotDecorator` types.

### Unchanged

- If no decorator is set, slots look and behave the same as in 1.6.0.
