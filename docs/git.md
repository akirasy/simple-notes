---
layout : default
title  : Git Version Control
---

# {{ page.title }}
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Single file
Put the exact filename.
```
.DS_Store
```

## Directories
Include their paths and putting a `/` on the end.
If `/` is not present, it will match both files and directories with that name.
```
node_modules/
logs/
```

## Negation
Use a prefix of `!` to negate a file.
```
!example.log
```

## Double Asterisk
- `**` can be used to match any number of directories.<br>
- `**/logs` matches all files or directories named logs<br>
- `**/logs/*.log` matches all files ending with `.log` in a logs directory<br>
- `logs/**/*.log` matches all files ending with `.log` in the logs directory and any of its subdirectories.<br>
- `logs/**` matches all files inside of logs.

## Comments
Any lines that start with `#` are comments.
```
# macOS Files
.DS_Store
```
