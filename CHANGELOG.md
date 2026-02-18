## Changelog

- 2026 02 18 (v260218.1000clean)
  - Migration Cleanup & Test Suite
    - **Bug Fix (Cellular County Search)**: Fixed case-sensitive `LIKE` → `ILIKE` in `data_access.py` for cellular county queries. In DuckDB, `LIKE` is case-sensitive (unlike SQLite), so searching "fairfax" would miss "FAIRFAX".
    - **Bug Fix (NaN Coordinates)**: Fixed `dms_to_dd()` in `utils.py` to return 0.0 for NaN inputs instead of propagating NaN through coordinate calculations.
    - **Bug Fix (SDR++ Export)**: Fixed `generate_sdrpp_json()` to skip NaN frequencies instead of creating invalid bookmarks.
    - **Connection Leak Prevention**: Wrapped all DuckDB query functions and the license detail route in `try/finally conn.close()` to prevent connection leaks on errors.
    - **Dependency Cleanup**: Removed unused `SQLAlchemy` from `requirements.txt`, added `pytest`.
    - **Test Suite**: Built comprehensive pytest suite (53 tests) covering query logic, utils, exporters, and Flask routes.
    - **Housekeeping**: Archived old SQLite files (`data_access_sqlite.py`, `build_database.py`, etc.) to `archive/` folder.

- 2026 02 18 (v260218.0927fix)
  - DuckDB Migration & Stability Fixes
    - **Database Build Repair**: Fixed a critical bug in `build_db_duck.py` where 5 tables (`PUBACC_AN`, `PUBACC_CO`, `PUBACC_CP`, `PUBACC_LM`, `PUBACC_SF`) were silently dropped due to strict CSV parsing errors. Added `strict_mode=false` to restore data integrity.
    - **License Detail Crash Fix**: Resolved a 500 error in the license detail route caused by DuckDB returning tuples instead of dict-like Row objects. Switched to `fetchdf().to_dict()` conversion.
    - **Defensive Error Handling**: Added try/except wrappers to optional database queries (Control Points, History) so the detail page renders even if specific tables are missing.
    - **UI/Map Refinement**: Sanitized `None` and `NaN` display in map popups and license detail templates, ensuring microwave sites without frequency records display "N/A" or "—" gracefully.

- 2026 02 17 (v260217.2020fix)
  - Microwave & LMR Query Fixes
    - **Subquery Refactoring**: Fixed a bug where LMR-only results were over-filtered when "add microwave" was selected with the default ("Any") class.
    - **Case-Insensitive Matching**: Switched to `ILIKE` for licensee and county matching in DuckDB to prevent data loss due to casing mismatches.
    - **Improved Visibility**: Added `radio_service_code` to result sets to assist in differentiating LMR and Microwave records.
  - Verification & Stability
    - Enhanced reproduction scripts to provide detailed metrics and service code breakdowns.
    - Verified all 8 existing unit tests pass with the new query logic.


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