## HATE WYSIWYG?
#### WORDPRESS GOT YOU DOWN ð¤·

---

## ï¸ï¸TRY PANDOC ðª

---
<div class="fragment" data-fragment-index="1"></div>
<div class="fragment" data-fragment-index="2"></div>
<div class="fragment" data-fragment-index="3">Write</div>
<div class="fragment" data-fragment-index="4">Convert</div>
<div class="fragment" data-fragment-index="5">Publish</div>

```mermaid
graph LR
    classDef start stroke-dasharray: 5 5
    classDef input stroke:#FFF,stroke-width:1px, fill:darkslateblue;
    classDef output stroke:#FFF,stroke-width:2px, fill:teal;

    START([START]):::start-->MARKDOWN[/"ð¤ MARKDOWN"/]:::input-->PYPANDOC[["ð PANDOC"]]-->HTML[/"ð HTML"/]:::output
    PYPANDOC-->PDF[/"â¨ PDF"/]:::output
    PYPANDOC-->DOCX[/"ð DOCX"/]:::output
```

```mermaid-animation
START 
START-->MARKDOWN MARKDOWN
MARKDOWN-->PYPANDOC PYPANDOC
PYPANDOC-->HTML HTML
PYPANDOC-->PDF PDF
PYPANDOC-->DOCX DOCX
```

---

# â SUCCESS!