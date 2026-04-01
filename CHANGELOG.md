## Changelog

- 2026 03 31 (v260331.2030)
  - RF Coverage Model Calibration & Performance
    - **Coverage Radius Bug Fix**: Discovered and resolved a critical UI state desync issue where the floating coverage widget would silently fall back to a 30km radius if the main panel was cleared, causing artificially small "tight circle" plots for hypothetical sites. The widget now correctly prioritizes its own radius parameter.
    - **ITM Variability Restored**: With the radius scaling bug fixed, restored the rigorous public safety design criteria (90% time, 90% locations, 95% situations). The model again produces highly conservative, realistic boundaries based on TIA TSB-88 standards rather than 50/50/50 median predictions.
    - **Dynamic Terrain Resolution**: Replaced the fixed 100-point elevation profile with dynamic scaling based on radius (one sample per ~300m, min 100 / max 1000 points). Previously, large radius requests (e.g., 300 km) spread 100 samples across 3 km intervals, smoothing out all terrain features.
    - **Terrain File Caching**: SRTM `.hgt` tiles are now cached in memory after first read, eliminating thousands of redundant disk I/O operations per coverage plot. Particularly beneficial for high-resolution profiles at large radii.
    - **UI Radius Limit**: Increased the maximum coverage radius input from 150 km to 500 km in both the main panel and floating queue widget.

- 2026 03 29 (v260329.1145)
  - Hypothetical RF Site Modeling
    - **Hypothetical Site Creation**: Users can now create "Test Sites" not present in the FCC database. New sites can be created by dropping a pin on the map or by cloning an existing FCC site.
    - **Persistent Test Site Library**: Added a new "Test Sites Library" panel to manage, edit, and export hypothetical sites. Data is saved locally in `user_test_sites.json`.
    - **Decoupled Coverage Workflow**: Hypothetical sites are treated as first-class citizens in the coverage queue, allowing modeling independently of FCC search results.
    - **Enhanced Map Visibility**: Added automatic transmitter markers (yellow pushpins) for hypothetical sites in both the live map and KML exports.
    - **Improved Interference Analysis**: Fixed a bug where interference parameters were lost during search, ensuring accurate frequency-based filtering.
    - **UI/UX Polishing**: Refactored the Test Sites form for better visibility on small screens and added descriptive labels to action icons.

- 2026 03 20 (v260320.2100)
  - Map-Based RF Coverage Site Selection
    - **Map-Based Coverage Selection**: Added a highly-requested feature enabling users to queue sites for RF coverage modeling directly from map marker popups using `+Cov` buttons.
    - **Floating Coverage Queue Widget**: Added a persistent overlay to track queued coverage sites. The widget includes inline tools to manage the queue and launch the coverage computation engine directly from the map.
    - **Custom Thresholds**: Added the ability to manually specify absolute signal threshold (dBm) targets for RF coverage contours beyond the standard presets, available in both the main panel and the floating widget.

- 2026 03 20 (v260320.1950)
  - Coverage Model Accuracy Overhaul (TIA TSB-88 / OET-69 Alignment)
    - **Directional Antenna Patterns**: Antenna gain offset is now applied per-bearing in the link budget. Directional antennas produce lobed (non-circular) coverage contours instead of omnidirectional circles.
    - **EIRP-to-ERP Correction**: Subtracts 2.15 dB when the license power source is EIRP, correcting the isotropic→dipole power reference used in the link budget.
    - **Bare-Earth ITM**: The Longley-Rice model now uses bare-earth SRTM terrain only. Clutter (vegetation, buildings) has been removed from the geometric profile per OET-69/TSB-88 guidelines, eliminating a "floating antenna" artifact.
    - **Path Clutter Loss**: Per-segment absorption loss values for midpath vegetation and urban clutter approximately doubled, with cap raised from 15→20 dB, to compensate for removing clutter from ITM geometry.
  - FCC Antenna Database Schema Fix
    - **AN Column Alignment (Critical)**: Corrected the `PUBACC_AN` column definition in `build_db_duck.py` from 27 columns to 37, matching the official FCC data format. Previously, every column from position 6 onward was mislabeled — beamwidth, gain, azimuth, and HAAT data existed in the database but was mapped to wrong field names.
    - **RA/AN Data Merge**: `get_site_coverage_params` now uses COALESCE to prefer RA (microwave/cellular) antenna data over AN (LMR) data, ensuring antenna pattern parameters are available across all service types.
    - **License Detail Improvements**: The Structure/Antenna table on the license detail page now displays site name, azimuth, beamwidth, gain, polarization, and HAAT from the corrected AN schema.
  - Nuanced RF Coverage Modeling (Heatmaps)
    - **Nuanced Contours**: Upgraded coverage modeling from a single boundary to tiered signal strength contours. Displays "Strong", "Medium", and "Fringe" tiers with distinct colors (Red, Yellow, Green).
    - **KML Export Fix**: Resolved a bug where tiered coverage features were being nested improperly, causing empty KML files. The exporter now correctly maps colors and signal level names for all contours.
    - **Scalable Evaluation**: Refactored the core Longley-Rice loop in `coverage_engine.py` to evaluate signal levels point-by-point along radials, preparing for future Land Use/Land Cover (LULC) clutter data integration.
    - **UI Transparency**: Adjusted coverage layer opacity (0.25) to allow visibility of underlying terrain and base map features.
    - **Dynamic Popups**: Updated map popups to reflect the specific signal level tier of each contour.

- 2026 03 19 (v260319.0700)
  - RF Coverage Modeling Feature
    - **New Feature (RF Coverage Modeling)**: Added terrain-aware RF coverage area estimation for LMR sites. Users can select up to 10 sites from search results and plot estimated coverage polygons on the map.
    - **ITM Propagation Model**: Integrated the `itmlogic` (Longley-Rice Irregular Terrain Model) library for point-to-point propagation modeling. Coverage polygons reflect actual terrain effects (ridges, valleys) using SRTM elevation data — not just free-space circles. Critical for use cases like ridge shielding analysis and frequency reuse across terrain features.
    - **UI Controls**: Added collapsible "RF Coverage Modeling" panel with configurable parameters: max radius (5–500 km), signal threshold (dBm), and receiver type (portable/mobile).
    - **Site Selection**: Added per-row checkboxes to the results table with a "Select All" option (capped at 10 sites). Plot Coverage button is dynamically enabled/disabled based on selection count.
    - **Coverage KML Export**: Added "Download Coverage KML" button that appears after plotting. KML includes coverage polygons (color-coded by frequency band) plus site pin placemarks for each transmitter site. Pins are colored to match band color.
    - **Bug Fix (Folium Map in iframe)**: Fixed JavaScript coverage rendering — Folium renders its map inside an `iframe` via `_repr_html_()`. Coverage layer is now correctly added by accessing the map through `iframe.contentWindow` and using the iframe's Leaflet instance.
    - **Bug Fix (Session Cookie Overflow)**: Replaced Flask session storage for coverage data with a server-side in-memory cache (`_coverage_cache`). GeoJSON coverage data (~11KB) exceeds the 4KB browser cookie limit, preventing KML export.
    - **Terrain Data Integration**: Coverage calculations use previously downloaded SRTM terrain tiles managed by `terrain_manager`. Falls back to Free Space Path Loss (FSPL) if terrain data is unavailable.

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