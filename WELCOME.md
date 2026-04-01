# 👋 Welcome to Freq. Finder

> **You're looking at the quick-start guide for users running the PyInstaller desktop build.**
> If you're a developer setting up from source, see [README.md](README.md) instead.

**Freq. Finder** is an offline-first desktop tool for querying and visualizing FCC radio license data from the Universal Licensing System (ULS). It runs entirely on your computer — no cloud account, no subscription, no internet required after initial setup (except to sync new FCC data).

---

## Table of Contents

1. [Step 1 — Download & Install](#step-1--download--install)
2. [Step 2 — Windows Defender & Antivirus Warnings ⚠️](#step-2--windows-defender--antivirus-warnings)
3. [Step 3 — First Launch](#step-3--first-launch)
4. [Step 4 — Data Maintenance (FCC Sync + Terrain)](#step-4--data-maintenance-fcc-sync--terrain)
5. [Step 5 — Your First Query (800 MHz Interop in your state)](#step-5--your-first-query-800-mhz-interop-in-your-state)
6. [Step 6 — Working with the Map & Results](#step-6--working-with-the-map--results)
7. [Step 7 — RF Coverage Modeling](#step-7--rf-coverage-modeling)
8. [Step 8 — Hypothetical (Test) Sites](#step-8--hypothetical-test-sites)
9. [Tips & Troubleshooting](#tips--troubleshooting)
10. [Feedback](#feedback)

---

## Step 1 — Download & Install

### Find the Release on GitHub

All builds are hosted in the **[Freq.Finder.Beta repository](https://github.com/paceplandotUS/Freq.Finder.Beta/tree/main)**. Look for the two `.zip` files near the top of the file list:

| Platform | File |
|---|---|
| 🪟 **Windows (64-bit)** | `Freq.Finder.Beta.XXXXXX.XXXX.Windows.zip` |
| 🍎 **macOS (Apple Silicon / Arm64)** | `Freq.Finder.Beta.XXXXXX.XXXX.Mac(Arm64).zip` |

> **Don't see a download button?** Click the filename, then look for the **Download raw file** button (a downward-arrow icon) on the right side of the page.

---

### Extract and Place the Application

> [!CAUTION]
> **Do NOT run Freq. Finder directly from your Downloads folder.** Windows and macOS both sandbox applications run from Downloads. Always move it to a proper location first.

**Windows — Recommended location: `C:\Freq.Finder\` or wherever you like to put things :)**

1. Open **File Explorer** and navigate to your `Downloads` folder.
2. Right-click the `.zip` file → **Extract All…**
3. In the destination box, type `C:\Freq.Finder` and click **Extract**.
4. Your application folder is now at `C:\Freq.Finder\FreqFinder\`. Inside you'll find **`FreqFinder.exe`**.

**macOS — Recommended location: `~/Freq.Finder/` (your home directory)**

1. Open **Finder** and go to your `Downloads` folder.
2. Double-click the `.zip` file — macOS will extract it automatically.
3. Open a new Finder window and press `Cmd+Shift+H` to go to your Home folder.
4. Create a new folder called `Freq.Finder` and drag the extracted `FreqFinder` application folder into it.

---

## Step 2 — Windows Defender & Antivirus Warnings

> [!WARNING]
> **Freq. Finder is NOT code-signed.** Obtaining a commercial software signing certificate is expensive, and as a free community tool we haven't gotten one yet. This means Windows SmartScreen and some antivirus tools will flag the application on first launch. **This is expected.** The code is open-source and available for inspection.

### On Windows — SmartScreen "Windows protected your PC"

When you first double-click `FreqFinder.exe`, you may see a blue dialog that says **"Windows protected your PC."**

1. Click **"More info"** (a link that appears below the warning text).
2. A new button appears: **"Run anyway"** — click it.
3. The app will launch normally. You will only see this once per binary version.

### On Windows — Windows Defender / Antivirus Quarantine

Some aggressive antivirus configurations may quarantine `FreqFinder.exe` immediately after extraction. If the `.exe` disappears after unzipping:

1. Open your antivirus application and look in its **Quarantine** or **Threat History** section.
2. **Restore** the file and mark it as a **trusted exception**.
3. Alternatively, add `C:\Freq.Finder\` to your antivirus's **exclusions list** before extracting.

### On macOS — "App is damaged" or "from unidentified developer"

macOS Gatekeeper may block the app. Open **Terminal** and run:

```bash
xattr -dr com.apple.quarantine ~/Freq.Finder/FreqFinder/FreqFinder
```

Then try launching again.

---

## Step 3 — First Launch

**Windows:** Double-click `C:\Freq.Finder\FreqFinder\FreqFinder.exe`

**macOS:** Double-click the `FreqFinder` executable inside your `~/Freq.Finder/FreqFinder/` folder.

A **black console window** will appear — this is normal. It shows the internal server starting up. Leave it running in the background; closing it will shut down the application.

After a few seconds, your **default web browser** will (or should) automatically open to:

> **http://127.0.0.1:7223**

If it doesn't open automatically, copy and paste that address into your browser.

> [!NOTE]
> Freq. Finder is a web application that runs locally on your computer. The browser is just the user interface — no internet connection is used to display the app (though FCC data syncing requires internet). Maps will be a bit wonky without internet as well, though once you've viewed something it'll be cached. 

---

## Step 4 — Data Maintenance (FCC Sync + Terrain)

Before your first real search, the app needs two things: **FCC license data** and **terrain tiles** for your area. Both are managed from the **Data Maintenance** page.

### Getting There

On the main page, look at the top-left area under the **Freq. Finder** heading. Click the blue **"Data Maintenance →"** link.

---

### Part A: FCC License Data Sync

At the top of the Data Maintenance page, you'll see a single blue button: **"Start Data Update"**.

> [!CAUTION]
> **This process can take a very long time — anywhere from 20 minutes to a few hours depending the FCC's site capacity...Frankly it's often QUITE slow.** The FCC's weekly database dump is enormous. The download log will show you exactly what's happening line by line. Let it run to completion before searching.

1. Click **"Start Data Update"**.
2. A dark console log will fill the screen — you'll see messages about downloading FCC zip files, extracting them, and importing them into the local database.
3. When the process finishes, the log will show a completion message and the button will re-enable.

**What it's downloading:** Full components of the FCC's Universal Licensing System (ULS) weekly export — every radio license in the country across all services (LMR, microwave, cellular, etc.). It's not a huge set of files, but it's not tiny either. 

---

### Part B: Terrain Data (SRTM Elevation Tiles)

Terrain data is what makes the **RF coverage modeling** feature accurate. Without it, coverage estimates fall back to simple circles (free-space path loss). With terrain data, the model traces every hill, ridge, and valley.

In the **"Terrain Data Download"** section:

1. Find the **"Select States to Download"** list box (it shows all 50 states).
2. Click your state(s). Hold **Ctrl** (Windows) or **Cmd** (Mac) to select multiple states.
3. Click the green **"Download Terrain"** button.
4. Watch the **Download Log** next to it as tiles are fetched one by one.
5. The **"Cached Terrain Data"** panel on the right shows a ✅ when a state is fully downloaded, 🔶 if partially cached, and ⬜ if nothing has been downloaded yet.

> [!TIP]
> Start with just your home state. You can always add more states later. Terrain tiles are ~1–5 MB each and there are dozens per state, so large states like Texas or California can take a while.

---

### Part C: Ground Cover (Clutter) Data

The **"Ground Cover (Clutter) Data Download"** section works identically to terrain data. Clutter data improves coverage accuracy further by modeling how forests, urban areas, and open farmland absorb RF energy differently.

1. Select your state(s) from the **"Select States to Download"** list.
2. Click the green **"Download Clutter Data"** button.
3. The log and cached status panel will update as tiles are fetched.

> [!NOTE]
> Clutter data is technically optional but **highly recommended** for accurate results in forested or urban areas. If you're in a flat, open rural region it makes less difference.

Once terrain and clutter data are downloaded, go back to the main page: click **"← Back to Query Tool"** at the top.

---

## Step 5 — Your First Query (800 MHz Interop in your state)

Let's run a real search. We'll pull all **800 MHz Interoperability** channels licensed in Virginia — a classic public safety frequency coordination starting point.

### Setting Up the Search

The search form is on the left side of the main page. Here's what to fill in:

1. **Search Type:** Make sure **"LMR / Microwave"** is selected (it should be by default — it's the first radio button at the top).

2. **State(s):** In the **"State(s)"** multi-select box, scroll to find and click your state. For this example, click **"Virginia"**. *(Use Ctrl/Cmd to select multiple states at once.)*

3. **Frequency Group(s):** In the **"Frequency Group(s)"** multi-select dropdown, scroll down and select **"800 Interop"**. This automatically searches across all 800 MHz interoperability channels without needing to know the specific frequencies.

4. **License Types:** Leave the default checkboxes as-is (FB, FB2, FB2C, FB2T are pre-checked — these are the fixed-base transmitter classes you care about for coverage).

5. Click the blue **"Query"** button (it has a magnifying glass icon).

6. Click "Clear Form" to start a new query. Know your FRN? Try entering it. For "fun," select "Microwave" as well. If your system is all under one FRN, it SHOULD map your system :) 

### What Happens Next

A loading spinner will appear while the query runs against the local database. This typically takes **1–10 seconds** depending on how many results are returned.

When it finishes, the map will populate with **color-coded markers** representing licensed transmitter sites across Virginia in the 800 MHz interop band. You'll also see a scrollable **results table** below the map.

---

## Step 6 — Working with the Map & Results

### Reading the Map Markers

Markers are **color-coded by frequency band**:
- 🟢 **Green** — VHF (30–174 MHz)  
- 🔵 **Blue** — UHF (406–512 MHz)  
- 🟠 **Orange** — 700 MHz band  
- 🔴 **Red** — 800/900 MHz band

Clustered dots indicate multiple sites close together — click a cluster to zoom in and separate them.

### Clicking a Site Marker

Click any individual site marker (a small colored circle with an antenna icon) and a **popup** will appear showing:

- The **Call Sign** and **Licensee Name**
- The **transmit frequency** (or frequencies if multi-channel)
- A link to the **FCC Full Detail** record
- For each frequency ≥ 25 MHz, a small blue **`+Cov`** button

### Sorting and Exporting Results

The results table below the map supports:
- **Sorting** by any column — just click the column header
- **Export to KML** (for Google Earth)
- **Export to CSV** (for spreadsheets like Excel)
- **SDRTouch / SDR++** preset files for use with software-defined radios

---

## Step 7 — RF Coverage Modeling

Freq. Finder can estimate the RF coverage footprint of any licensed site using the **Longley-Rice Irregular Terrain Model (ITM)** — the same propagation model referenced by the FCC and TIA TSB-88.1 for public safety system design.

### Queuing Sites from the Map (Recommended)

This is the fastest way to get to coverage modeling:

1. Run a query and wait for markers to appear on the map.
2. Click a site marker to open its popup.
3. In the popup, find the frequency table. Next to each transmit frequency, you'll see a small blue **`+Cov`** button.
4. Click **`+Cov`** — a floating **"Coverage Queue"** widget will appear in the **bottom-right corner** of your screen, showing the site you just added.
5. Add up to **10 sites** total by clicking `+Cov` on other markers.
6. In the Coverage Queue widget, adjust settings:
   - **Max Radius (km):** How far outward to model (default 30 km — bump to 50–80 km for 800 MHz hilltop sites)
   - **Receiver Type:** "Portable (Hip)" or "Mobile (Vehicle)" — this sets the antenna height and body loss for the receiver end
   - **Threshold (dBm):** The minimum received signal level. `-95 dBm (Good)` is a sensible default for digital P25. Use `Custom...` to type any value.
7. Click **"Plot Coverage"** in the widget.

### What You Get

After a moment (coverage modeling is compute-intensive — allow 30–90 seconds per site), the map will show **three nested coverage contour polygons** per site:
- **Inner (solid)** — "Strong" signal (threshold + 20 dB)
- **Middle** — "Medium" signal (threshold + 10 dB)  
- **Outer** — "Fringe" — the outer edge of usable coverage at your threshold

A **"Coverage KML"** download link appears at the top of the map section, letting you export everything to Google Earth.

### Queuing from the Results Table

Alternatively, scroll down to the results table and use the **checkboxes in the leftmost column** to select sites. Then click the **"RF Coverage Modeling"** collapsible panel above the table, configure your settings, and click **"Plot Coverage"**.

> [!NOTE]
> Coverage accuracy is highest in areas where you've downloaded both terrain and clutter data. Without terrain data, the model falls back to simple circles. Check the **Data Maintenance** page to fill in any gaps.

---

## Step 8 — Hypothetical (Test) Sites

Want to model a site that doesn't exist in the FCC database yet — a proposed tower, a temporary antenna position for an exercise, or an emergency site?

The **Test Sites Library** lets you do exactly that.

### Opening the Library

On the main page, look for the **"Test Sites Library"** toggle button on the **left edge** of the screen. Click it to open a side panel that lists all your saved hypothetical sites.

### Creating a Test Site

There are two ways to create a test site:

**Method 1 — Drop a Pin on the Map:**
1. In the Test Sites Library panel, click the **"Drop Pin"** button.
2. Click anywhere on the map — a new test site form will open pre-filled with those coordinates.
3. Fill in: site name, frequency (MHz), transmit power (watts ERP), antenna height AMSL (meters), and antenna pattern (Omni, 120° Sector, Cardioid, or Yagi).
4. Click **Save**.

**Method 2 — Clone an Existing FCC Site:**
1. Run a query and click any map marker.
2. In the popup, click the **"Clone to Test Site"** button.
3. A new test site form opens pre-filled with that real site's parameters — frequency, power, antenna height, etc.
4. Modify whatever you'd like (change the power, height, or location) and save.

### Running Coverage for a Test Site

1. In the Test Sites Library panel, find your site and click **"+ Add to Coverage Queue"**.
2. The site appears in the floating Coverage Queue widget.
3. Configure settings and click **"Plot Coverage"** — the site will appear on the map with a **yellow pushpin** marker to distinguish it from real FCC sites.

### Saving and Sharing

Each test site is saved locally to your computer and persists between sessions. You can also click **"Export"** on any site to save it as a `.json` file that you can share with another Freq. Finder user.

---

## Tips & Troubleshooting

| Problem | Solution |
|---|---|
| App doesn't open in browser | Manually open your browser and go to `http://127.0.0.1:7223` |
| Console window closed, app stopped | Re-launch `FreqFinder.exe` (or `FreqFinder` on Mac) |
| No results for my query | Try broadening the search — fewer filters, "Any" license type, or a wider frequency range |
| Coverage plot is just a circle | Terrain data not downloaded for that area — visit **Data Maintenance** and download the relevant state(s) |
| FCC data sync is stuck | Leave it running — the FCC servers are sometimes slow. Check the log for error messages. If it fails, restart and try again |
| App runs very slowly during coverage computation | This is normal for sites with large max radius values (>50 km) and many radials — let it finish |
| Windows blocks the `.exe` every time | Add `C:\Freq.Finder\` to your antivirus exclusion list |
| Database date shows "N/A" | Run **Data Maintenance → Start Data Update** to perform your initial sync |

---

## Feedback

Freq. Finder is a work-in-progress. Your feedback directly shapes what gets built next. But also I make no promises or warranties. 

📧 **[gabe.elias@PACEPLAN.us](mailto:gabe.elias@PACEPLAN.us)**

Please include:
- Your OS and version (e.g., Windows 11, macOS 15)
- The app version number (visible at the top of the main page in red)
- What you were doing and what happened (expected vs. actual)

---

*Copyright © 2025 PACEPLAN.us and Gabe Elias. All Rights Reserved.*
*Data provided for informational purposes only. Always verify with the official [FCC ULS database](https://www.fcc.gov/uls).*
