# [goutam.io](https://goutam.io) blog posts
This is a repository of personal project write-ups[^nycdsa] and blog posts for [goutam.io](https://goutam.io)

The workflow is made possible with [^pandoc]Pandoc and [^tufte-css]Tufte CSS.

### Generating articles from [`Pandoc markdown`](https://garrettgman.github.io/rmarkdown/authoring_pandoc_markdown.html)
There is a lot of benefit to having content written in markdown. Primarily, it allows one to spend less time customizing the look via a WYSIWYG editor such as Wordpress or Microsoft Office tools and more time actually writing. Additionally, it is simpler to use than <img src="svgs/LaTeX.svg" align=middle>. Although it took several days to set up this project, it was time well spent.

![Use of Time](./time-use.gif#gh-light-mode-only)
![Use of Time](./time-use.gif#gh-dark-mode-only)

```mermaid
pie 
    title Using Pandoc Markdown to Create Blog Posts
         "Time spent writing" : 80
         "Time spent tooling" : 15
         "Time spent styling" : 5
```

[^pandoc]: [Pandoc](https://pandoc.org/MANUAL.html) (and several filters) process the Pandoc markdown files. I am using [pypandoc](https://pypi.org/project/pypandoc/) as a Pandoc wrapper. Filters include:
    [pandoc-sidenote](https://github.com/jez/pandoc-sidenote)
    [mermaid-filter](https://github.com/raghur/mermaid-filter)
    [pandoc-plot](https://laurentrdc.github.io/pandoc-plot/MANUAL.html)
    [lua-filters](https://github.com/pandoc/lua-filters)

[^tufte-css]: See Dave Liepmann's [port](https://edwardtufte.github.io/tufte-css/) of Edward R. Tufte's style to CSS

[^nycdsa]: This repo contains blog posts cross-posted on [NYC Data Science](https://nycdatascience.com/blog/) as well as personal articles

### File Conversion Process

```mermaid
flowchart TB
    classDef input fill:aliceblue;
    classDef output fill:papayawhip;
    
    start(["Create *.md files\n \nPlace in sibling\n directory to src/"])
    
    markdown[/"🔤 my-article.md"/]:::input
    
    subgraph pypandoc ["🐍 pypandoc converter"]
        
    end
    
    html[/"🙌 my-article.html"/]:::output
    pdf[/"✨ my-article.pdf"/]:::output
    word[/"🤷 my-article.docx"/]:::output
    
    start-->markdown-->pypandoc-->html & pdf & word
```


