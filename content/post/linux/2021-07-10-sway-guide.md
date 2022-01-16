---
title: Sway Window Manager Guide
date: 2021-07-10T09:40:05+08:00
categories:
- linux
tags: 
- guides
summary: "What is Sway? Sway is a tiling window manager, which is one of the
simplest, most powerful desktop environments available."

draft: false

---

What is Sway? [Sway](https://swaywm.org/) is a tiling window manager, which is
one of the simplest, most powerful desktop environments available. It is a
keyboard-oriented interface where your applications are always 2 keystrokes away
from you.

To truly understand Sway, you need to understand the few modes that Sway has. 

- Tiling mode: the default mode. Applications are always tiled to maximise
  screen space
- Tabbed mode: applications take up the whole screen, with a tab-bar above
  applications
- Floating mode: applications can float above tiling area. Useful for
  applications where you need to bring up for a few seconds for reference and
  then put away. (refer to scratchpad section below)
- Stacking mode: You can nest tiling and tabbed modes in each other using stacking

Here is a list of Sway shortcuts. Note that Mod by default is your windows key.

```md
// applications
Mod + Enter -> open terminal
Mod + D -> open wofi to search and open applications
Mod + Shift + Q -> kill current focused application

// navigating within workspace
Mod + left/right/up/down -> move within workspace
Mod + h/j/k/l -> vim-based navigation 
Mod + Shift + left/right/h/j/k/l -> move applications within your workspace

// tiling options
Mod + W -> tabbed mode 
note: useful for many instances of the same application
Mod + S -> stack mode
Mod + E -> tiling mode 
note: repeat to re-tile in different orientation
Mod + B -> split horizontal (in tiling mode)
Mod + v -> split vertical (in tiling mode)
Mod + F -> fullscreen an application
Mod + Shift + Space -> floating mode
Mod + Space -> toggle between tiling area and floating area
Mod + drag with mouse -> to move floating windows with mouse

// scratchpad (a bag for holding windows)
Mod + Shift + Minus -> move current window to the scratchpad
Mod + Minus -> show the scratchpad 
note: repeat to cycle the windows of the scratchpad
Mod + Shift + Space -> send currently focused scratchpad to tiling area

// resize
Mod + R -> enter resize mode. Note: use arrow keys/hjkl to resize. 
You can also use your mouse to drag edges of floating windows

// navigate workspaces
Mod + <num> -> go to the num-th workspace
<num> is any number from 0 to 9
Mod + PageUp/PageDown -> move the the left/right workspace

// Custom utilities enabled by the Fedora Post-install guide
Shift + Print -> select and screenshot
Print -> screenshot whole screen
Brightness keys -> control brightness
Sound keys -> control sound

// sway
Mod + Shift + C -> reload sway to load new config
Mod + Shift + E -> quit sway
```
