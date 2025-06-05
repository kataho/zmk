---
title: Layer Behaviors
sidebar_label: Layers
---

## Summary

Often, you may want a certain key position to alter which layers are enabled, change the default layer, etc.
Below are the list of behaviors that can be used to activate and deactivate layers.

:::note
Multiple layers can be active at the same time and activating a layer will not deactivate layers higher up in the "layer stack".

Layer numbers start at 0 following the order they are defined in the keymap node, for example `&mo 3` would activate the 4th layer node defined in the keymap.
See [Layers](../index.mdx#layers) for more information.
:::

## Momentary Layer

The "momentary layer" behavior enables a layer while a certain key is pressed. Immediately upon
activation of the key, the layer is enabled, and immediately upon release of the key, the layer is disabled
again.

### Behavior Binding

- Reference: `&mo`
- Parameter: The layer number to enable while held, e.g. `1`

Example:

```dts
&mo 3
```

## Layer-Tap

The ["layer-tap" behavior](hold-tap.mdx#layer-tap) enables a layer when a key is held, and outputs a [key press](key-press.md) when the key is only tapped for a short time.
See linked documentation for details.

## To Layer

The "to layer" behavior enables a layer and disables _all_ other layers _except_ the default layer.

### Behavior Binding

- Reference: `&to`
- Parameter: The layer number to enable, e.g. `1`

Example:

```dts
&to 3
```

## Toggle Layer

The "toggle layer" behavior enables a layer if it is currently disabled, or disables it if enabled.

### Behavior Binding

- Reference: `&tog`
- Parameter: The layer number to enable/disable, e.g. `1`

Example:

```dts
&tog 3
```

### Configuration

#### Toggle mode

If you wish to ensure that a layer is toggled on or off specifically, rather than switching between the two states, then you can do so with the `toggle-mode` property.
Define a new behavior and assign `"on"` or `"off"` to `toggle-mode`:

```dts
/ {
    behaviors {
        tog_on: toggle_layer_on_only {
            compatible = "zmk,behavior-toggle-layer";
            #binding-cells = <1>;
            display-name = "Toggle Layer On";
            toggle-mode = "on";
        };
    };
};
```

You can then use `&tog_on` in place of `&tog` whenever you wish to only toggle a layer on, and not toggle it off. An `"off"` version of the behavior can be defined similarly.

## Conditional Layers

The "conditional layers" feature enables a particular layer when all layers in a specified set are active.
For more information, see [conditional layers](../features/conditional-layers.md).

## Default Layer

?> What is the default layer?

It is the first one you define on your keymap (unless changed using this behavior). It is _special_ in two aspects:

- It can't be disabled by other behaviors.
- It is the only one active when the board starts running.

This behavior allows configuring a different default layer, for example to test DVORAK while keeping QWERTY on another layer, or moving a couple keycodes around for Windows/Mac usage.

This setting is stored on a per-endpoint basis, so you can configure USB to use QWERTY, and the first BLE endpoint to use DVORAK.

The stored settings are read and applied when the keyboard boots (receives powers) and also when the selected endpoint changes.

### Behavior Binding

- Reference: `&df`
- Parameter: The layer number to set as default for current endpoint, e.g. `1`

Example:

```dts
&df DVORAK
```

For a keymap with:

```dts
#define QWERTY 0
#define DVORAK 1
```
