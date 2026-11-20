# PARES — AI architecture for a historical archive

A reference design for turning scans of 16th-century documents into searchable historical records. Each published fact points to its supporting folio. The portal supports structured queries and access-controlled PDF exports.

## Architecture

- Ingest scans asynchronously. Use Tesseract for printed material and handwritten text recognition for manuscripts. Route uncertain dates or unsupported fields to archivist review.
- Record approved facts, citations and document versions in TimescaleDB (PostgreSQL). Use pgvector to retrieve related passages for context. The proposed deployment runs the application on AWS and uses managed TimescaleDB.
- Recheck current permissions and source versions before serving an answer or PDF. A withdrawn source makes an earlier export unavailable.

[Functional flow](functional-flow.svg) · [Technical architecture](technical-architecture.svg)

## Design decision: retrieving facts and citations

For a question about three ships in the catalogue, a similarity search might return passages about only two. The proposed query retrieves all eligible ships and their citations through SQL. Vector search adds context. If two folios disagree, the response shows both attributed claims. If a field is unsupported, the response marks it as unknown.

## How the design would be evaluated

Check field accuracy against human transcriptions, verify that each citation supports its claim, and test conflicting records, withdrawn sources and unauthorised exports. The design includes failure scenarios for interrupted uploads and duplicate ingestion.

