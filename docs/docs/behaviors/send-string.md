---
title: Send String Behavior
sidebar_label: Send String
---

## Summary

The send string behavior types a string of text when pressed.

## String Definition

Each string you want to send must be defined in your keymap, then bound to a key.

For example, the following behavior will type `Hello, world!` when triggered:

```dts
/ {
    behaviors {
        hello_world: hello_world {
            compatible = "zmk,behavior-send-string";
            label = "ZSS_hello_world";
            #binding-cells = <0>;
            text = "Hello, world!";
        };
    };
};
```

You can then bind the string to a key in your keymap by using its node label, which is the text before the colon (`:`), prefixed with an ampersand (`&`) (not to be confused with the `label` property, which must be set to a short, unique string which is not used by any other behavior). In the above example, the label is `&hello_world`:

```
    default_layer {
        bindings = <&hello_world>;
    };
```

The `text` property can contain almost any text, however some characters must be formatted specially due to the Devicetree file format, and some cannot be used at all due to bugs in Zephyr:

- Double quotes must be escaped with a backslash, e.g. `\"quoted text\"`.
- Non-ASCII characters can be used with a custom [character map](#creating-character-maps).
- `\n` (enter) and `\\` (backslash) cannot be used, as they will cause syntax errors.
- `\t` (tab) will be stripped from the string and do nothing.
- `\x08` can be used to press backspace.
- `\x7F` can be used to press delete.

:::caution Character Maps
After defining your string behaviors, you must also select a [character map](#character-maps) to use so that ZMK knows which key to press for each character in the string.
:::

### Convenience Macros

You can use the `ZMK_SEND_STRING(name, text)` macro to reduce the boilerplate when defining a new string.

The following is equivalent to the first example on this page:

```dts
/ {
    behaviors {
        ZMK_SEND_STRING(hello_world, "Hello, world!")
    };
};
```

You can also add a third parameter with extra properties such as [timing configuration](#timing-configuration) and [character map selection](#character-maps):

```dts
/ {
    behaviors {
        ZMK_SEND_STRING(hello_world, "Hello, world!",
            wait-ms = <15>;
            tap-ms = <15>;
        )
    };
};
```

### Timing Configuration

The wait time setting controls how long of a delay there is between key presses. The tap time setting controls how long each key is held. These default to 0 and 5 ms respectively, but they can be increased if strings aren't being typed correctly (at the cost of typing them more slowly).

You can set these timings globally in [your `.conf` file](../config/index.md) with the `ZMK_SEND_STRING_DEFAULT_WAIT_MS` and `ZMK_SEND_STRING_DEFAULT_TAP_MS` settings, e.g.:

```kconfig
ZMK_SEND_STRING_DEFAULT_WAIT_MS=15
ZMK_SEND_STRING_DEFAULT_TAP_MS=15
```

You can also set them per behavior with the `wait-ms` and `tap-ms` properties:

```dts
    ZMK_SEND_STRING(hello_world, "Hello, world!",
        wait-ms = <15>;
        tap-ms = <15>;
    )
```

### Behavior Queue Limit

Send string behaviors use an internal queue to handle each key press and release. Adding a send string behavior to your keymap will automatically set the size of the queue to 256, allowing for 128 characters to be queued.

To allow for longer strings, you can change the size of this queue via the `CONFIG_ZMK_BEHAVIORS_QUEUE_SIZE` setting in your configuration, [typically through your `.conf` file](../config/index.md). For example, `CONFIG_ZMK_BEHAVIORS_QUEUE_SIZE=512` would allow for a string of 256 characters.

## Character Maps

In order for ZMK to know which key to press to type each character, you must select a character map. ZMK provides one character map, `&charmap_us`, which maps the characters on a US keyboard layout. You can [create additional character maps](#creating-character-maps) to handle other keyboard layouts.

To set the character map to use by default, set the `zmk,character_map` chosen node:

```dts
/ {
    chosen {
        zmk,character_map = &charmap_us;
    };
};
```

You can also override this for individual send string behaviors with the `charmap` property:

```dts
/ {
    behaviors {
        ZMK_SEND_STRING(hello_world, "Hello, world!",
            charmap = <&charmap_us>;
        )
    };
};
```

:::note
Properties with a `-map` suffix have a special meaning in Zephyr, hence the abbreviated `charmap` property name.
:::

### Creating Character Maps

If you have your OS set to a non-US keyboard layout, you will need to create a matching character map.

Add a node to your keymap with the following properties:

```dts
    compatible = "zmk,character-map";
    behavior = <&kp>;
    map = <codepoint keycode>
        , <codepoint keycode>
        ...
        ;
```

The `behavior` property selects the behavior that the key codes will be sent to. This should typically be `&kp`.

The `map` property is a list of pairs of values. The first value in each pair is the Unicode [code point](https://en.wikipedia.org/wiki/Code_point) of a character, and the second value is the key code to send to `&kp` to type that character. Add a pair for every character that you want to use.

A character map for Dvorak might look like this (many characters are omitted from this example for brevity):

```dts
#include <dt-bindings/zmk/keys.h>

#define DV_A A
#define DV_B N
#define DV_C I
...

/ {
    charmap_dvorak: character_map_dvorak {
        compatible = "zmk,character-map";
        behavior = <&kp>;
        map = <0x08 BACKSPACE>
            , <0x20 SPACE>
            ...
            , <0x41 LS(DV_A)>
            , <0x42 LS(DV_B)>
            , <0x43 LS(DV_C)>
            ...
            , <0x5A LS(DV_Z)>
            ...
            , <0x61 DV_A>
            , <0x62 DV_B>
            , <0x63 DV_C>
            ...
            , <0x7A DV_Z>
            , <0x7B DV_LEFT_BRACE>
            , <0x7C DV_PIPE>
            , <0x7D DV_RIGHT_BRACE>
            , <0x7E DV_TILDE>
            , <0x7F DELETE>
            ;
    };
};
```

You can then select this character map by setting the chosen node to its node label:

```dts
/ {
    chosen {
        zmk,character_map = &charmap_dvorak;
    };
};
```

If you want different strings to use different character maps (for example if you have different layers for different OS keyboard layouts), you can set the `charmap` property on each send string behavior:

```dts
/ {
    chosen {
        zmk,character_map = &charmap_us;
    };

    behaviors {
        ZMK_SEND_STRING(hello_world_us, "Hello, world!")
        ZMK_SEND_STRING(hello_world_dv, "Hello, world!",
            charmap = <&charmap_dvorak>;
        )
    };
};
```

By default, any code point not in the character map will be ignored. If you add a `unicode-behavior` property to a character map which refers to a one-parameter behavior, then that behavior will be invoked with the unmapped code point as its parameter instead.

ZMK does not yet have a behavior for sending arbitrary Unicode characters, but when this is added, it could be used as follows:

```dts
&uni {
    // Unicode behavior configuration...
};

/ {
    charmap_dvorak: character_map_dvorak {
        compatible = "zmk,character-map";
        behavior = <&kp>;
        unicode-behavior = <&uni>;
        ...
    };
};
```
