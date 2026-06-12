---
title: "HALO Calculator"
description: "A simple calculator that runs as a userland application in Halo OS"
category: "Personal Systems Project"
status: "ONGOING"
github: "https://github.com/kaveen-lakmuthu/halo-calc"
last_updated: "2026-06-12"
date: "2026-06-12"
---

A lightweight, modular calculator built in pure Rust. It is designed to be portable, separating the mathematical engine from the GUI implementation, making it ready for future porting to the Halo OS kernel environment.

### Features
* **Syntax Highlighting**: Automatically detects file types (Rust, TOML, JS, HTML, etc.) and highlights syntax.
* **Smart Highlighting**: Uses syntect for industry-standard parsing.
* **Zoom Control**: Ctrl + and Ctrl - to adjust font size dynamically.
* **Status Bar**: Real-time tracking of Cursor Position (Line/Col) and active Theme.
* **Theme Switcher**: Toggle between Solarized, GitHub, Dracula, and more instantly.
