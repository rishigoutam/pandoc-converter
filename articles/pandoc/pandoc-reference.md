---
title: Pandoc Markdown Syntax Reference
subtitle: With Tufte Pandoc CSS
author: Rishi Goutam
date: 'April 6, 2022'
abstract:
keywords:
subject:
description: A reference for Pandoc markdown syntax
---

<section>

We wish to write articles in Pandoc markdown, and publish as html5 files.

</section>

## File Conversion
Run the below to convert this markdown file (saved as `pandoc-reference.md`) to a `pandoc-reference.html`
```
pandoc -s --extract-media=media --from=markdown+fenced_code_blocks+backtick_code_blocks+fancy_lists+startnum+table_captions+simple_tables+multiline_tables+grid_tables+pipe_tables+pandoc_title_block+intraword_underscores+strikeout+superscript+subscript+inline_code_attributes+tex_math_dollars+link_attributes+implicit_figures+footnotes+inline_notes+emoji+task_lists+intraword_underscores  --to=html5 pandoc-reference.md -o pandoc-reference.html
```

We can check out the Pandoc extensions used in the [Pandoc Manual](https://pandoc.org/MANUAL.html)

## Tables

Note: We need a line break before a creating a table

This is a pipe table

| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |

Use `:` in the second row under the headers to tell markdown align columns 

| Right | Left | Default | Center |
|------:|:-----|---------|:------:|
|   12  |  12  |    12   |    12  |
|  123  |  123 |   123   |   123  |
|    1  |    1 |     1   |     1  |

This is a table with where we can be lazy by not manually specifying widths. We also omit pipes around the edges

fruit| price
-----|-----:
apple|2.05
pear|1.37
orange|3.09

## Font styles

Text can be __emphasized__ using two underscores. Text can also be **emphasized** with two asterisks `**`

Text can be ___italicized___ with three underscores `__`

A part of a word can be empha*sized* as well

~~Struck through~~ text using two tilde `~` symbols

Text can be [underlined]{.underline}

[We can]{.smallcaps} begin a paragraph with small caps.

Note: Use the below if we do not have the `.smallcaps` class (such as when using pandoc without the `--standalone` flag)

```<span style="font-variant:small-caps;">Small caps</span>```

We can create <mark>highlighted text</mark> using the `<mark>...</mark>` tag

We can create subscripts (H~2~O) as well as superscripts (X^2^). E.g., `H~2~0` and `X^2^`

## Lists
**Checklists**
Project milestones:

- [x] Create project
- [ ] Write project blog posts
- [ ] Share online

**Unordered list**
Some big technology firms:

* Amazon
* Microsoft
* Apple
* Google

**Ordered list**
Steps to create a data science project:

1. Clean and merge datasets
2. Conduct exploratory data analysis
3. Manipulate data to suit models and visualizations
4. Create predictive models
5. Visualize data
6. Present insights

**Fancy lists**

We can use Roman numeral or alphabetical lists. Enclose the list markers in parentheses, or follow the marker up with a `)` or `.`

(i) foo
(ii) bar
(iii) baz

Capital letters must have two spaces after the `)` or `.`

I.  foo
II.  bar
III.  baz

Lists can also start at a specific number or letter. Subsequent list markers do not need to be specified directly and `#.` can be used

c. charlie
#. delta
   iv) subfour
   #) subfive
   #) subsix
#. echo

**Definition lists**

Definitions can be created in markdown with optional inline formatting:

Recurrent neural network (RNN)
: A recurrent neural network is a class of artificial neural networks where connections between nodes form a directed or undirected graph along a temporal sequence

Long Short-Term Memory (LSTM)
: This is one definition of LSTM. A PhD in mathematics is needed to understand it.
: Here is a second definition for LSTM for a general audience. It has multiple paragraphs.

    ```python
    def lstm_model():
        ...
    ```
    
    Third paragraph of definition 2.
    
**Example lists**

Sequentially numbered examples can be placed in the document. They do not need to be in a single list. They do need to be separated from a paragraph by a line break.

The Indo-European languages are a language family native to the overwhelming majority of Europe, the Iranian Plateau, and the northern Indian subcontinent. The Indo-Iranian branch comprises of early languages such as:

(@sanskrit)  Sanskrit
(@)  Avestan (historically known as Zend)

These two languages have modern representatives (such as Hindustani and Persian, respectively). There are other early Indo-European languages such as:

(@)  Latin
(@)  Celtic

which have their own modern representatives (such as Portuguese and Irish, respectively)

Example lists can be named and referred to elsewhere in the document

An examination of (@sanskrit) will show that it arose in South Asia after its predecessor languages had diffused there from the northwest in the late Bronze Age

## Emojis

We :heart: emojis :joy:. Do not copy/paste an emoji (as that causes issues with creating a PDF from markdown via $\LaTeX$). Instead, use its shortcut like so: `:joy:`

[emojipedia.org](https://emojipedia.org/) is a great resource to copy/paste emojis from :smile:

## Footnotes and inline notes

Let's say we are writing about a technique I found on stackoverflow, medium, or web[^singlelinefootnote] article. We can link to it via a footnote. The footnote will show up at the bottom of the page.

[^singlelinefootnote]: Footnote syntax can be found [here](https://www.markdownguide.org/extended-syntax/#footnotes)

We can also have a longer footnote[^longfootnote] with multiple paragraphs and even code formatting. Scroll to the bottom or click the footnote superscript number to read the footnote. The footnote includes a link back to the original text

[^longfootnote]: Here's one with multiple paragraphs and code.
    Indent paragraphs to include them in the footnote.

    ```python
    def fancy_algo():
        ...
    ```

    Add as many paragraphs as you like. Note that we have a line break when we want to break up a paragraph

Notes can also be written inline.^[Inlines notes are easier to write, since
you don't have to pick an identifier and move down to type the
note.]

## Code
We can inline code `1+1` in a paragraph

We can also create a code block (this is transformed into an html `<pre>` tag). Wou can also specify the language used as well for syntax highlighting:

~~~{.python .numberLines}
def foo(someint: int) -> int:
    """
    returns 1 plus the input
    
    :param someint: an integer
    """
    return 1 + someint
~~~

## Images

Images can be created with a `width` attribute. Images are inline so they can be posted side-by-side by omitting a line break between two (or more) images

This works in most markdown formats but is not supported by Pandoc:
    
```![Memoji picture of Rishi Goutam](https://hackmd.io/_uploads/B1zQAX5X9.png =x250)```

We want to use this:

```![Memoji picture of Rishi Goutam](https://hackmd.io/_uploads/B1zQAX5X9.png){ width=250px }```
    
![Memoji picture of Rishi Goutam](https://hackmd.io/_uploads/B1zQAX5X9.png){ width=100px }
![Memoji picture of Rishi Goutam](https://hackmd.io/_uploads/B1zQAX5X9.png){ width=50px }
![Memoji picture of Rishi Goutam](https://hackmd.io/_uploads/B1zQAX5X9.png){ width=25px }

