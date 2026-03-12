# Lucas Pavez - baltilucas - IIT414W - March 11

## System Info

**Operating System:** Debian GNU/Linux 12

**Python:** 3.11.2

## Setup Instructions

Requiremnts:

    - python 3.11.2 or above
    - virtual enviroment  (venv) installed
    - vscode with jupyter extension or Jupyter Notebook installed

To create the enviroment use:

```bash
python3 -m venv venv
```
If there are errors try with py insetead python3

And to activate in windows use 
```bash
# For CMD
venv\Scripts\activate

#For Powershell
.\venv\Scripts\Activate.ps1

#For Git Bash
source venv/Scripts/activate
```

To activate in a Linux Distribution from a terminal:

```bash
source venv/bin/activate
```

And then, when the virtual enviroment is activated, run:

```bash
pip install -r requirements.txt
```

## How to run

To test, every notebook allow to execute with a run all command from the first comand cell.

## Problems encountered

There were not find any problems with the develop of code, but the syntax and style of Prompts.MD and the more focus use the ai was new and make me review the instruction many times.

## Expected Outputs

### setup_check.ipynb

When running all cells:

1. **Reproducibility Header** — Prints Python version, NumPy version, and seed constant:
   ```
   Python  : 3.11.2
   NumPy   : 2.4.3
   Seed    : 414
   ```

2. **Dependency Guard** — Confirms all packages are installed:
   ```
   All required packages already installed ✓
   fastf1  : 3.8.1
   pandas  : 2.3.3
   ```

3. **Package Versions Table** — Displays a 6-row table with package versions (numpy, pandas, sklearn, matplotlib, seaborn, fastf1), plus working directory path and FastF1 cache location, plus git version and recent 5 commits.

4. **OS & Architecture Verification** — Prints Linux OS type, kernel release, 64-bit architecture, available disk space (~91 GB), locale (es_CL), and UTF-8 encoding.

5. **Abu Dhabi Analysis** — Generates:
   - Two-panel plot showing all qualifying laps and fast-only laps for top 5 drivers
   - Summary table with best lap times and gaps to pole position
   - Violin plot comparing lap time distributions between two drivers
   - Variance analysis with standard deviation metrics

### data_check.ipynb

When running all cells:

1. **Reproducibility Header** — Prints Python version, NumPy version, and seed constant (same as above).

2. **F1 Session Data** — Loads 2024 Italian Grand Prix (Monza) Qualifying session and displays a table with:
   - Session name: `Qualifying`
   - Event name: `Italian Grand Prix`
   - Number of drivers in results: `20`
   - Result columns list (DriverNumber, BroadcastName, Abbreviation, etc.)
   - First lap data from session

3. **Ergast API Driver Standings** — Fetches 2024 F1 driver standings from ergast API and prints:
   ```
   HTTP status code: 200
   Number of records returned: 24
   ```
   Displays top 3 drivers (Max Verstappen, Lando Norris, Charles Leclerc) with points.

**Note:** Python version, NumPy/package versions, OS details, and disk space will differ based on your machine. The data pulled from FastF1 and Ergast API should match regardless of machine.

