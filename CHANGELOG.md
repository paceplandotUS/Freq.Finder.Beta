## Changelog

- 2026 02 15 (v260215.1200)
  - Documentation & UI
    - Refreshed the **Instructions** page with detailed explanations for recent features (DuckDB, Data Currency).
    - Added a persistent **Changelog** section to the instructions page, synchronized with the project's root `CHANGELOG.md`.
    - Clarified "Termination Pending" status labels and data currency definitions.

- 2026 02 14 (v260214.1600)
  - Database Migration
    - **DuckDB Engine Swap**: Migrated from SQLite to DuckDB for high-performance querying and native spatial support.
    - **Spatial Integration**: Integrated DuckDB Spatial extension for native point-in-polygon and distance filtering.
  - Bug Fixes & Refinement
    - Fixed a `NameError: name 'conditions' is not defined` in `query_builder.py` microwave path.
    - Sanitized Git history to remove accidental 1.4GB database commit.
    - Updated unit test suite to 8 tests covering microwave expiration logic.

- 2026 02 13 (v260213.fix)
  - Data Retrieval & Filtering Fixes
    - Fixed missing microwave paths by anchoring subquery on the Header table (`PUBACC_HD`), ensuring sites without frequency records (passive reflectors) are included.
    - Enhanced **Hybrid Geo-Filtering** to correctly preserve microwave paths crossing state lines by checking Radio Service codes.
    - Improved robustness of "Any" search type to properly handle mixed LMR and microwave results without accidental filtering.
  - Development & Verification
    - Implemented a comprehensive command-line unit test suite in `tests/`.
    - Created `setup_test_db.py` to generate rich mock data for complex edge cases (passive sites, multi-state paths).
    - Fixed a latent SQL join bug in the Cellular-Microwave query path identified during testing.

- 2026 01 19 (v260119.1345)
  - Search Filter Improvements
    - Added **Expiration Range Search** (30, 60, 90, 120 days).
    - Added **Termination Pending Filter** (Status 'X' licenses).
    - Implemented **Hybrid Geo-Filtering** for microwave queries to suppress nationwide LMR spam while preserving path links.
    - Enhanced data date tracking with a `metadata` table in the database.
  - Query Builder Refactoring
    - Fixed bug where status and expiration filters were ignored in "Type: Any" query paths.

- 2025 12 12 (v251212.1030)
  - Data Date Display
    - Updated the user-facing data date to reflect the actual date of the data source, rather than the date the maintenance process was run.

- 2025 12 09 (v251209.11530)
  - Interference Analysis
    - Implemented logic for calculating harmonics (causing) and sub-harmonics (receiving) up to 4th order
    - Added adjacent channel checks (+/- 12.5 kHz)
    - Created "Prime Suspect" feature to highlight exact harmonic matches (+/- 2.5 kHz) in bold red
    - Added diagnostic display of all searched frequencies
    - Widened search bandwidth to +/- 25 kHz
  - Documentation
    - Updated README and Instructions with Interference Analysis Guide

- 2025 11 25 (v251125.1630)
  - UI Improvements
    - Implemented independent scrolling for the results table
    - Added sticky headers to the results table for better usability with large datasets

- 2025 10 31 (v251031.1600)
  -  added CHANGELOG
  -  License dates
     - added effective, grant, and expiration dates to local license detail sheet
     - added sortable expiration date column to main results table