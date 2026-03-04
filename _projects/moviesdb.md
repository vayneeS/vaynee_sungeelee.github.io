---
layout: single
title: "MoviesDB - IMDb Data Engineering & Analytics (In progress)"
excerpt: "A full data pipeline from raw IMDb files to an interactive graph exploration app."
header:
  overlay_color: "#1a4a3a"
  overlay_filter: 0.6
toc: true
toc_label: "Contents"
---

<p class="notice--info">
  <strong>Stack:</strong> Python · MySQL · Docker · Neo4j · Streamlit · SQL
</p>

## Overview

MoviesDB transforms raw IMDb data into a clean, queryable database exposed through an interactive graph exploration interface.

The pipeline runs end-to-end: raw TSV files → relational database → graph database → Streamlit UI.

**Primary objectives:**
- Data prep & ingestion: Ingest and structure the IMDb dataset into a relational MySQL database
- Write SQL queries to extract and transform subsets of interest (e.g., Marvel films)
- No-Code database: Import structured data into Neo4j to leverage graph queries for relationship exploration
- Visualization: Build a Streamlit app for interactive visualization of movie-people relationships
- Testing and automation: Experiment with different services / frameworks using data pipeline to test flexibility and benchmark performance
- Develop CI/CD pipelines to test an ddeploy code after testing
---

## Architecture

| Stage | Tool | Rationale |
|---|---|---|
| Ingestion & storage | MySQL + Docker | Normalization, integrity constraints, efficient joins |
| Extraction & transformation | SQL | Structured filtering, aggregation, subset definition |
| Relationship modeling | Neo4j Aura | Multi-hop queries, graph traversal |
| Interactive front end | Streamlit | Lightweight UI, no heavy web overhead |

---

## Database Schema

| Table | Key columns |
|---|---|
| `titles` | `title_id`, `primary_title`, `genres`, `start_year` |
| `names` | `person_id`, `person_name` |
| `principals` | `title_id`, `person_id`, `job`, `category` |
| `ratings` | `title_id`, `average_rating`, `num_votes` |
| `characters` | `character_name`, `person_name`, `person_id` |

---

## SQL Analytics Layer

```sql
SELECT t.primaryTitle, r.averageRating
FROM titles t
JOIN ratings r ON t.tconst = r.tconst
WHERE t.genres LIKE '%Action%'
ORDER BY r.averageRating DESC;
```

The SQL layer handles complex joins, aggregations, franchise filtering, and clean CSV export for Neo4j.

---

## Future Improvements

- **Marvel title extraction** - scrape the Marvel wiki to build a complete title list and join with the Movies table for precise franchise filtering

- **dbt integration** - separate SQL transformation logic from infrastructure, benchmark performance