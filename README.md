# cloud-itonami-lei-549300nvjuh16nxg8k87

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Scorpio Tankers Inc..**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**Scorpio Tankers Inc.**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: Scorpio Tankers Inc.
- **LEI (ISO 17442)**: [549300NVJUH16NXG8K87](https://search.gleif.org/#/record/549300NVJUH16NXG8K87) (GLEIF-verified)
- **Jurisdiction**: MH
- **Website**: https://www.scorpiotankers.com
- **Ticker**: STNG (NYSE)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 18 verified registry facts with per-fact provenance (9 about the
  entity, its registration, issuer and legal form; 9 one-per-ISIN). **Generated** —
  see below.
- `scripts/verify-facts.cljs` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljs           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljs --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO requests back the file (`CHECKED 11` when it was written,
2026-08-23T04:45Z, golden copy 2026-08-22T16:00Z) — the LEI record (legal name
`SCORPIO TANKERS INC.`, jurisdiction `MH`, entity **ACTIVE**, registration
**ISSUED** with the next renewal due 2027-07-24, last updated 2026-07-23,
`FULLY_CORROBORATED`, conformity flag `CONFORMING`; entity status and
registration status are different fields and are recorded separately), its
**9 ISINs**, read from `meta.pagination.total` of a single page — because the
whole list fits in that one page, each identifier is also mirrored as its own
`:security` entity (`MHY7542C1306`, `NO0013462630`, `US80918T3077`,
`US80918TAC36`, `US80918TAE91`, `US80918TAF66`, `US80918TAG40`, `US80918TAH23`,
`USY7542CAA46`) — its managing LOU and LEI-issuer accreditation (Ubisecure Oy,
LEI `529900T8BM49AURSDO55`, marketing name Ubisecure RapidLEI, accredited
2018-04-03), registration authority `RA000444` (Marshall Islands Maritime and
Corporate Administrators, registration number `36141`), ISO 20275 legal form
`DSII` (`Corporation`, MH), reporting exceptions at both consolidation levels
(`NATURAL_PERSONS` — GLEIF records no parent, direct or ultimate, because the
entity is controlled by natural persons rather than by a consolidating legal
entity), and **0 direct children**, read from `meta.pagination.total` of a
15-per-page request — a measured zero, not an unasked question. The
`direct-parent` and `ultimate-parent` endpoints answered `404` because GLEIF
publishes the exception side of that pair for this entity, which the checker
treats as a fact rather than a failure.

The checker's exit codes are three, not two: `0` every recorded fact matches the
live sources, `1` a citation broke or a fact drifted, `3` the check could not be
performed at all — an absent `facts.edn`, or every request failing at the
transport level. A check that could not run must not be indistinguishable from a
check that ran and found nothing, so it refuses to report a pass rather than
exiting 0. All outcomes were exercised before this landed: unmodified `0`;
`:registration/next-renewal-date` edited one year forward → `1` naming
`DRIFT gleif-lei-record :registration/next-renewal-date`; the
`gleif-isin-no0013462630` entity deleted → `1` naming
`ADDED gleif-isin-no0013462630`; both `:relationship/exception-reason` values
rewritten to `NON_CONSOLIDATING` → `1` naming `DRIFT` on both
`gleif-direct-parent-reporting-exception` and
`gleif-ultimate-parent-reporting-exception`; `:relationship/direct-child-count`
edited `0` → `1` → `1` naming `DRIFT gleif-direct-children-count
:relationship/direct-child-count`; the GLEIF host in the checker rewritten to an
unresolvable name → `3` (`INCONCLUSIVE … refusing to report a pass`).

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
