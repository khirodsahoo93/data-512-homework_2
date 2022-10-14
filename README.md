
## Goal
The goal of this project is investigate bias in the article quality score of various wikipedia pages by countries and regions.

## Data Sources Used
For the analysis we are using 2 data sources:

- The Wikipedia English Article Dataset - contains list of article names , regions and their urls
- Population Data by Country - contains list of countries, their region and population

### Data Descriptions

*The politician Dataset*

| Column | Description |
| ------ | ----------- |
| name   | title/name of the politician
| url    | url of the wikipedia page
| country| country associated with the politician

*Population dataset*

| Column | Description |
| ------ | ----------- |
| Geography   | Country and Regions
| population (in millions) | Population in millions

Note-The above data contains some rows that provide cumulative regional population counts. These rows are distinguished by having ALL CAPS values in the 'geography' field (e.g. AFRICA, OCEANIA)

### ORES
For predicting article quality, the ORES API was used. The prediction returned by ORES is in the form of probabilities, as well as an absolute prediction. An article may be predicted to have one of the following quality levels:

FA - Featured article

GA - Good article

B - B-class article

C - C-class article

Start - Start-class article

Stub - Stub-class article

We define "high-quality" as an article predicted to be either FA or GA.

A sample response from ORES looks like this:
```sh
{
  "enwiki": {
    "models": {
      "wp10": {
        "version": "0.5.0"
      }
    },
    "scores": {
      "757539710": {
        "wp10": {
          "score": {
            "prediction": "Start",
            "probability": {
              "B": 0.0950995993086368,
              "C": 0.1709859524092081,
              "FA": 0.002534267983331672,
              "GA": 0.005731369423122624,
              "Start": 0.7091352495053856,
              "Stub": 0.01651356137031511
            }
          }
        }
      },
      "783381498": {
        "wp10": {
          "score": {
            "prediction": "Start",
            "probability": {
              "B": 0.020202281665235494,
              "C": 0.040498863202895134,
              "FA": 0.002648428776337411,
              "GA": 0.005101906528059532,
              "Start": 0.4793812253273645,
              "Stub": 0.452167294500108
            }
          }
        }
      }
    }
  }
}
```

## Output files

`wp_countries-no_match.txt` - This file contains the names of the politicians for which no response was found.

`wp_politicians_by_country.csv` -This file contains the name of the politicians ('article title') with 'country','region','population','revision_id' and 	'article_quality' for which respone was found

## License
- This assignment code is released under the MIT License.
- The Wikipedia English language articles data source is released under the CC-BY-SA 4.0 license.
- The population data is released under the ??? license.


## Research Implications

### Special Considerations

1. The population dataset contained name of both countries and regions in the Geography column. The population against the regions were the sum of population of all the countries preceded by it. So a data cleaning operation was performed to keep only countries name in the Geography columns and mapped the corresponding regions against it with the lowest hierarchy.
2. In the politician datasets, there were duplicate rows which were removed but the duplicates based on name and country were retained so that they can be used to calculate articles per capita.
3. Since, the population is in millions, there were countries which may have low populations due to which population in millions is 0. This turned out problematic when ratios were calculated.

What I Learned ?

- Reproducibility is hard

In my own code, I faced problems when I was trying to re-run a snippet. There were different errors everytime I was re-running. Also, when I was calling the API between different days, I was surprised to see that some pages gets deleted the next day which also impacted few of my figures. 

- Results may be biased

Since , the data is crawled from the wikipedia website and even if several preprocessing had been done to get a cleaner data , there is still a chance that some articles may not have been successfully crawled which could lead to wrong interpretation of the results. More details on how the data was captured could validate my hypothesis. Also, there is a chance of linguistic bias in the data as few non-english speaking nations may be educated but their english may be slightly different leading to scoring low for those articles. 

What biases did you expect to find in the data (before you started working with it), and why? 

- I expected to see the developed nations to have high coverage as compared to underdeveloped nation as I assumed that developed nations would have higher literacy rate and hence would have more number of articles.

Can you think of a realistic data science research situation where using these data (to train a model, perform a hypothesis-driven research, or make business decisions) might create biased or misleading results, due to the inherent gaps and limitations of the data?

- The exit poll survey that is often used to predict election results has biases because the survey responders could only be the people who are willing to participate. Also, the survey may done on regions which may not be the right representative of the entire population who are going to vote.

How might a researcher supplement or transform this dataset to potentially correct for the limitations/biases you observed? 

- For one instance, the researcher can include english speaking population numbers in each country. So that the articles per capita can be calculated based on english speaking population instead of total population to give a fair comparison
