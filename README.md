# FilmArc
Examining a semi-automatic interactive visualization using movie data collected since 2019

## Creating a Longitudinal Evaluation
So to create what is envisioned, first we need to turn the separate *vintages* of movie data into a long-form data set
I first started with the "Movie Cellar" files as these are more geared towards the critical acclaim

<details>
<summary> <b>Click to see data import</b> </summary>

```python
import pandas as pd
import plotly as pltly
import plotly.express as px

#Reading in data
cellar = pd.read_csv("MovieCellar.csv")
cellar25 = pd.read_csv("MovieCellar_25.csv")

#Making things uppercase
cellar["Movie"] = cellar['Movie'].str.upper()
cellar25["Movie"] = cellar25['Movie'].str.upper()

#Merging
full_cellar = pd.merge(cellar, cellar25, on='Movie')

#Flattening and Making Longitudinal
cellar_long = pd.wide_to_long(full_cellar, ["ScoreNow_cellar","ScoreByDays_cellar","ScoreNowDifZ_cellar","ScoreNowDif_cellar","ScorediffLeft_cellar","ScorediffLeftPer_cellar"], i="Movie", j="year", sep="-")
# Resetting index? 
flat_cellar_long = cellar_long.reset_index()
flat_cellar_long
```
</details>

### And then created a simple improvement plot
```python
fig = px.line(flat_cellar_long, x="year", y="ScoreNowDifZ_cellar",color="Movie", markers=True)
fig.update_layout(
    xaxis=dict(
        title = "Year",
        tickvals=[2024, 2025]
    )
)
fig.show()
```
  
