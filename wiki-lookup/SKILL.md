---
name: wiki-lookup
description: "personal wiki, book reference, what did we learn about, knowledge base, wiki search, article lookup, find in wiki, book learnings, remembered reading"
tags:
  - research
  - knowledge-base
---

# Wiki Lookup — Personal Knowledge Base

## Triggers
personal wiki, book reference, what did we learn about, knowledge base, wiki search, article lookup, find in wiki, book learnings, remembered reading

## When To Use
When the task benefits from Spencer's personal knowledge base — book insights, article extracts, frameworks, or quotes.

## Query Commands

**Full-text search:**
```bash
node ~/clawd/tools/book-learnings/query.js "search terms"
```

**By book:**
```bash
node ~/clawd/tools/book-learnings/query.js --book "Book Title"
```

**By author:**
```bash
node ~/clawd/tools/book-learnings/query.js --author "Author Name"
```

**By topic:**
```bash
node ~/clawd/tools/book-learnings/query.js --topic "investing" --limit 10
```

## Wiki Files
Book entries on Glyph: `node ~/clawd/tools/book-learnings/find-glyph.js` then `/book-wiki/books/`
Article entries: same path under `/book-wiki/articles/`
Concepts: `/book-wiki/concepts/`
Master index: `/book-wiki/books-index.md`

## Rules
- Query FIRST, then cite — do not invent citations
- Include title and author when citing
- If nothing relevant, say so
