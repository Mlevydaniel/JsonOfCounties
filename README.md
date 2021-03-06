# County-level Data

This is a repository of various data, broken down by US county.

While most of this repository is dedicated to providing the data and code that I used to produce the final dataset, the dataset itself is simply "states.json".

An example entry:

```JavaScript
{
  // ...
  "Nebraska": {
    // ...
    "holt county": {
      "area": 0.5223427619161941,
      "longitude": -98.78364595127402,
      "latitude": 42.465209445121566,
      "zip-codes": [
        "68766",
        "68759",
        "68725",
        "68713",
        "68746",
        "68726",
        "68823",
        "68764",
        "68735",
        "68763",
        "68711",
        "68777",
        "68780",
        "68734",
        "68742"
      ],
      "race_demographics": {
        "non_hispanic_white_alone_male": 0.4622715661230104,
        "non_hispanic_white_alone_female": 0.4660051090587542,
        "black_alone_male": 0.0020632737276478678,
        "black_alone_female": 0.0017685203379838867,
        "asian_alone_male": 0.0021615248575358615,
        "asian_alone_female": 0.003340538416191786,
        "hispanic_male": 0.02859107879740617,
        "hispanic_female": 0.024071526822558458
      },
      "age_demographics": {
        "0-4": 0.07044606012969148,
        "5-9": 0.0734918451562193,
        "10-14": 0.07142857142857142,
        "15-19": 0.05570839064649243,
        "20-24": 0.0500098251129888,
        "25-29": 0.04725879347612497,
        "30-34": 0.04951856946354883,
        "35-39": 0.0546276282177245,
        "40-44": 0.04666928669679701,
        "45-49": 0.046865788956573,
        "50-54": 0.056494399685596386,
        "55-59": 0.08351346040479465,
        "60-64": 0.078109648260955,
        "65-69": 0.06543525250540382,
        "70-74": 0.048143053645116916,
        "75-79": 0.039791707604637454,
        "80-84": 0.027706818628414228,
        "85+": 0.03478089998034977
      },
      "male": 5088,
      "female": 5090,
      "population": 10178,
      "deaths": {
        "suicides": 17,
        "firearm suicides": 12,
        "homicides": null
      },
      "labor_force": 5763.0,
      "employed": 5613.0,
      "unemployed": 150.0,
      "unemployment_rate": 2.6,
      "fatal_police_shootings": {
        "total": 0,
        "unarmed": 0,
        "fire-armed": 0
      },
      "police_deaths": 0,
      "avg_income": 51404,
      "covid-deaths": {
        "growth-rate-est": null,
        "6/3/20": 0,
        "6/10/20": 0,
        "6/17/20": 0,
        "6/24/20": 0,
        "7/1/20": 0
      },
      "covid-confirmed": {
        "6/3/20": 1,
        "6/10/20": 1,
        "6/17/20": 1,
        "6/24/20": 3,
        "7/1/20": 3
      },
      "elections": {
        "2008": {
          "total": 4974,
          "dem": 1089,
          "gop": 3746
        },
        "2012": {
          "total": 4749,
          "dem": 862,
          "gop": 3789
        },
        "2016": {
          "total": 4979,
          "dem": 522,
          "gop": 4275
        }
      },
      "fips": "31089"
    },
    // ...
  },
  // ...
}
```

If you want to use this data, most of the sources are from .gov websites.  The exceptions are fatal police shooting data (from Washington Post) and election data (from a variety of sources, see the [repo's readme](https://github.com/tonmcg/US_County_Level_Election_Results_08-16) for more info).

Please let me know if you want to use the data (it's nice to know my work is being used).  You can email me at ${myGithubUsername}@gmail, or just open an issue on Github.

## Sources

1. State geometry (location, area, etc.) is computed from data.gov, specifically [here](https://catalog.data.gov/dataset/tiger-line-shapefile-2017-nation-u-s-current-county-and-equivalent-national-shapefile) (download "Shapefile Zip File").  This data is NOT included in the repository because it is 122 MB; you may download it from the source, or from Google Drive [here](https://drive.google.com/file/d/1RvdPYAx3l0wJeGwNEfDnFOZafthqS4_b/view?usp=sharing).  Regardless of where you download it from, save the corresponding directory inside the "data" directory.  County areas are not exact; they assume longitude/latitude are x/y coordinates, and are then adjusted by multiplying by the cosine of the county's central point.  We also use the convex hull of the county, because of oddities that arise when a county has small "islands" of territory.  This overestimates the area of counties that contain many islands (most notably Monroe County, FL, Aleutians West, AK, and Honolulu County, HI)

2. Population and demographics comes from the Census Bureau from a 2018 table ([here](https://www.census.gov/data/tables/time-series/demo/popest/2010s-counties-detail.html#par_textimage_1383669527)).  This "United States" csv file is NOT included, as it is 154 MB, but can be downloaded from Google Drive [here](https://drive.google.com/file/d/11k-YAy4SM36jbXYUy5pylgo0mE-ZKudZ/view?usp=sharing).

3. Deaths, suicides, and homicides come from the [CDC's website](https://wonder.cdc.gov/cmf-icd10.html), where you can request the totals for each county.  We manually requested totals for all deaths, suicides, firearm suicides, and homicides.  Adding other causes of deaths is easy if you can give us the [ICD-10 Codes](https://wonder.cdc.gov/wonder/help/cmf.html#ICD-10%20Codes) you're interested in.  This is the source of the vast majority of missing data (marked as null in the JSON), since the CDC suppresses data from counties when there are fewer than 10 cases.

4. Labor statistics comes from [here](https://www.bls.gov/lau/#cntyaa) (the BLS).

5. Police shootings data comes from the [Washington Post's dataset](https://github.com/washingtonpost/data-police-shootings) and only includes 2019 shootings.  In cases where the county is ambiguous, shootings are distributed fractionally.

6. Average income comes from the BEA [here](https://apps.bea.gov/regional/downloadzip.cfm).  I don't trust the median income estimates (there are some really crazy counties...), so I only report the average income.

7. Covid data comes from [this site](https://usafacts.org/visualizations/coronavirus-covid-19-spread-map/) on usafacts.org, who says their data comes from the CDC.  Data was last updated on June 20th, 2020.

8. Presidential election data comes from Bill Morris' [github repository](https://github.com/tonmcg/US_County_Level_Election_Results_08-16/blob/master/US_County_Level_Presidential_Results_08-16.csv).  Alaska's counties are missing, as is Kalawao County, Hawaii.

9. Zip code-to-county data is from data.world [here](https://data.world/niccolley/us-zipcode-to-county-state), specifically Nic Colley, and is in the Public Domain.  Zip codes can overlap multiple counties.

Note that this "states.json" does NOT contain a superset of each data source.  For instance the racial/age demographic breakdown provided by "cc-est2018-alldata.csv" is extremely specific (giving race/sex break downs for every age bucket for 9 years!) but we don't include all of that in states.json.

Fortunately, it shouldn't be hard for somebody with some Python experience to modify "create_json.py" to add whatever additional data they might need.

## How to (Re)Generate

Run

```
$ python get_county_shootings.py
$ python create_json.py
```

The first script writes some JSONs to generated/... and is only necessary if you've made changes to fatal-police-shootings.csv or get_county_shootings.py.

## Why

I was originally trying to reproduce [this paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6391295/?fbclid=IwAR2Y0h6D-cEWXqk4_dooBX2MgUUrADyEIHN6iQFmbDc1qXf0MYHK3qWbUPo) (it reproduced!) and then I wanted to try and reproduce it with county-level data (since 3000 is a much larger sample size than 50).

This effort formed the base of this project, and I figured, as long as I had sunk in several hours into this project, I might as well publicize the results.

