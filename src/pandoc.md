## HATE WYSIWYG?
#### WORDPRESS GOT YOU DOWN 🤷

---

## ️️TRY PANDOC 💪

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

    START([START]):::start-->MARKDOWN[/"🔤 MARKDOWN"/]:::input-->PYPANDOC[["🐍 PANDOC"]]-->HTML[/"🙌 HTML"/]:::output
    PYPANDOC-->PDF[/"✨ PDF"/]:::output
    PYPANDOC-->DOCX[/"📄 DOCX"/]:::output
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

# ✌ SUCCESS!