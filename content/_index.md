+++
title = 'CataByte'
date = 2024-09-12T16:50:57+02:00
draft = false
math = true
+++

This site is built with [hugo](https://gohugo.io/),
[hextra](https://imfing.github.io/hextra/) (a hugo theme),
and includes some pages with embedded WebAssembly built with [emscripten](https://emscripten.org/)

This is mostly a space for me to store my notes, and practice what I have learnt.

To learn more about me, you can visit the [About Me]({{< relref about.md >}}) page.

<!-- TODO: Learn more about this site here:-->
<!-- TODO: choose make a random selector for wasm showcases here instead:-->

## Example embedded WebAssembly

{{< wasm border_width=0 width="100%" src="https://cdn.jsdelivr.net/gh/DragonStorm97/minesweeper3d@release-artifacts-latest/release/raw/minesweeper3d/minesweeper3d.js" >}}

<!-- {{< wasm console=false width="100%" controls=false src="https://cdn.jsdelivr.net/gh/DragonStorm97/test_remote_wasm_load@release-artifacts-latest/release/raw/a/a.js" >}} -->
