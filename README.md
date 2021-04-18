# transcheck

The purpose of this LaTeX package is to add additional checks to LaTeX files for translations which will alert the translator to missing or duplicate pages, equations, sections, or figures. Some other useful macros for translations are also part of the package.

I was motivated to make this package after noticing missing parts of translations I have produced and missing parts of even professionally produced translations.

This package uses the marginnote, ulem, and refcount packages. There is an additional optional macro, `\conversion{}`, which is only available if the babel package was previously called.

## Useful macros

- `\transcheck{}` underlines phrases which you are unsure of the translation of, and prints a warning in the log file.
- `\Transnote{}` can be used to add marked text from the translator with the phrase "Translator's note:" preceding the text.
- `\Transfootnote{}` creates a footnote starting with "Translator's note:".
- `\Transfootnotetext{}` creates a footnote without the associated footnote mark like `\footnotetext{}` does.
- `\conversion{}{}` uses the babel package to write *text in original language* = *text in language of translation*. This is useful to note what phrases mean in figures where it is inconvenient to edit the text in the image. E.g., `\conversion{german}{dimensionslose Zeit}{dimensionless time}` returns "*dimensionslose Zeit* = *dimensionless time*".

## Marking and checking page numbers

Frequently translations will put in the margin where new pages begin in the original document. This is a good practice for readers who may want to consult with the original. This practice is reinforced with this package by also keeping track of the *expected* original page number and comparing that with a page number entered by the translator. If the package expects the next page to be page 11, but the translator writes that they are starting page 13, likely page 12 was missed by accident.

Start the original page counter with the `\origpagestart{}` macro, e.g.:

> When`\origpagestart{10}` creating devices for the formation of ordered flows of monodisperse macroparticles by the method of forced capillary decay, it is necessary to be able to calculate the parameters of the formed drops.

To place the margin note at the right location, put the macro *after* the first word on a particular page.

When the page changes, put the `\origpage{}` macro after the first word of the new page, e.g.:

> Calculations show that the scatter of droplet`\origpage{12}` sizes increases with an increase in the wavenumber in the range $0.6 < k < 0.95$ (different from satellite) and decreases with an increase in the amplitude of the initial signal.

The `\origpage{}` macro will check that the reported page number matches that of the counter the transcheck package is keeping, and return an error if the pages do not match.

For original documents which used multiple columns, an optional first parameter can be used in `\origpagestart[]{}` and `\origpage[]{}`. to mark the column, however, this parameter is not checked at present. This parameter is zero by default (corresponding to no columns). 1 is the left column (represented by L in the margin note, e.g., p. 12L means page 2, left column). 2 is the center-left column (represented by CL in the margin note). 3 is the center column (represented by C in the margin note). 4 is the center-right column (represented by CR in the margin note). 5 is the right column (represented by R in the margin note). E.g.: `\origpage[1]{12}` corresponds to the left column of page 12.

## Checking equation numbers

The `\transeqlabel{}` macro replaces the `\label{}` macro used in math environments. The transcheck package expects the parameter of `\transeqlabel{}` to be the equation number. transcheck compares the expected equation number from LaTeX's counter with the equation number entered by the translator, returning an error if the two do not match.

These equations can be referenced with the label `eq:X` where X is the equation number.

For equations which do not have integer numbers, the `\transeqlabel{}` macro can be omitted. The checking is only done the `\transeqlabel{}` macro is present.

If an equation is missing from the original, you can use the `\MissingEq{}` macro to note that. That is, putting `\MissingEq{12}` in the LaTeX file after (preferably) equation 11 will increment the counter. For the moment this macro does not provide any checking so the parameter can be empty.

## Checking section numbers

The `\transseclabel{}` macro replaces the `\label{}` macro used in sections. The transcheck package expects the parameter of `\transseclabel{}` to be the section number. transcheck compares the expected section number from LaTeX's counter with the section number entered by the translator, returning an error if the two do not match.

These sections can be referenced with the label `sec:X` where X is the section number.

## Checking figure numbers and figure filename convention

The `\transfiglabel{}` macro replaces the `\label{}` macro used in figures. The transcheck package expects the parameter of `\transfiglabel{}` to be the section number. transcheck compares the expected figure number from LaTeX's counter with the figure number entered by the translator, returning an error if the two do not match.

These figures can be referenced with the label `fig:X` where X is the figure number.

The `\transfigimage{#1}{#2}{#3}{#4}` macro can be used to conveniently load figures if the filename of the figure is #1.#3, where #1 is the figure number and #3 is the filename extension. #2 is the figure caption. #4 defines the image width. To be more clear, this macro is simply the following:

```
\newcommand*{\transfigimage}[4]{\includegraphics[width=#4]{#1.#3}%
   \caption{#2}%
   \transfiglabel{#1}}
```
