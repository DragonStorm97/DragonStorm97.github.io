+++
title = 'CataByte'
date = 2024-09-12T16:50:57+02:00
draft = false
math = true
+++

This site is built with [hugo](https://gohugo.io/),
[hextra](https://imfing.github.io/hextra/) (a hugo theme),
and includes some pages with embedded WASM built with [emscripten](https://emscripten.org/)

<!-- TODO: Learn more about this site here:-->
<!-- TODO: I... Learn more about me here:-->

## Example embedded WASM

<!-- {{< wasm border_width=2 height="500px" width="100px" console=false controls=false src="http://localhost:6931/a/a.js" >}} -->

{{< wasm console=false controls=false src="https://cdn.jsdelivr.net/gh/DragonStorm97/test_remote_wasm_load@release-files/release/a/a.js" >}}
