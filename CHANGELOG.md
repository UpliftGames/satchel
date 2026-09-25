# Changelog

Changes to the `upliftgames/satchel` fork.

## 1.8.0

### Added

- `Satchel:SetIconSize(pixels, bufferPixels)` sets the slot size of the hotbar and inventory at runtime, including while the inventory is open. `nil` restores the default (60, or 100 on ten-foot interfaces). The gap between slots scales with the size unless `bufferPixels` is given. See [Icon size](README.md#icon-size-upliftgames-fork).
- If a custom size would make the hotbar wider than the screen, or the hotbar and open inventory taller than the space below the topbar, the size is reduced until it fits. It is re-fitted when the screen size changes.
- `Satchel:GetIconSize()` returns the slot size and gap in use, after any fitting.
- Dragging, drop targets, hotkeys, gamepad selection, search and the slot decorator keep working after a resize. A resize in the middle of a drag or a gamepad select pulse doesn't leave the slot at its old size or position. The decorator is called again for every slot after a resize.

### Unchanged

- If `SetIconSize` is never called, the backpack is laid out exactly as in 1.7.1 on every platform. The default size is not fitted to the screen.
- The gamepad hints bar keeps its fixed height (60, or 95 on ten-foot interfaces). It sizes to its hint icons and text, not the slots. Its width still follows the hotbar.

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
