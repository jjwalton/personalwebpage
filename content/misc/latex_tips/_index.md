# LaTeX Tips

I am no accomplished LaTeX guru: anything more complicated than writing basic equations sends me to a search engine --- inevitably then a nice Stack Exchange answer --- rather than trying to remember what I did last time. However there are some tips I wish I'd known about, or just implemented earlier: see in particular some simple (yet I reckon rarely used) organisational tips to [make course material easily updatable between years](#course-material-maintenance). I've also added some personal preferences that I think make LaTeX code a little easier to digest.

- [Syntax imposters](#syntax-imposters)
- [Characters are cheap](#characters-are-cheap)
- [Macros](#macros)
- [Folder structure](#folder-structure)
- [Course material maintenance](#course-material-maintenance)
- [Simple Beamer slides](#simple-beamer-slides)


Syntax imposters
---------------------

Some tex code produces the symbols you want when there's a preferred variant (which may produce subtle differences). As a result, I bet that in some cases below the suggestion may be the "approved" option without being the most commonly used!

The below produce marginal differences in spacing:

- `\colon` instead of `:`  e.g., `$f \colon X \to Y$` gives: $f \colon X \to Y$.
- `\mid` instead of `|` in set comprehension e.g., `$\{2x+3 \mid x<2\}$` gives $ \\{2x+3 \mid x<2\\} $. That looks a lot better than $ \\{2x+3 | x<2\\} $, doesn't it? And no one wants to be artificially adding white space everywhere by writing something like `\{2x+3 \ | \ x<2\}`.
- Using apostrophes or quotation marks as single and double quotations only gives closing ones, so it looks totally wrong: `'apostrophes'` gives $\text{'}\text{apostrophes}\text{'}$ and `"quotations"` gives $\text{"}\text{quotations}\text{"}$. You can fix this by replacing an opening apostrophy with a backtick `` ` `` (probably to the left of the 1 key on your keyboard), and an opening double quotation mark can be replaced with two backticks ``` `` ```, later followed by a closing pair of apostrophes `''`. There are also packages like `csquotes` which can adjust this for you whilst still using usual quotation marks in your editor.
- Some standard mathematical functions can be put into equations using a preceding backslash `/`. Writing `cos(x)` incorrectly italicises to $cos(x)$, whereas `\cos(x)` gives the better $\cos(x)$. The same goes for the other trig functions and their hyperbolic versions. Other useful ones include `\ln`, `\log`, `\det`, `\gcd`, `\inf`, `\sup`, `\lim`, `\hom`, `\ker`.
- `\coloneqq` instead of `:=` when defining an object, e.g., `\mathbb{T} \coloneqq \mathbb{R} / \mathbb{Z}`. This needs a package, such as `mathtools`.

On the topic of spacing, a space following a `.` is interpretted as the end of a sentence. We should let the compiler know if that's not the case, because the spacing is different, such as when we have a mid-sentence abbreviation like "Dr." or "e.g." (although some might say that an "e.g. " should always be an "e.g., " anyway, in which case the spacing issue won't occur):

- Use a trailing backslash `.\` after a `.` not ending a sentence. For example: `Dr.\ X`.

If you have a "tall" equation then the standard brackets will be too short:

- Adding an opening `\left` and then closing `\right` to your brackets lets them resize to what's inside. For example, `(2x-3) \left( \frac{y^2+1}{3z^3-2} \right)` produces $ \displaystyle (2x-3) \left( \frac{y^2+1}{3z^3-2} \right) $, which looks a lot better than $ \displaystyle (2x-3)(\frac{y^2+1}{3z^3-2}) $, just using `( ... )`. Newer versions of TeX also have a `\middle` which can be useful for resizing the middle pipe in set comprehension e.g., `\middle|` (see also `\mid` above). One can also use the `braket` package, which allows you to write something like `\Set{ ... | ... }`, which automatically resizes the outer braces as well as the middle pipe (it has macros for other, similar scenarios, including of course its namesake bra-ket notation).

There are various kinds of dashes which should be typeset differently, the hyphen, en-dash and em-dash:

- The shortest, the **hyphen**, uses a single `-`. These are used for double-barrelled names (e.g., `Swinnerton-Dyer`) or compound words (e.g., `double-barrelled`).

- The medium **en-dash** is typed `--`. It is used for ranges in both numbers (e.g., `See pages 6--19`) and when referring to a range of authors. For example, we would write `The Gauss--Bonnet Theorem` or `The Birch--Swinnerton-Dyer Conjecture`; in this case the length of dashes helpfully informs us that there are two (not one or three) mathematicians involved: Birch and Swinnerton–Dyer.

- Logically enough, for the longer **em-dash** we adjoin yet one more hyphen: `---`. They are those standard grammatical dashes used to break up sentences.

Writing display equations (as opposed to inline equations):

- For reasons that I never remember, it's preferable to use `\[ ... \]` rather than `$$ ... $$`. Certainly with the former, for particularly long equations I find it marginally easier to scope out the start and end of the equation.

- There is an analogous variant `\(...\)` instead of `$...$` for inline equations, although I never use it (maybe I should?).


Characters are cheap
---------------------

For me, cramped LaTeX code is a pain to read. It doesn't cost anything to add extra spaces or tabs, which can give the code some more room to breathe. Readability should be favoured over character count:

- I mentioned the use of `\[ ... \]` for display equations above. I personally like to put the brackets on their own lines so that the display equation has some space around it (as it will do in your compiled output):
   ```
   We consider the sum
   \[
   \sum_{n=1}^k P(n+4)^\ell
   \]
   for all $\ell > 0$.
   ```
- I also added unnecessary spaces in the above. Although I don't have a fixed convention for this, I do think this can help readability. For example,
   ```
   e^z=\lim_{n\to\infty}\left(1+\frac{z}{n}\right)^n
   ```
   feels pretty cramped to me. I can read the below more quickly:
   ```
   e^z = \lim_{n \to \infty} \left( 1 + \frac{z}{n} \right)^n
   ```
- Use whitespace when nesting commands and writing lists too. For example, adding tabs before `\item`s can help with readability:
  ```
  \begin{itemize}
  	\item Option A;
  	\item Option B;
  	\item or one of these options:
  	\begin{enumerate}
  		\item the option C(i)
  		\item the option C(ii)
  	\end{enumerate}
  \end{itemize}
  ```

Of course, when referring to numbered theorems, sections etc., you should use labels, so that any reordering won't mess up your references:

- If labelling a theorem, definition, lemma, corollary, proposition, equation, section, figure,... it's good to have a convention of starting your label with a corresponding tag: `\label{thm: main}`, `\label{def: fundamental group}`, `\label{lem: reparametrising}`, `\label{cor: A to B equivalence}`, `\label{prop: angle formula}`, `\label{eq: sum of powers}`, `\label{sec: Preliminaries} `, `\label{fig: Arnold's cat}`,... you get the picture. You can then refer back to your main theorem by writing something like `By Theorem \ref{thm: main}`. In most cases it'll save more time to be descriptive with the label compared to any neglible time saved by typing a few less characters. Indeed, many LaTeX editors bring up the list of labels you currently have when you start typing `\ref{...` and it's nice when you don't have to scroll back to the item to double-check it's the right one. The same goes for sensible labels for citations.

Macros
---------------------

I should probably use more macros. They can both improve readability and, of course, save time:

- Sometimes one decides that notation should be changed. You'll often save time by writing a `\newcommand{\name}{definition}` from the outset, rather than committing early (one can also use `\def...`). For example, if I wanted to denote a mathematical gadget by `\Gamma`, then it's a good idea to put `\newcommand{\gadget}{\Gamma}` in the preamble, and using `\gadget` within the body of my document instead of `\Gamma`. If I later decided I should have called all gadgets something else, say `G` then I just need to change the `\Gamma` within the newcommand to `G`, which will redenote all of my gadgets. This is easier than doing a 'replace all' (which could risk changing some Gammas accidently; perhaps I used the Gamma function somewhere, forgetting that I'd been using `\Gamma` for my gadgets). The other advantage is that `\gadget` is self-explanatory: this is referring to a gadget, whereas `\Gamma` is referring to a Greek letter. So there are idiomatic reasons to do this even if you fully expect to keep notation.

- It's common to have the following in the preamble: `\newcommand{\R}{\mathbb{R}}` (or `\def\R{\mathbb{R}}`) for $ \mathbb{R} $, and similarly `\C`, `\Z`, `\Q`, `\N` for $\mathbb{C}$, $\mathbb{Z}$, $\mathbb{Q}$ and $\mathbb{N}$. This is surely a case where shorter also happens to be easier to read.

- If you use a lot of vectors, bra-kets or matrices of some size, a good macro will likely be useful.

Folder structure
---------------------

Especially for bigger projects (see also [below](#course-material-maintenance) on course note recommendations), having a sensible folder/file structure can keep things more organised.

- When there are lots (or, in my opinion, even just *any*) figures, it makes sense to keep them in a separate folder. I usually have an `images` folder, which contains the images in pdf form, as well as an `svgs` sub-folder which contains the editable vector images, for me typically drawn in [Inkscape](https://inkscape.org/). When you include the figures into your paper (say, using `\includegraphics` or `\import`), you just need to point to the right file path by including the folder name, or by adding a `\graphicspath` in your preamble.

- Sometimes it makes sense to have a different tex file for each section. These can then be combined with the master tex file using `\input{filename}`, which basically copies everything from `filename.tex` into the corresponding place in the current document. One can also use `\include{filename}`, which clears the previous page before inclusion. One can also use `\includeonly{filename1, ... ,filenameN}`. This can be helpful for a large project (like a thesis or book), or if on a slow computer and you only want to work on a small section at a time. The `import` package can also be used, which can be more powerful (e.g., if you want nested importing).

- You can make your LaTeX folders look less cluttered by deleting the auxiliary and log files etc. This can be done automatically by the "Clean" tool of Texmaker when it's closed (found in preferences). This doesn't seem to work, though, if you open then tabs, and there are some arguments for not deleting these extra files anyway. Instead, you could change the output directories to keep things tidier. Or you can just not get too hung up on a few extra files being there...

Course material maintenance
---------------------

Courses can easily consist of 50 or more documents: workshop sheets, lecture notes/slides, problem sheets, their solutions, formula sheets, the booklet of course information,....

If any contain year-specific data then this forces an unnecessary and incredibly tedious waste of time scanning for such updates at the beginning of a new term. I don't intend to promote a "set it then forget it" approach of rarely updating course material, which we should try to improve on each iteration. But realistically, a lot of material will be re-used. Is fishing through for these simple changes each year (crossing fingers that none are missed) really the best way to motivate improvements to the material? Probably not.

I'm thinking of year-specific data like:
 * dates/times: of classes, tests, office hours; year of the course;
 * lecturer/course convenor details;
 * links/QR-codes: to live question sessions, the course's virtual online learning environments;
 * ...

Wouldn't it be great if there was a simple list of all of the data that might need to change between years, and editing this made the necessary updates to *all* other files? It's easy to do with LaTeX using `sty` files:

1. If upgrading an old set of materials, go through each tex file to find data which will (or merely might) need updating between iterations. If starting on a new set of notes, of course just do this from the outset: replace the year-dependent data with a descriptive command, like `\spring_exam_date` or `\lecturer_name`. These should then be defined in the preamble of the tex file: `\newcommand{\spring_exam_date}{30\textsuperscript{th} January 2020}` or `\newcommand{\lecturer_name}{Prof.\ Smartypants}`. Make sure everything still compiles.

2. Next, cut all of these `\newcommand`s (from all tex files of your course) into a *single* `sty` file; call it `variables.sty` or something. I tend to put this file in a subfolder `year_dependent` from the root folder of the course.

3. Your tex files need to know where to look for these commands. Add `\usepackage{../../year_dependent/variables}` to the preamble of each which needs access, where the number of `../` is just how many folders back you need to go to the root folder.

Voilà! Now at the start of the year, just update that small `sty` file and everything will recompile to the new version. Far more painless than scouring numerous files for outdated culprits, and it even only takes marginally longer to implement the first time (and hence pays big dividends already by the second).

Some further tips on course folder structure:

- Clearly it's cleaner to separate different components into their own folders, like a folder for "Workshop sheets" (or "Problems classes"), lecture notes/slides, exam files,... I'd also say that a new folder for each problem sheet, within a `problem_sheets` folder, is also easier to navigate.

- You'll probably have a lot of duplication in preambles: your slides (if in separate chapters) will have the same preambles, your problem sheets will have the same preambles, and so on. So why not reduce redundancy and have just one `sty` file for each? Put an `sty_static` folder in the root, and in there put a `type.sty` file for each type of resource. Then each problem sheet can start with `\usepackage{../../sty_static/problem_sheet}`, rather than multiple lines of the same copied preamble each time. This has two additional advantages: firstly, if for whatever reason one problem sheet has a unique property necessitating a change in the preamble, then this will be instantly apparent in its tex file (whose preamble will now only be a few lines). Secondly, if you decide you want to change the appearance of your problem sheets, you can do them all together by editing just one style file.

- Similarly, you'll surely be using the same macros for maths symbols (like using `\newcommand{\R}{\mathbb{R}}`, as mentioned above). Instead of copying these into *every* tex file, why not just collect them in a single `sty` file? You can call it `commands.sty`, or something similar, put it into the `sty_static` folder mentioned above, and then just put `\usepackage{../../sty_static/commands}` into each preamble rather than the full list of maths commands. Much neater.

Putting all of the above together, our course will have something like the following folder structure:
```
course
├──booklet
├──lecture_slides
│   │   └──images
│   │       └──svgs 
│   ├──1
│   ├──2
│   └──...
├──problem_sheets
│   ├──questions
│   │  ├──1
│   │  ├──2
│   │  └──...
│   └──solutions
│      ├──1
│      ├──2
│      └──...
├──sty_static
└──year_dependent
├──workshops
│   ├──sheets
│   │  ├──1
│   │  ├──2
│   │  └──...
│   └──solutions
│      ├──1
│      ├──2
│      └──...
├──...
```
In the above, `sty_static` holds the `sty` files for preambles and macros (such as for maths notation). The folder `year_dependent` contains everything we need to edit for a new year: the `variables.sty` file and maybe some individual files that might need updating (like images files for QR codes to student participation activities).

In case it helps a single soul, I've started a very basic [template](/templates/2020_course.zip) implementing the above.


Simple Beamer slides
---------------------

Maths talk/lecture slides are often made on Beamer. I regularly see the bottom navigation symbols displayed but have *never* seen them used:

- The (in my opinion, redundant) slide navigation symbols can be disabled with `\setbeamertemplate{navigation symbols}{}` in the preamble.

Purely personal preference: I prefer slides to be clean and minimal. I'd rather the important content be drawing the attention than a busy frame. Here's a [template](/templates/simple_slides.tex) for some simple, adaptable slides.

