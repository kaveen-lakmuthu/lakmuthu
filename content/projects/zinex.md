---
title: "Zinex - external tiling layout generator"
description: "Zinex is an external tiling layout generator for the River Wayland compositor."
category: "Personal Systems Project"
status: "ONGOING"
github: "https://github.com/kaveen-lakmuthu/zinex"
last_updated: "2026-06-12"
date: "2026-06-12"
---

Written in Zig, Zinex connects directly to River using custom Wayland protocol interfaces, calculating window placements and managing client geometries dynamically in a frame-perfect manner.

Unlike traditional window managers that embed layout logic inside the core compositor, River delegates layout generation to external processes. Zinex acts as one of these processes, implementing layout algorithms and responding to display changes, window creations, and focus updates.
