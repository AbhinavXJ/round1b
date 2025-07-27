# approach_explanation.md

## Approach Explanation Persona-Driven Document Intelligence

We have built this project to solve the problem of helping someone quickly find the most relevant parts of a set of documents, based on who they are (the persona) and what they need to do (the job-to-be-done). The goal is to build an offline tool that works fast, uses only CPU, and stays within a limited size, all while giving smart, useful results.

To achieve this, we found a solution that avoids large, slow models and focuses on a lightweight, fast, and practical approach. It uses a small sentence embedding model called `all-MiniLM-L6-v2`, which gives good results and loads quickly. For reading PDF files, we have used `pdfplumber`, which is simple and fast for extracting text.

The process starts by reading a file called `input.json` that contains the persona and their task. From that, it creates a few short search phrases to figure out what kind of content would be useful for that person.

Next, the system reads through up to 5 PDF documents, scanning a limited number of pages in each (to save time). For every page, it checks how similar the page’s content is to the persona's need, using sentence embeddings and a bit of keyword matching.

If a page looks relevant, the system tries to find a suitable title from the first few lines. It also pulls out a short summary by picking the most useful sentences on that page.

At the end, it picks the top 5 most relevant sections across all documents. It saves all the results, including document names, page numbers, section titles, summaries, and metadata,  all into an output file.

The whole solution is packaged inside a Docker container so it can be run easily on any system. It works completely offline, finishes in under 60 seconds, and follows all the rules given in the challenge.

This approach focuses on keeping things simple, fast, and effective, while still giving results that are actually helpful and meaningful for the user.