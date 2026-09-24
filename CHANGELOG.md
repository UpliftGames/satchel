# Changelog

Changes to the `upliftgames/satchel` fork.

## 1.7.1

### Fixed

- When a slot was cleared, the slot decorator was called while the slot still held the old tool. Clearing now notifies the decorator after the tool is removed, so `state.tool` is `nil`. This affected dragging a hotbar tool into the inventory, swaps, double-clicking to the hotbar, and tools being removed. Before the fix, the old decoration stayed on the slot until the backpack closed.
- `Satchel:GetSlotForTool(state.tool)` now returns the current slot when called from inside the decorator during a fill.

## 1.7.0

### Added

- `Satchel:SetSlotDecorator(decorator)` hands slot visuals to game code while Satchel keeps the slot logic (fill/clear, drag, reorder, hotkeys, gamepad, search). See [Slot decorator](README.md#slot-decorator-upliftgames-fork).
- Exported `SlotState` and `SlotDecorator` types.

### Unchanged

- If no decorator is set, slots look and behave the same as in 1.6.0.
