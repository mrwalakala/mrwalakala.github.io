---
layout: post
title:  "How to fill PDF Template from server-side?"
date:   2022-02-14 19:12:29 +0100
category: ["ServiceNow", "PDF"]
permalink: pdf-generation-server-side

---

There are many ways to generate PDFs via the server side but only recently did I discover the possibility of being able to populate an existing PDF template.
This can be useful in the case of graphically complex PDFs to be generated entirely in ServiceNow.

The JavaScript class to be used is [PDFGenerationAPI][pdf-generation-api].
In particular, the [fillDocumentFields][fill-documents-fields] method allows a filled version of a PDF to be saved on a record.

Around this method I have created this simple scope application that allows you to:
- Define a PDF template per table
- Using a dynamic mapping between PDF fields and fields on the table
- Create the filled PDF file via UI Action on Workspace/NextExperience/UI16

Here the link to the repository  --> [https://github.com/mrwalakala/sn-pdfgeneration][github-link]

<img src="/assets/pdf-generation-server-side-00.gif" alt="" />


[pdf-generation-api]: https://developer.servicenow.com/dev.do#!/reference/api/sandiego/server/sn_pdfgeneratorutils-namespace/PDFGenerationAPIBothAPI
[fill-documents-fields]: https://developer.servicenow.com/dev.do#!/reference/api/sandiego/server/sn_pdfgeneratorutils-namespace/PDFGenerationAPIBothAPI#P-fillDocumentFields_O_S_S_S_S
[github-link]: https://github.com/mrwalakala/sn-pdfgeneration