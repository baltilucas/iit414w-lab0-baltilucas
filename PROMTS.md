# PROMPTS.md __ AI Usage Log
# IIT414W - Lucas Pavez - Lab 0


## Entry [1] — [Looking for OS reproducibility checks] · [2026-03-10]

**Context:**

I was trying to get some example in the __setup_check.ipynb__ for reproducibility checks.

**Prompt(s):**
Prompt 1: "Give me some example of systems check for reproducibility, different from current executions. try with OS verifications."

prompt 2: "Please regrite some sections considering deprecate functions. "

**Relevant Output:**
The AI model give me a code section inplace and the chat give a summary of the code written.

```python
import platform
import shutil
import locale

print('── OS & Architecture Verification ──────────────────────────────')

# 1. Operating System and Release
print(f'OS Type        : {platform.system()}')
print(f'OS Release     : {platform.release()}')
print(f'OS Version     : {platform.version()}')

# 2. Python Executable Path
# Why: Ensures code runs in the intended environment (venv, conda, system Python)
print(f'\nPython Exec    : {sys.executable}')

# 3. Architecture (32-bit vs 64-bit)
# Why: Affects numeric precision, memory limits, and integer overflow behavior
print(f'Architecture   : {platform.architecture()[0]}')
print(f'Processor      : {platform.processor() or "Not determined"}')

# 4. Free Disk Space
# Why: Reproducibility fails if disk is full → no data caching, incomplete downloads
disk_usage = shutil.disk_usage('/')
print(f'\nFree Disk Space: {disk_usage.free / (1024**3):.1f} GB (on root partition)')
if disk_usage.free / (1024**3) < 5:
    print('⚠️  WARNING: Less than 5 GB free — FastF1 cache or data downloads may fail!')

# 5. Default Locale
# Why: Affects string sorting, number parsing, and CSV handling across regions
print(f'\nDefault Locale : {locale.getdefaultlocale()}')

print('\n🔍 REPRODUCIBILITY INSIGHT:')
print('   These OS checks ensure your code runs consistently:')
print('   • Same Python path → same package versions')
print('   • Same architecture → same float precision (32-bit vs 64-bit)')
print('   • Adequate disk space → data downloads complete without interruption')
print('   • Same locale → CSV parsing and sorting behave identically')
```

**Validation:**
- Test 1: Run the code to find syntax errors quickly
- Test 2: Analize the useful of information given.

**Adaptations:**
- The key parts of code like import libraries or execution of some lines with description of the system, but data like ``sys.executable` isn't useful to reproduce since every computer and OS have its own folder path.

**Final Decision:**
I keep partially because data like memory aviable is useful and relevant to find some posible errors and problems to execute the notebook.


## Entry [2] — [VAR driver comparsion] · [2026-03-10]

**Context:**

I was trying to get some example in the __setup_check.ipynb__ for the instruction of comparsion of VAR driver with another.

**Prompt(s):**
Prompt 1: "Compare one additional driver against VER and summarize variance differences using the data structure of cell 17, then express in a table and a stylish graphic. use standard deviation of lap times in seconds."


**Relevant Output:**
The AI model give me a code section inplace and the chat give a summary of the code written especially why violin plot is relevant and a variance if condition to analize the variance summary.

```python

# Pick an additional driver: the 5th place qualifier for contrast
additional_driver = top5[2]  # 5th in qualifying order

# Drivers to compare
drivers_to_compare = ['VER', additional_driver]

# Filter fast laps for these drivers
compare_df = fast_laps[fast_laps['Driver'].isin(drivers_to_compare)].copy()

# Compute standard deviation of lap times
std_table = (
    compare_df.groupby('Driver')['LapTimeSec']
    .agg(['std', 'count'])  # Also include count for context
    .reset_index()
    .rename(columns={'std': 'StdDev_LapTimeSec', 'count': 'NumLaps'})
)

print('── Lap Time Variance Comparison ──')
print(std_table.to_string(index=False))

# Interpretation sentence
ver_std = std_table.loc[std_table['Driver'] == 'VER', 'StdDev_LapTimeSec'].values[0]
add_std = std_table.loc[std_table['Driver'] == additional_driver, 'StdDev_LapTimeSec'].values[0]
variance_diff = abs(ver_std - add_std)
more_consistent = 'VER' if ver_std < add_std else additional_driver
print(f"\nVariance Summary: {more_consistent} shows more lap time consistency (lower std dev: {min(ver_std, add_std):.3f}s vs {max(ver_std, add_std):.3f}s), with a difference of {variance_diff:.3f}s across their qualifying runs.")

# Stylish graphic: Violin plot for lap time distributions
fig, ax = plt.subplots(figsize=(10, 6))
sns.violinplot(
    data=compare_df, 
    x='Driver', 
    y='LapTimeSec', 
    ax=ax, 
    palette=['#FF6B6B', '#4ECDC4'],  # Stylish colors: red for VER, teal for other
    inner='quartile',  # Show quartiles inside
    linewidth=1.5
)
ax.set_title('Qualifying Lap Time Distributions: VER vs ' + additional_driver, fontsize=14, fontweight='bold')
ax.set_xlabel('Driver', fontsize=12)
ax.set_ylabel('Lap Time (seconds)', fontsize=12)
ax.grid(axis='y', alpha=0.3)
sns.despine()  # Remove top and right spines for cleaner look
plt.tight_layout()
plt.show()
```

**Validation:**
- Test 1: Run the code to find syntax errors quickly
- Test 2: Analize the useful of information given.

**Adaptations:**
- The code given keeps since the result is the expected since the request was just compare.
**Final Decision:**
I keep completly because the result was correct and didin't need mayor corrections.


## Entry [3] — [Load data from Fastf1] · [2026-03-11]

**Context:**

At __data_check.ipynb__ i asked by a load session example as instruction asked
**Prompt(s):**
Prompt 1: "Load one complete session from any 2024 race Print: session name and event name, Number of laps in the results (session.results.shape), Column names of the results DataFrame, First 5 rows of lap data (session.laps.head())"


**Relevant Output:**

The AI model give me a code section inplace and the chat give a summary of the code written, the place selected was Bahrain by the ai.
```python

import fastf1

# Load a complete session from the 2024 Bahrain Grand Prix Qualifying
session = fastf1.get_session(2024, 'Bahrain', 'Q')
session.load()

# Print session name and event name
print(f"Session Name: {session.name}")
print(f"Event Name: {session.event['EventName']}")

# Number of laps in the results
print(f"Number of laps in results: {session.results.shape[0]}")

# Column names of the results DataFrame
print(f"Column names: {list(session.results.columns)}")

# First 5 rows of lap data
print("First 5 rows of lap data:")
print(session.laps.head())
```
**Validation:**
- Test 1: Run the code to find syntax errors quickly

**Adaptations:**
- The code given dont use a real dataframe so i adapted to show a real table, and change Bahrain to Monzo.
**Final Decision:**
I keep most of code created with the changes to use pandas dataframes and Monzo instead Bahrain.

## Entry [4] — [Load data from Jolpica] · [2026-03-11]

**Context:**

At __data_check.ipynb__ i asked by charge data from jolpica API.

**Prompt(s):**
Prompt 1: "write a request in python for Query the Jolpica API (Ergast successor) for one historical endpoint. Suggested: 2024 driver standings (https://api.jolpi.ca/ergast/f1/2024/driverstandings/). Print:  HTTP status code (must be 200), Number of records returned, First 3 driver entries (name, nationality, points)"


**Relevant Output:**
The AI model give me a code section inplace and the chat give a summary of the code written, use only request and dont load the json, just charge the request.
```python
import requests

# Endpoint
url = "https://api.jolpi.ca/ergast/f1/2024/driverstandings/"

# Query API
response = requests.get(url)

# 1. HTTP status code
print("HTTP status code:", response.status_code)

data = response.json()

# Extract standings list
standings = data["MRData"]["StandingsTable"]["StandingsLists"][0]["DriverStandings"]

# 2. Number of records returned
print("Number of records returned:", len(standings))

# 3. First 3 driver entries (name, nationality, points)
print("\nFirst 3 drivers:")
for driver in standings[:3]:
    name = f"{driver['Driver']['givenName']} {driver['Driver']['familyName']}"
    nationality = driver['Driver']['nationality']
    points = driver['points']
    
    print({
        "name": name,
        "nationality": nationality,
        "points": points
    })
```


**Validation:**
- Test 1: Run the code to find syntax errors quickly

**Adaptations:**
- The code given dont use a real dataframe so i adapted to show a real table, and use json library instead json from request, the process is similar but is better catching errors.
**Final Decision:**
I keep most of code created with the changes to use pandas dataframes and Monzo instrad Bahrain.


## Entry [5] — [Finishing the Output of readme] · [2026-03-11]

**Context:**

At __README.MD__ the output expected was ideal for ai agent since can read the notebooks and write what will appear.

**Prompt(s):**
Prompt 1: "Add expected outputs of datacheck and setupcheck, e.g., "setup_check.ipynb prints a 6-row package table and 'All checks passed ✓'" " For context to copilot i added the __setup_check.ipynb__ and __data_check.ipynb__.


**Relevant Output:**
Copilot added the expected output to the README.MD, the added section is:

````markdown
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


````


**Validation:**
- It wasn't necessary, only read to be sure that the expected output is correct

**Adaptations:**
- It wasn't neccessary because it's only text and was correct
**Final Decision:**
I keep the copilot modification exactly like was given.