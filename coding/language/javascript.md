---
title  : Javascript
layout : default
parent : Language
---

# {{ page.title }}
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Looping through object

```
Object.entries(obj).forEach(item => {
  let key = item[0];
  let value = item[1];
});
```

## Filter out empty item in list

```
let someArray = ['', 'a', 'b', '', 'c', ''];
let output = someArray.filter(item => item);

console.log(output);
// Expected output
// ['a', 'b', 'c']
```

