# Pachpedi Naka Smart Traffic Optimization - version 2

**PROVISIONAL SYNTHETIC DEMO DATA - NOT CALIBRATED OR VALIDATED FOR REAL-WORLD USE.** A research/teaching simulation, not a real signal controller.

## Start here (Windows 10/11)

1. Install Python 3.9+ from python.org. During setup tick **"tcl/tk and IDLE"** and **"Add python.exe to PATH"** (Tkinter is needed for the window).
2. Open *Command Prompt* in this folder and install the two packages:
   ```
   py -m pip install -r requirements.txt
   ```
3. Start the application (double-click `run_app_windows.bat`, or):
   ```
   py Pachpedi_Naka_Ultra_Visual_Studio.py
   ```
That single file is the whole application: it carries the research engine inside itself and unpacks it into a folder `Pachpedi_Naka_Engine_v2` next to it on first start.

Other commands
```
py Pachpedi_Naka_Ultra_Visual_Studio.py --selftest     headless checks, no window (about 3 minutes; add --quick for 1 minute)
py Pachpedi_Naka_Ultra_Visual_Studio.py --classic      your original "Smart Traffic Lab" window
cd pachpedi_traffic ; py -m pytest -q                    312 automated tests (needs: py -m pip install pytest)
cd pachpedi_traffic ; py -m src.benchmark --seeds 20 --seed-start 100 --output outputs\benchmark_rerun    re-create the benchmark (add --duration 900 or 1800 for 15 / 30 minutes)
```

## What is in the window
* **Run length** (side panel): choose **15 min, 30 min or 60 min**. The engine is re-run for all five controllers at the chosen length; the clock, scrub bar, timeline and charts follow. A shorter run uses the *first* 15 or 30 minutes of the demand profile; the surge scenario scales with the length. 60 minutes is the default and the length all headline results use.
* **Intersection** - replay of a real engine run: signals from the engine's signal log, vehicles from the engine's per-vehicle records; single or side-by-side view of any two of five controllers on identical arrivals; phase timeline; live engine metrics; playback controls (Start / Pause / Resume / Reset, speed, scrub bar, Space and arrow keys).
* **Controller comparison** - all five controllers, same arrivals, one seed: table plus four charts.
* **Experiments** - the 20-seed benchmark (embedded, or re-run it from the button) with 95 % bootstrap intervals; the original two-controller experiment is kept as the "Legacy" button.
* **Controller decisions** - why the adaptive controller ended each green and chose each next phase.
* **Engine figures** - the charts written by the benchmark.
* **Classic demo** - your original illustrative demo (self-contained, not the research engine; its road drawing was corrected to left-hand traffic).

## Folder map
| Path | What |
|---|---|
| `Pachpedi_Naka_Ultra_Visual_Studio.py` | the application (single file, engine embedded) |
| `pachpedi_traffic/` | full engine source: `src/`, `config/`, `data/`, `tests/`, `tools/`, `outputs/benchmark` (60 min: all CSVs, charts, report), `outputs/benchmark_runlength` (15 and 30 min), `outputs/tuning_evidence` |
| `reference_existing_results/` | results shipped with your original engine (legacy format) |
| `00_ORIGINAL_BACKUP/` | your original files, read-only, with `SHA256SUMS.txt`, plus my previous deliverable |
| `docs/` | `TECHNICAL_REPORT.md`, `CHANGELOG.md`, `VERIFICATION_CHECKLIST.md`, `DEMO_GUIDE.md` |
| `test_results/` | raw output of every test run quoted in the report, and the scripts that produced them |

## Troubleshooting
| Symptom | Fix |
|---|---|
| "No module named numpy / matplotlib" | `py -m pip install numpy matplotlib`. Use the *same* interpreter that runs the script: run `py -c "import sys;print(sys.executable)"`. An IDE may use a different Python than the command line - install into the one it reports, or run from Command Prompt. |
| "No module named tkinter" / window will not open | Re-run the Python installer -> Modify -> tick *tcl/tk and IDLE*. |
| The window opens but says the engine is unavailable | The message shows the exact `pip install` line. The Classic demo tab still works. |
| "Could not extract the embedded engine" | The folder is read-only. Move the file to a writable folder (Documents). A temp-folder fallback is tried automatically. |
| Error such as "module 'src.scenarios' has no attribute 'RUN_LENGTHS_S'" | An older copy of the engine was left in `Pachpedi_Naka_Engine_v2` by an earlier build of this program. **Fixed in the current file:** it now stamps each extraction with the archive's SHA-256 and automatically moves a stale folder aside (renamed `Pachpedi_Naka_Engine_v2_replaced_<time>`, safe to delete) and extracts a fresh one. If you still see it with an older copy of the script, delete the folder `Pachpedi_Naka_Engine_v2` next to the script and start again. |
| Old results appear after upgrading | Results are written to `Pachpedi_Naka_Benchmark_Results` (new) and `Pachpedi_Naka_Research_Results` (legacy original). Delete a folder to remove its results. The engine now unpacks to `Pachpedi_Naka_Engine_v2`, so an old `Pachpedi_Naka_Engine` folder is never reused. |
| Blurry window on a high-DPI screen | The program asks Windows for DPI awareness; if text still looks wrong, right-click python.exe -> Properties -> Compatibility -> high DPI settings. (Not tested on Windows by me.) |
| "Missing file ... src\benchmark.py" when running `python -m src.benchmark` | Run it from inside the `pachpedi_traffic` folder. |

## Honest scope
Four single-approach phases, one vehicle class, no lanes, turns, pedestrians or emergency vehicles; all demand synthetic. See `docs/TECHNICAL_REPORT.md` section 10.
