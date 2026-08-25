# CHANGELOG

We use this CHANGELOG to document breaking changes, bug fixes, and config value changes that affect the WA build. Changes to the Nexstrain build as a whole are logged by the Nextstrain team in the repo root: CHANGELOG.md.

## 2026

* 25 Aug 2026: Increased min length for genome build from 7000 to 9500. After accounting for other filtering, this drops 2 additional sequences.

* 05 Aug 2026: Implemented Contextual subsampling. Results in the following changes to seqeunce counts:
  *  E1 build: -73 seqs from date filter... and -1,906 global seqs (mostly Asia), -0 North American seqs, -24 USA seqs, -0 WA seqs
  *  Genome build: -9 seqs from date filter... all other samples retained
