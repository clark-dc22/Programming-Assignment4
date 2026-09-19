# Programming Assignment 4
---
## Introduction
This Jupyter Notebook (`DEL CARMEN_PA4.ipynb`) contains the solution for **Experiment 4** of ECE 2112. The activity demonstrates **data wrangling** and **data visualization** techniques using **Pandas** and **Matplotlib** on the ECE Board Exam 2 dataset.
--- 
## Intended Learning Outcomes
1. Filter tabular data using several categorical and numerical conditions.
2. Construct focused DataFrames by selecting relevant features.
3. Summarize the relationship between categorical features and a numerical variable.
4. Communicate a data comparison using clear and correctly labeled plots.

---
## Problems
---
### Problem A:
Filters students whose `Hometown == "Visayas"` **AND** `Track == "Communication"`.

Retains only these columns, in order: `Name, Gender, Math, Electronics, Average`.

Both filtering conditions are applied to the source dataset **before** the columns are selected.

Code:
```python
df.columns = ['Name', 'Gender', 'Track', 'Hometown',
              'Math', 'GEAS', 'Electronics', 'Average']
df.columns.tolist()
```

**Output:**

```
['Name', 'Gender', 'Track', 'Hometown', 'Math', 'GEAS', 'Electronics', 'Average']
```

Code:
```python
#Displaying specific columns
VisComm = df[(df["Hometown"] == "Visayas") & (df["Track"] == "Communication")][
    ["Name", "Gender", "Math", "Electronics", "Average"]]
VisComm
```
**Output:** 

5 rows — S11, S12, S18, S22, S28 — with only the five required columns.

```python
#Display number of rows in VisComm
print("Number of rows in VisComm:", VisComm.shape[0])
```

**Output:**

```
Number of rows in VisComm: 5
```

---
### Problem B
Create a second DataFrame named VisFemale containing students whose Hometown is Visayas and
whose Gender is Female. Display VisFemale. Then display only the rows of VisFemale whose Average is at least 60. Do not
overwrite VisFemale when performing this second filter

Code:

```python
#Displaying female boardtakers in Visayas
VisFemale = df[(df["Hometown"] == "Visayas") & (df["Gender"] == "Female")][
    ["Name", "Track", "GEAS", "Electronics", "Average"]
]

VisFemale
```
Output:

6 rows — S6, S11, S21, S22, S24, S26.

Code:
```python
#Displaying average greater than 60.
VisFemale_Average60 = VisFemale[VisFemale["Average"] >= 60]
VisFemale_Average60
```

**Output:** 

4 rows — S6 (83), S11 (67), S21 (72), S26 (62).

---
### Problem C
Examine how the recorded Average differs across the three categorical features Track, Gender, and
Hometown.

a. For each feature, compute the mean of Average for every category using Pandas.

b. Display the three summary tables.

c. Create one figure containing three bar charts: mean Average by Track, by Gender, and by
Hometown.

d. Below the figure, write three concise statements identifying the category with the highest sample
mean for each feature.

Code:

```python
#Displaying average
mean_by_track = df.groupby("Track")["Average"].mean()
print("Mean Average by Track:")
print(mean_by_track)
print()

mean_by_gender = df.groupby("Gender")["Average"].mean()
print("Mean Average by Gender:")
print(mean_by_gender)
print()

mean_by_hometown = df.groupby("Hometown")["Average"].mean()
print("Mean Average by Hometown:")
print(mean_by_hometown)
```
- `df.groupby("Track")["Average"]` — Splits `df` into three sub-groups (one per Track) and selects the `Average` column from each.
- `.mean()` — Computes the arithmetic mean of `Average` inside each group.
- `mean_by_track = ...` — Stores the result as a Series indexed by Track name.
- `print(...)` (×3) — Displays each Series.
- `print()` (blank) — Adds spacing between sections.

Output:

```
Mean Average by Track:
Track
Communication       64.2
Instrumentation     57.3
Microelectronics    64.4
Name: Average, dtype: float64

Mean Average by Gender:
Gender
Female    63.533333
Male      60.400000
Name: Average, dtype: float64

Mean Average by Hometown:
Hometown
Luzon       60.166667
Mindanao    61.571429
Visayas     64.181818
Name: Average, dtype: float64
```

Code:

```python
fig, axes = plt.subplots(1, 3, figsize=(18, 6))
```

- `plt.subplots(1, 3, ...)` — Creates a figure with **1 row × 3 columns** of subplots. Returns `fig` (the whole figure) and `axes` (an array of 3 Axes objects).
- `figsize=(18, 6)` — Sets the figure's width × height in inches — wide enough for three readable charts side-by-side.

```python
# --- Bar chart 1: Mean Average by Track ---
axes[0].bar(mean_by_track.index, mean_by_track.values, color="steelblue")
axes[0].set_title("Mean Average by Track")
axes[0].set_xlabel("Track")
axes[0].set_ylabel("Mean Average")
axes[0].tick_params(axis="x", rotation=0)
```

- `axes[0].bar(x, y, color=...)` — Draws a bar chart on the first subplot. X-values = Track names (`mean_by_track.index`); Y-values = means (`mean_by_track.values`). Bars are steel-blue.
- `set_title(...)` — Title above the chart.
- `set_xlabel / set_ylabel(...)` — Axis labels — required by the spec.
- `tick_params(axis="x", rotation=0)` — Keeps x-axis labels horizontal (they fit easily).

```python
# --- Bar chart 2: Mean Average by Gender ---
axes[1].bar(mean_by_gender.index, mean_by_gender.values, color="seagreen")
axes[1].set_title("Mean Average by Gender")
axes[1].set_xlabel("Gender")
axes[1].set_ylabel("Mean Average")
```

Same pattern, but plotted on `axes[1]` with green bars and Gender on the x-axis.

```python
# --- Bar chart 3: Mean Average by Hometown ---
axes[2].bar(mean_by_hometown.index, mean_by_hometown.values, color="indianred")
axes[2].set_title("Mean Average by Hometown")
axes[2].set_xlabel("Hometown")
axes[2].set_ylabel("Mean Average")
axes[2].tick_params(axis="x", rotation=45)
```

Same pattern on `axes[2]`, with red bars. The x-labels are rotated 45° because "Mindanao," "Visayas," and "Luzon" are longer and may overlap.

Code:

```python
plt.tight_layout()
plt.show()
```

- `plt.tight_layout()` — Automatically adjusts subplot spacing so titles, axis labels, and tick labels don't overlap.
- `plt.show()` — Renders and displays the figure. In Jupyter, it also embeds the image in the output cell.

**Output:** 

One figure containing three bar charts. All three share a common y-axis meaning ("Mean Average") but Matplotlib auto-scales each subplot independently.

Code: 

```python
top_track = mean_by_track.idxmax()
top_gender = mean_by_gender.idxmax()
top_hometown = mean_by_hometown.idxmax()

print(f"1. Track: The category with the highest sample mean Average is '{top_track}'.")
print(f"2. Gender: The category with the highest sample mean Average is '{top_gender}'.")
print(f"3. Hometown: The category with the highest sample mean Average is '{top_hometown}'.")
print()
print("Note: These differences describe the observed dataset only; they do not establish")
print("that Track, Gender, or Hometown causes a higher board-exam score.")
```

- `mean_by_track.idxmax()` — Returns the **index label** (category name) of the maximum value in the Series. E.g., `"Microelectronics"`.
- Same for gender and hometown — Finds `"Female"` and `"Visayas"`.
- `print(f"...{top_track}...")` — f-string interpolation inserts the category name into the sentence.
- Final `print(...)` statements — Explicitly state the interpretation rule — a group mean difference is descriptive, not causal.

**Output:**

```
1. Track: The category with the highest sample mean Average is 'Microelectronics'.
2. Gender: The category with the highest sample mean Average is 'Female'.
3. Hometown: The category with the highest sample mean Average is 'Visayas'.

Note: These differences describe the observed dataset only; they do not establish
that Track, Gender, or Hometown causes a higher board-exam score.
```
