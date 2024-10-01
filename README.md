# DragonStorm97.github.io

## TODO:

- Content!
- "Run This Code" support?
  - Modify that to support more languages
  - maybe yoink default code rendering to pass attributes so we can add custom compilation flags, etc.
- Disqus/giscus discussion support
- https://gohugo.io/templates/embedded/#open-graph
- Add more archetypes
- Make headers collapsible
- Add "See Related" section https://gohugo.io/content-management/related/
- Add summary sections to frontmatter for pages
- Extra Project:
  - in-browser Clang:
    - https://github.com/binji/wasm-clang/tree/master
    - https://github.com/jprendes/emception
- maybe change wasm libs to use emscripten args "-MODULARIZE=1 EXPORTNAME='asdfasdf'"? (Try and get multiple wasm apps on the same page)
  - then have the wasm shortcode accept a module name as well

## Supported features

Code blocks:

```cpp
std::cout<<"Hello, World!"<<std::endl;
```

```md
Inline LaTeX: $\sigma(z) = \frac{1}{1 + e^{-z}}$
```

Inline LaTeX: $\sigma(z) = \frac{1}{1 + e^{-z}}$

Separate Paragraph LaTeX:

```md
$$F(\omega) = \int_{-\infty}^{\infty} f(t) e^{-j\omega t} \, dt$$
```

$$F(\omega) = \int_{-\infty}^{\infty} f(t) e^{-j\omega t} \, dt$$

Chemistry expressions with mhchem: `$H_2O$` $H_2O$

Mermaid Graphs:

```md
graph TD;
A-->B;
A-->C;
B-->D;
C-->D;
```

```mermaid
graph TD;
  A-->B;
  A-->C;
  B-->D;
  C-->D;
```
