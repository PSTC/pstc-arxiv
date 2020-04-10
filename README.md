# Practical Sized Typing for Coq

These are the source files for the arXiv submission of PSTC. The implementation can be found [here](https://github.com/ionathanch/coq/tree/dev).

## What is this mess of files in the root directory?

There are a bunch of things in place to satisfy arXiv's compilation of LaTeX and should _not_ be meddled with unless you know what you are doing (i.e. unless you are William).

### Coloured code snippets and the `minted` package

This paper uses `minted` for syntax-highlighted code snippets, which in turn uses the Python library Pygments. Therefore, compilation requires the `--shell-escape` flag, which is not set in arXiv (see [this](https://tex.stackexchange.com/q/280590/202877) for further details). The workaround is to compile with `\usepackage[finalizecache,cachedir=.]{minted}`, then change the package option to `frozencache`, and submit to arXiv with all the relevant files (`*.pygtex`, `*pygstyle`). For arXiv to be happy, these files _must_ be in the root directory, so _do not_ change the `cachedir` directory or move any of the `*.pyg*` files into a different directory. I know you'll really want to, Future Jonathan, to clean it all up, but you mustn't.

### Generating DOT graphs

The directed graph in Section 4.3 was generated using DOT, located in `figures/digraph.tex`. This also required the `--shell-escape` flag when compiling in Overleaf; this option is set using the `latexmkrc` file (see [here](https://www.overleaf.com/learn/latex/Articles/How_to_use_latexmkrc_with_Overleaf:_examples_and_techniques) for more details). But then I broke it somehow, and I decided to use a hardcoded PNG image since that specific example was unlikely to change. Which is just as well, since arXiv doesn't allow that flag anyway.

### No `hypertex` package by default

arXiv automatically adds `hypertex` to LaTeX submissions, which can intefere with packages that try to also add `hypertex`. The `00README.XXX` file tells arXiv to not add it; that file can also do other things that are listed [here](https://arxiv.org/help/00README).

### Bibliography

arXiv wants the `.bbl` file and not the `.bib` file. I'm also pretty sure that it has to be named `main.bbl`. 

## Before submitting to arXiv (‼️)

* Remove `review` option from document class
* Compile once with the `finalizecache` option for `minted`, just in case
* Make sure the `.bbl` file is being generated from the `.bib`
* Be sure to include _all_ files _except_ the following: `README.md`, `latexmkrc`, `main.bib`, anything excluded by `.gitignore`

## Administrivial TODOs

* Allow using `\varw` somehow (currently they aren't rendered at all).
* Change all uses of `bussproofs` judgements to `mathpartir` judgements.
* Generate the digraph in Section 4.3 using the DOT code.
