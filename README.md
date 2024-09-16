# DragonStorm97.github.io

## TODO:

- Content!
- "Run This Code" support?
  - Other Jonathan seems to have done it already: https://www.foonathan.net/2021/05/hugo-godbolt/
- Disqus/gh discussion support
- https://gohugo.io/templates/embedded/#open-graph!
- Add more archetypes
- Make headers collapsible

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
