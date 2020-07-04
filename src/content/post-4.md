---
title: "Test LaTex"
date: "2020-06-04"
draft: false
path: "/blog/test-latex"
---

https://gist.github.com/a-rodin/fef3f543412d6e1ec5b6cf55bf197d7b

<img src="https://render.githubusercontent.com/render/math?math=%5Cbegin%7Balign*%7D%0Ay%20%3D%20y(x%2Ct)%20%26%3D%20A%20e%5E%7Bi%5Ctheta%7D%20%5C%5C%0A%26%3D%20A%20(%5Ccos%20%5Ctheta%20%2B%20i%20%5Csin%20%5Ctheta)%20%5C%5C%0A%26%3D%20A%20(%5Ccos(kx%20-%20%5Comega%20t)%20%2B%20i%20%5Csin(kx%20-%20%5Comega%20t))%20%5C%5C%0A%26%3D%20A%5Ccos(kx%20-%20%5Comega%20t)%20%2B%20i%20A%5Csin(kx%20-%20%5Comega%20t)%20%20%5C%5C%0A%26%3D%20A%5Ccos%20%5CBig(%5Cfrac%7B2%5Cpi%7D%7B%5Clambda%7Dx%20-%20%5Cfrac%7B2%5Cpi%20v%7D%7B%5Clambda%7D%20t%20%5CBig)%20%2B%20i%20A%5Csin%20%5CBig(%5Cfrac%7B2%5Cpi%7D%7B%5Clambda%7Dx%20-%20%5Cfrac%7B2%5Cpi%20v%7D%7B%5Clambda%7D%20t%20%5CBig)%20%20%5C%5C%0A%26%3D%20A%5Ccos%20%5Cfrac%7B2%5Cpi%7D%7B%5Clambda%7D%20(x%20-%20v%20t)%20%2B%20i%20A%5Csin%20%5Cfrac%7B2%5Cpi%7D%7B%5Clambda%7D%20(x%20-%20v%20t)%0A%5Cend%7Balign*%7D">

<img src="https://render.githubusercontent.com/render/math?math=%5Cbegin%7Balign*%7D%0Amau%20test%20aja%20cuy%20%5C%5C%0Ay%20%3D%202%5E%7B2%7D%20%5Ctimes%204x%0A%5Cend%7Balign*%7D">

## Problem
A lot of GitHub projects need to have pretty math formulas in READMEs, wikis or other markdown pages. The desired approach would be to just write inline LaTeX-style formulas like this:

```markdown
$e^{i \pi} = -1$
```

Unfortunately, GitHub does not support inline formulas. The issue is tracked [here](https://github.com/github/markup/issues/897).

## Investigation
However, it does support them in Jupyter notebooks, [scroll below](#file-notebook-ipynb) to see an example. One might question how does it work in notebooks. It turns out that instead of relying on [MathJax](https://www.mathjax.org/) as [nbviewer](https://github.com/jupyter/nbviewer) does, GitHub's notebooks renderer converts inline math to images with source URLs that look like this:

<a href="https://render.githubusercontent.com/render/math?math=e%5E%7Bi%20%5Cpi%7D%20%3D%20-1&mode=inline">`https://render.githubusercontent.com/render/math?math=e%5E%7Bi%20%5Cpi%7D%20%3D%20-1&mode=inline`</a>

Inspecting the URL, it is possible to notice that:

1. The `&mode=inline` part is not required, the URL still returns the same image without it:

    <a href="https://render.githubusercontent.com/render/math?math=e%5E%7Bi%20%5Cpi%7D%20%3D%20-1">`https://render.githubusercontent.com/render/math?math=e%5E%7Bi%20%5Cpi%7D%20%3D%20-1`</a>.
    
2. Modern browsers [encode URLs automatically](https://stackoverflow.com/a/4110523), thus this link would work as well:

    <a href="https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1">`https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1`</a>.

## Solution
So the solution would be to insert this code in Markdown files:

```markdown
<img src="https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1">
```
and such an image would be rendered as <img src="https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1">.

This is more clumsy than just `$e^{i \pi} = -1$`, but it is still possible to write the formula by hand directly to the Markdown file this way.

Note that this syntax for image insertion would not work because the URL contains spaces:

```markdown
![formula](https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1)
```

## Addition from 2019-10-30

I made a [small app](https://alexanderrodin.com/github-latex-markdown) that allows to generate Markdown code from LaTeX using the method described above automatically.
