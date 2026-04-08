---
title: Markdown Syntax Reference
layout: default
nav_order: 100
description: "This page introduces Markdown syntax used in Jekyll websites, including Front Matter, headings, attribute blocks, links, emphasis, and other syntax examples."
---

# Markdown Syntax Reference

This page introduces Markdown syntax used in Jekyll websites. These syntax examples are based on common usage in the Just the Docs theme.

## 1. Front Matter (YAML Header)

Front Matter is a YAML-formatted configuration block at the top of Jekyll pages, used to set page properties.

```yaml
---
title: Page Title
layout: Layout Name
nav_order: Navigation Order
description: "Page Description"
permalink: /custom-url-path
---
```

**Parameter Explanation:**
- `title`: Page title
- `layout`: Layout template to use (e.g., home, default, page, etc.)
- `nav_order`: Display order in navigation menu (lower numbers appear first)
- `description`: Page description, used for SEO and page summaries
- `permalink`: Custom URL path, overrides default filename-based path

## 2. Headings

Markdown uses `#` symbols to indicate heading levels, from `#` (level 1) to `######` (level 6).

```markdown
# Level 1 Heading
{: .fs-9 }

## Level 2 Heading
{: .fs-8 }

### Level 3 Heading
{: .fs-7 }
```

**Note:** The Just the Docs theme supports adding attribute blocks after headings to set CSS classes, such as `{: .fs-9 }` for setting font size.

## 3. Attribute Blocks

Attribute blocks are used to add CSS classes or other attributes to the preceding element.

```markdown
This is a text paragraph.
{: .fs-6 .fw-300 .text-red }

This is a paragraph.
{: .note }
```

**Common CSS Classes:**
- `.fs-9`, `.fs-8`, `.fs-7`, `.fs-6`: Font sizes
- `.fw-300`, `.fw-400`, `.fw-700`: Font weights
- `.text-red`, `.text-blue`: Text colors
- `.note`, `.warning`: Callout styles

## 4. Horizontal Rules

Create horizontal rules using three or more hyphens, asterisks, or underscores.

```markdown
---

***

___
```

## 5. Blockquotes

Use `>` symbol to create blockquotes.

```markdown
> This is a blockquote.
> It can span multiple lines.
>
> Empty lines separate quote paragraphs.
```

## 6. Links

### Basic Links
```markdown
[Link Text](https://example.com)
```

### Links with Titles
```markdown
[Link Text](https://example.com "Link Title")
```

### Reference-style Links
```markdown
This is a reference-style [example link][1].

[1]: https://example.com "Example Website"
```

## 7. Emphasis

### Italic
```markdown
*Italic Text*
_Italic Text_
```

### Bold
```markdown
**Bold Text**
__Bold Text__
```

### Bold and Italic
```markdown
***Bold and Italic Text***
___Bold and Italic Text___
```

## 8. Footnotes

Footnotes are used to add notes at the bottom of the page.

```markdown
This is text containing a footnote[^1].

[^1]: This is the footnote content, displayed at the bottom of the page.
```

## 9. Code

### Inline Code
```markdown
Use `code` to mark inline code.
```

### Code Blocks
Use three backticks to create code blocks, optionally specifying a language for syntax highlighting.

````markdown
```python
def hello_world():
    print("Hello, World!")
```
````

## 10. Lists

### Unordered Lists
```markdown
- Item One
- Item Two
  - Subitem One
  - Subitem Two
- Item Three
```

### Ordered Lists
```markdown
1. First Item
2. Second Item
   1. Subitem One
   2. Subitem Two
3. Third Item
```

## 11. Liquid Template Syntax

Jekyll supports the Liquid templating language, which can be used within Markdown.

```liquid
Current Year: {{ "now" | date: "%Y" }}

Site Title: {{ site.title }}

Page URL: {{ page.url }}
```

**Common Liquid Filters:**
- `| date: "%Y"`: Format date
- `| downcase`: Convert to lowercase
- `| upcase`: Convert to uppercase
- `| strip_html`: Remove HTML tags
- `| markdownify`: Convert Markdown to HTML

## 12. Callouts

The Just the Docs theme provides special callout syntax.

```markdown
{: .note }
This is a note callout.

{: .warning }
> This is a warning callout, often combined with blockquotes.
```

**Available Callout Types:**
- `.note`: Regular note
- `.warning`: Warning
- `.important`: Important note
- `.new`: New content note

## 13. Tables

```markdown
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |
```

## 14. Task Lists

```markdown
- [x] Completed Task
- [ ] Incomplete Task
- [ ] Another Incomplete Task
```

## 15. Definition Lists

```markdown
Term One
: Definition One

Term Two
: Definition Two
: Second Definition
```

## Best Practices

1. **Maintain Consistency**: Use the same Markdown style throughout the website
2. **Use Front Matter Appropriately**: Set appropriate title, description, and navigation order for each page
3. **Use Attribute Blocks Reasonably**: Utilize CSS classes provided by the theme to enhance visual effects
4. **Semantic Markup**: Use correct heading levels (don't skip levels)
5. **Accessibility**: Add alt text for images, provide meaningful text for links

## References

- [Markdown Official Documentation](https://daringfireball.net/projects/markdown/)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Just the Docs Theme Documentation](https://just-the-docs.com/)
- [Liquid Template Language](https://shopify.github.io/liquid/)
