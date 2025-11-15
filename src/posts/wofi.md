---
title: Configuring Wofi, the natural replacement for rofi on Wayland
created: 2025-11-15
---

Wofi is a program launcher for wlroots based Wayland compositors, like Sway. It's the natural replacement for [rofi](https://github.com/davatorium/rofi) on Xorg.

> Ensure you have `wofi` installed. It's probably located in your distros core/extra package repository.

## Set as application launcher

Where to place this depends on your window manager of choice. I'm currently on Sway so for that it's in `$HOME/.config/swag/config`:

```shell
set $menu wofi --show drun

bindsym $mod+d exec $menu
```

## Styling

Styling the menu is really simple. Create a file at `$HOME/.config/wofi/style.css` and open it in a text editor.

We can apply some basic styling to the menu by targeting the `window` element and `#input` class. This will leave us with a pretty minimal base menu.

```css
window {
  font-size: 14px;
  font-family: "JetBrainsMono Nerd Font";
  background: rgba(50, 50, 50, 0.9);
  border-radius: 4px;
}

#input {
  background: rgba(50, 50, 50, 0.9);
  color: #fff;
  border: none;
  border-radius: 4px 4px 0 0;
  box-shadow: none;
}
```

Styling the entries in the list can be done by targeting the `#entry` class. If you want to only apply styles to the currently selected item, you can use the `:selected` selector.

```css
#entry {
  padding: 0.4rem;
  color: #fff;
}

#entry:selected {
  background: linear-gradient(90deg, #bbffdd, #dd77ff);
  color: #333;
  outline: none;
}
```

But, as you can probably see, the text on the selected item is still white. We can fix this by targeting `#text:selected`. The color above only applies to, for example, the arrow to the left of the entry (indicating that an application is already open).

```css
#text:selected {
  color: #333;
}
```

Here you can also add other text-specific styling, like font weight:

```diff
#text:selected {
    color: #333;
+   font-weight: bold;
}
```

> For a full reference of all CSS targets, you can check the **WIDGET LAYOUT** section on the wofi(7) man page.

## Config file

Out of the box, Wofi works great by just running it with `--show drun`. But behaviour can be further customized using the `$HOME/.config/wofi/config` file:

```shell
menu=drun
```

For this do get respected though we'll need to go back to where you invoke the `wofi` command and remove the `drun` option:

```diff
In $HOME/.config/sway/config

- set $menu wofi --show drun
+ set $menu wofi --show
```

In this file you can also configure the size of the window. I personally think the default window is too large, so I'll set it to something smaller:

```diff
menu=drun
+width=600
+height=300
```

## Application images

It's possible to show an image for each entry. In your config file, add the following two options:

```shell
allow_images=true
image_size=20
```

Styling of the images can then be adjusted in the CSS config file:

```css
image {
  margin-right: 0.25rem;
}
```
