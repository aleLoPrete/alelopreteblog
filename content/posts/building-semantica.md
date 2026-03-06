+++
title = "Building Semantica: Search for Meaning in Markdown"
date = 2025-03-10
+++

I've recently come across the idea of **Retrieval-Augmented Generation (RAG)** and wanted to experiment with it. Since I had a backlog item to set up a local LLM for my notes and documents, I wanted to start implementing the **retrieval process** to improve the responses of the model I will use.

It was a fun project to build and requires just a few building blocks. I'm also excited about it because there's a lot of improvement to be made and therefore so much to learn.

This is how I assembled **Semantica**, a **local, FAISS-based search engine** for my markdown notes. It's still not very fast or efficient but it works great and, most importantly, it is fully local.

## What Is Semantic Search

Most search engines utilize keyword-matching at the core of their algorithm. Semantic search is a formal (mathematical) way of representing **meaning**. Instead of matching exact words, it looks at **context and intent** to find the most relevant results. It does this by converting text into **vectors** and finding similarities between them. This means you can search with natural language and get results that make sense, even if they don't contain the exact words you typed.

> This is a very simplistic look at semantics, from the perspective of a software engineer. Semantics is part of the vast and fascinating field of linguistics.

## Why Semantic Search

I've used Obsidian for the last three years as my primary knowledge base[^1]. This means I have thousands of notes spanning university classes, random ideas, and book notes.

The built-in fuzzy search of Obsidian is perfect for day-to-day productivity, and well-named notes are the building block of my collection.

Some notes, though, don't get reviewed for a long time, or were created and never opened again. I loved the idea of carrying out a semantic search for two main reasons:

- Finding something old, of which I forgot the title or even the existence
- Analysis of semantic relationships between notes — Obsidian lets you see links you created between notes, but I'd love automatic semantic linking. For personal notes this is not that useful, but for bigger databases with multiple authors, it could be extremely valuable for navigating the knowledge base.

## Tech Stack & Architecture

The pipeline is simple:

1. **Convert text and PDF into a vector representation**
2. **Build the index over the vector database**
3. **Perform similarity search over the index**

Tools used:

- **FAISS** — Facebook's vector search library for similarity search
- **SentenceTransformers** — To generate the vectorized database
- **Rich & Typer** — For a cleaner CLI

### Indexing Pipeline

1. Read all **Markdown & PDF** files from a folder
2. Extract text (and optionally YAML metadata) — since [Obsidian](https://obsidian.md) uses YAML frontmatter
3. Convert text to **embeddings** using SentenceTransformers from Hugging Face
4. Store embeddings in **FAISS** for efficient nearest-neighbor search
5. Save metadata (filenames, tags) for easy lookup

### Search Pipeline

1. Load the **FAISS index** and metadata
2. Embed the **search query** into the same vector space
3. Perform a **nearest-neighbor search**
4. Return the **top-K most similar notes**

### CLI

I built a simple CLI with Python's `Rich` and `Typer`. They look good but are a bit heavy — although definitely not the performance bottleneck.

## Next Steps

This was just the **first step** — the end goal is a **personalized GPT** to explore my own thoughts without relying on third-party cloud services.

Next steps:

- **Semantic analysis / semantic notes graph**
- **Make it a plugin for Obsidian**
- **Add local GPT**

[^1]: I don't love the term "second brain," but since it's well understood in productivity communities, I use it here for clarity.
