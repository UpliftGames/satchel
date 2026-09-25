<h1 align="center">
  <img src="docs/assets/favicon.svg" width="64">
  <br>
  Satchel
</h1>

<div align="center">

  [![CI](https://github.com/ryanlua/satchel/actions/workflows/ci.yml/badge.svg)](https://github.com/ryanlua/satchel/actions/workflows/ci.yml)
  [![GitHub Release](https://img.shields.io/github/v/release/ryanlua/satchel)](https://github.com/ryanlua/satchel/releases)
  [![Docs](https://img.shields.io/badge/docs-website-green)](https://satchel.luau.page/)
  [![Playground](https://img.shields.io/badge/playground-experience-blue)](https://www.roblox.com/join/iedaw)
  [![Discord](https://discord.com/api/guilds/1162303282002272359/widget.png)](https://discord.gg/N2KEnHzrsW)
  [![Mentioned in Awesome Roblox](https://awesome.re/mentioned-badge.svg)](https://github.com/awesome-roblox/awesome-roblox)

</div>

> [!WARNING]
> Satchel v1 is unmaintained to focus on v2 development. For a maintained alternative, see [Purse](https://purse.luau.page/).

Satchel is a modern open-source alternative to Roblox's default backpack. Satchel aims to be more customizable and easier to use than the default backpack while still having a "vanilla" feel. Installation of Satchel is as simple as dropping the module into your game and setting up a few properties if you like to customize it. It has a familiar feel and structure as to the default backpack for ease of use for both developers and players.

<img alt="Satchel on computer" src="assets/computer-thumbnail.png" style="width: 49%;"> <img alt="Satchel on computer with inventory open" src="assets/computer-inventory-thumbnail.png" style="width: 49%;">
<img alt="Satchel on mobile" src="assets/phone-thumbnail.png" style="width: 49%;"> <img alt="Satchel on mobile with inventory open" src="assets/phone-inventory-thumbnail.png" style="width: 49%;">

<https://github.com/ryanlua/satchel/assets/80087248/2cd3f164-6bf3-4c3b-a682-67a386f576d5>

## Documentation

See the [documentation site](https://satchel.luau.page) for more about Satchel. Find guides on how to get started, learn about the API, understand what Satchel is, and more.

If you see anything wrong, open a new [documentation issue](https://github.com/ryanlua/satchel/issues/new?template=documentation_issue.yml).

## Slot decorator (upliftgames fork)

`Satchel:SetSlotDecorator(decorator)` lets game code draw the hotbar and inventory slots. Satchel still owns the slot logic: filling and clearing, dragging and reordering, hotkeys, gamepad selection and search.

```luau
export type SlotState = {
	frame: TextButton, -- the slot's frame, mount your visuals in here
	tool: Tool?, -- nil when the slot is empty
	isEquipped: boolean,
	index: number,
	isHotbar: boolean,
}

Satchel:SetSlotDecorator(decorator: ((state: SlotState) -> (() -> ())?)?)
```

- Satchel calls the decorator whenever a slot's visual state may have changed: equip, unequip, a tool being added or removed, drag/swap/reorder, moving between hotbar and inventory, and changes to the tool's `Name` or its `TextureId`/`Title`/`ToolTip` attributes.
- Calls are keyed to the slot frame, not the tool. Tools move between frames, so redraw from `state` every time and don't cache anything per tool.
- The decorator must not yield. If it errors, Satchel `warn`s with a traceback and the backpack keeps working.
- The decorator can return a cleanup function. Satchel calls it before the next decorate call on the same slot, when the slot frame is destroyed, and when the decorator is replaced.
- While a decorator is set, Satchel hides its own slot background, equip border, tool icon and tool name. The slot number, tooltip, gamepad selection and drag border are still drawn by Satchel.
- Calling `SetSlotDecorator(fn)` decorates every existing slot right away. `SetSlotDecorator(nil)` puts the default visuals back.

```luau
Satchel:SetSlotDecorator(function(state)
	local backing = Instance.new("ImageLabel")
	backing.Size = UDim2.fromScale(1, 1)
	backing.BackgroundTransparency = 1
	backing.Image = if state.isEquipped then EQUIPPED_BACKING else BACKING
	backing.Parent = state.frame

	return function()
		backing:Destroy()
	end
end)
```

## Icon size (upliftgames fork)

`Satchel:SetIconSize(pixels, bufferPixels)` sets the size of the hotbar and inventory slots in pixels.

```luau
Satchel:SetIconSize(pixels: number?, bufferPixels: number?)
Satchel:GetIconSize(): (number, number) -- the size and gap in use
```

- `pixels` is the slot's width and height. `nil` restores the default: 60, or 100 on ten-foot interfaces (console).
- `bufferPixels` is the gap between slots and around the hotbar. By default it scales with the size, from 5px at the default size. For example, 80 gets a 7px gap. Pass a number to fix the gap instead.
- Values are rounded to whole pixels. `pixels` must be at least 1 and `bufferPixels` at least 0. Anything else, including `NaN` and infinity, raises an error.
- The change takes effect straight away, including while the inventory is open. Existing slots are resized and repositioned, and the inventory is laid out again above the hotbar.
- Fitting to the screen: a custom size is reduced, if needed, so the hotbar fits the screen width and the hotbar plus the open inventory fit below the topbar. The hotbar has 10 slots on desktop and 6 on phones. It is re-fitted when the screen changes size, such as a phone rotating. For example, on an 852×393 phone, 80 fits and 100 becomes 84. Use `GetIconSize` to read the size that is actually in use. The default size is never fitted, so it behaves as in 1.7.1.
- The slot decorator is called again after a resize. Size decorator visuals with `Scale`, such as `UDim2.fromScale(1, 1)`, so they fill the slot at any size.

```luau
Satchel:SetIconSize(80) -- bigger slots with a 7px gap
Satchel:SetIconSize(80, 10) -- bigger slots with a 10px gap
Satchel:SetIconSize(nil) -- back to the default
```

## Sponsors

Special thanks for our sponsors for supporting Satchel and it's future development. We distribute Satchel and provide updates for free, for anyone to use or modify.

<br>

<p align="center">
  <a href="https://dobig.com/" target=_blank>
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="docs/assets/sponsors/do-big-dark.png" height="60">
      <source media="(prefers-color-scheme: light)" srcset="docs/assets/sponsors/do-big-light.png" height="60">
      <img alt="Do Big Studios" src="docs/assets/sponsors/do-big-light.png" height="60">
    </picture>
    <br>
    Do Big Studios
  </a>
</p>

<p align="center">
  <br>
  <a href="https://github.com/5KFubi" target="_blank">
    <img src="https://github.com/5KFubi.png" width="64">
    <br>
    5KFubi
  </a>
</p>

<br>

[Become a sponsor](https://github.com/sponsors/ryanlua)

## Contributing

We welcome all contributions from the community. See the [contributing guidelines](.github/CONTRIBUTING.md) for details.

## License

Satchel is available under the Mozilla Public License 2.0 license. See [LICENSE.md](LICENSE.md) for details.
