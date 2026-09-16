---
title  : Markdown
layout : default
parent : Coding is Life
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

> This notes is taken from [The Markdown Guide](https://www.markdownguide.org)!

## Basic Syntax

These are the elements outlined in John Gruber’s original design document. All Markdown applications support these elements.

1.  Heading

    ```
    # H1
    ## H2
    ### H3
    ```

1. Bold

    ```
    **bold text**
    ```

1. Italic

    ```
    *italicized text*
    ```

1. Blockquote

    ```
    > blockquote
    ```

1. Ordered List

    ```
    1. First item
    2. Second item
    3. Third item
    ```

1. Unordered List

    ```
    - First item
    - Second item
    - Third item
    ```

1. Code

    ```
    `code`
    ```

1. Horizontal Rule

    ```
    ---
    ```

1. Link

    ```
    [Markdown Guide](https://www.markdownguide.org)
    ```

1. Image

    ```
    ![alt text](https://www.markdownguide.org/assets/images/tux.png)
    ```

## Extended Syntax

These elements extend the basic syntax by adding additional features. Not all Markdown applications support these elements.

1. Table

    ```
    | Syntax | Description |
    | ----------- | ----------- |
    | Header | Title |
    | Paragraph | Text |
    ```

1. Fenced Code Block

    ```
    \```
    {
    "firstName": "John",
    "lastName": "Smith",
    "age": 25
    }
    \```
    ```

1. Footnote

    ```
    Here's a sentence with a footnote. [^1]

    [^1]: This is the footnote.
    ```

1.  Heading ID

    ```
    ### My Great Heading {#custom-id}
    ```

1. Definition List

    ```
    term
    : definition
    ```

1.  Strikethrough

    ```
    ~~The world is flat.~~
    ```

1. Task List

    ```
    - [x] Write the press release
    - [ ] Update the website
    - [ ] Contact the media
    ```

1. Emoji

    ```
    That is so funny! :joy:
    ```

    > See also [Copying and Pasting Emoji](https://www.markdownguide.org/extended-syntax/#copying-and-pasting-emoji)

1. Highlight

    ```
    I need to highlight these ==very important words==.
    ```

1.  Subscript

    ```
    H~2~O
    ```

1.  Superscript

    ```
    X^2^
    ```