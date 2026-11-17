# PyLadies Dublin Sprints: Open Data, APIs and Git

A take-home reference for the first PyLadies Dublin sprint at Scopely, 17 Nov 2026. Keep it, come back to it, share it.

## Tonight's goals

- Fetch real open data from an API with Python
- Explore and clean a messy, real-world dataset
- Learn a basic Git workflow and share your work as a pull request

Things breaking is part of learning. Ask a facilitator or the person next to you.

## Key links

| What | Link |
| --- | --- |
| Sprints repo | [github.com/pyladiesdublin/sprints](https://github.com/pyladiesdublin/sprints) |
| Event page | [Luma](https://luma.com/szqpyy1g) |
| Irish Open Data Portal | [data.gov.ie](https://data.gov.ie) |
| Our dataset (RIA02) | [RTB Average Monthly Rent Report](https://data.gov.ie/dataset/ria02-rtb-average-monthly-rent-report) |
| API, CSV format | [CSO RIA02 CSV](https://ws.cso.ie/public/api.restful/PxStat.Data.Cube_API.ReadDataset/RIA02/CSV/1.0/en) |
| API, JSON-stat format | [CSO RIA02 JSON-stat](https://ws.cso.ie/public/api.restful/PxStat.Data.Cube_API.ReadDataset/RIA02/JSON-stat/2.0/en) |
| pandas docs | [pandas.pydata.org](https://pandas.pydata.org/docs/) |
| Git basics | [GitHub Docs: Hello World](https://docs.github.com/en/get-started/start-your-journey/hello-world) |
| PyLadies Dublin | [dublin.pyladies.com](http://dublin.pyladies.com/) |

## What is an API?

An API is a web address you ask for data, instead of downloading a file by hand. You send a request and get structured data back, usually JSON or CSV. Your code can rerun any time and always gets the latest version.

Tip: on data.gov.ie, filter datasets by the format "API" to find ones you can fetch with code.

## The dataset: RIA02 rent report

Average monthly rent in Ireland from the Residential Tenancies Board, published by the Central Statistics Office under CC BY 4.0. No API key needed.

- Year: 2008 to 2025
- Number of bedrooms: one, two, three, four plus, and groupings
- Property type: apartment, detached, semi detached, terrace, other flats
- Location: 446 areas, from counties down to towns and Dublin postcodes

About two thirds of the cells are empty, since small areas often lack enough tenancies. Expect to clean.

## Fetch it in Python

```python
import pandas as pd

URL = ("https://ws.cso.ie/public/api.restful/"
       "PxStat.Data.Cube_API.ReadDataset/RIA02/CSV/1.0/en")

df = pd.read_csv(URL)
print(df.shape)
print(df.columns)
print(df.head())
```

## Clean it and ask a question

```python
df = df.dropna(subset=["VALUE"])

dublin = df[
    (df["Location"] == "Dublin")
    & (df["Number of Bedrooms"] == "All bedrooms")
    & (df["Property Type"] == "All property types")
]
print(dublin[["Year", "VALUE"]])
```

Check the exact column names with df.columns before filtering; they can differ slightly.

## Challenges to try

1. Which county was most expensive in 2025, and which was cheapest?
2. Which area had the biggest percentage rise since 2015?
3. Apartments or houses: which costs more in your town?
4. Plot two cities over time on one chart.
5. Why are so many values missing? Can you spot a pattern?

## Share your work with Git

1. Fork the sprints repo on GitHub
2. git clone your fork to your laptop
3. git checkout -b my-branch
4. Add your notebook or script
5. git add . then git commit -m "Short description"
6. git push origin my-branch
7. Open a pull request on GitHub

Stuck on login? GitHub needs an SSH key or a personal access token. You can also upload files straight through the GitHub website.

## After tonight

Questions or ideas? Open an issue on the sprints repo, add tips to its wiki, or ask on the PyLadies Dublin Discord.
