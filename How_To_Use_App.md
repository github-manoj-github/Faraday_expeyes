# Pendulum Voltage Studio — Quick Start  
  
Pendulum Voltage Studio is the analysis application accompanying the   
swinging-magnet Faraday experiment. It loads CSV captures from ExpEYES,   
overlays the traces, lets you curate peaks, and computes a live linear fit   
of V_pp against sin(θ₀/2).  
  
## Installation  
  
Requires Python 3.9 or later. From the repository root:  
  
```
pip install -r requirements.txt

```
  
  
The ==eyes17== dependency in ==requirements.txt== is only needed if you intend   
to capture new data through ==apps/faster_polling_live.py==. For   
post-hoc analysis of existing CSVs, only NumPy, pandas, matplotlib, SciPy   
and PyQt5 are required.  
  
## Running  
  
```
python apps/pendulum_voltage_studio.py

```
  
  
The application opens a single window with three regions:  
  
- **Left sidebar**: list of loaded captures, with checkboxes to toggle each   
- release angle on or off in the overlay  
- **Centre workspace**: the voltage traces, aligned at their first zero   
- crossing (t = 0)  
- **Right panel**: the live linear fit summary and a preview scatter plot   
- of the per-angle data  
  
## Loading data  
  
Click **Open folder** in the left sidebar and point to a directory   
containing CSV files named ==<angle>_Deg_<reading>_whole.csv== (for example   
==30_Deg_1_whole.csv==). All matching files are loaded and listed in the   
sidebar.  
  
Each CSV has two columns: time in seconds and voltage in volts. The   
application detrends each trace (median subtraction), locates the central   
bipolar pulse, and aligns all traces at their first zero crossing.  
  
## Typical workflow  
  
1. **Toggle release angles** on or off in the left sidebar to compare specific subsets.   
2. **Toggle release angles** on or off in the left sidebar to compare specific subsets.   
3. **Click any trace** in the workspace to *focus* it. Focused traces expose draggable peak markers and an integration-window handle.  
4. **Adjust peaks** if the automatic detector picked an ambiguous edge (for example on a noisy trace). Right-click a marker to relocate it manually.  
5. **Drag the integration window** to expand or contract the ±T bounds.   
6. **Watch the fit panel** on the right. Slope, intercept, R² and the per-angle scatter plot update in real time as you curate. A through- origin fit is also reported below the free fit.  
7. **Export the session** via **File → Export CSV**, which writes a long-format file with one row per trace and columns for angle, reading, V_pp, |V|_max, dV/dt at zero crossing, and ∫|V|dt.  
  
## Tips  
  
- Use **View → Reset** to clear focus and re-show all visible traces.  
- The integration window is intentionally wider than the visible pulse to ensure ≥99% of the integral is captured. See section 4 of the paper for the justification.  
  
## CSV format expected  
  
Two-column CSV, header row optional, time in seconds, voltage in volts:  
  
```
Time(s),Voltage(V)
0.000663,0.005095
0.001657,0.007625
...

```
  
  
Files produced by ==apps/faster_polling_live.py== and by the official ExpEYES   
mobile application both match this format. Any other source that produces   
the same two-column format will also load.  
  
## Troubleshooting  
  
**The peak detector picks the wrong extremum.** Click the trace to focus   
it, then right-click the marker and drag it to the correct sample.  
  
**The fit doesn't include an angle I expect.** Check the left-sidebar   
checkbox for that angle. Excluded angles (because the magnet doesn't clear   
the coil, or because the user disabled them) are shown greyed out.  
  
**The integration window seems too wide / too narrow.** Drag the   
integration window handles in the focused trace. The fit recomputes   
immediately.  
