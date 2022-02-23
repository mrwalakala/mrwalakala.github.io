---
layout: post
title:  "How to use a different editor to edit knowledge articles?"
date:   2022-02-21 21:12:13 +0100
category: ["ServiceNow", "Service Portal", "Knowledge"]
permalink: knowledge-article-different-editor

---

Sometimes TinyMCE (and the small window of an HTML field) are not the best way to edit a knowledge article.
Using the capabilities of the Service Portal it is possible to create a widget which, together with a WYSWYG editor, can make it easier to create or edit knowledge articles.

One of them is [CKEditor][ckeditor].

Here you can find a good list of main features [https://ckeditor.com/docs/ckeditor4/latest/features/index.html][ckeditor-features]

Some of them are:
- [Pasting Content from Microsoft Word][ckeditor-word]
- [Pasting Content from Microsoft Excel][ckeditor-excel]
- [Inserting Code Snippets][ckeditor-codesnippet]
- [Mathematical Formulas][ckeditor-math]
- [Real-time PDF Export][ckeditor-rtpdf]


This simple application adds a UI Action that allows you to edit the knowledge article in a dedicated page of the Service Portal --> [https://github.com/mrwalakala/sn-sp-ckeditor][github-link]

<img src="/assets/knowledge-article-different-editor-00.gif" alt="" />


[ckeditor]: https://ckeditor.com/docs/ckeditor4/latest/features/index.html
[ckeditor-features]: https://ckeditor.com/docs/ckeditor4/latest/features/index.html 
[ckeditor-word]: https://ckeditor.com/docs/ckeditor4/latest/features/pastefromword.html
[ckeditor-excel]: https://ckeditor.com/docs/ckeditor4/latest/features/pastefromexcel.html
[ckeditor-codesnippet]: https://ckeditor.com/docs/ckeditor4/latest/features/codesnippet.html
[ckeditor-math]: https://ckeditor.com/docs/ckeditor4/latest/features/mathjax.html
[ckeditor-rtpdf]: https://ckeditor.com/docs/ckeditor4/latest/features/pastefromword.html
[github-link]: https://github.com/mrwalakala/sn-sp-ckeditor