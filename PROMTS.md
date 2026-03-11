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


## Entry [2] — [Looking for OS reproducibility checks] · [2026-03-10]

**Context:**

I was trying to get some example in the __setup_check.ipynb__ for reproducibility checks.

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