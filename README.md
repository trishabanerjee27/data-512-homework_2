# data-512-homework_2
Homework 2 repo for DATA 512


# README: Analysis of Wikipedia Articles on Political Figures

## Overview

This project explores the concept of bias in data using Wikipedia articles about political figures across various countries and regions. The analysis aims to assess how the coverage of politicians and the quality of Wikipedia articles about politicians vary among countries, using a machine learning service called ORES to estimate article quality. By combining a dataset of Wikipedia articles and a dataset of country populations, we calculated various metrics such as total articles per capita and high-quality articles per capita.

The project is structured to analyze potential biases in global Wikipedia content, focusing on representation gaps, article quality, and regional disparities. The outputs include ranked lists of countries and regions by article coverage and quality.

## Folder Structure


## Folder Structure

DATA-512-HOMEWORK_2
│
├── code
│   └── code.ipynb
│
├── data
│   ├── combined_duplicates_politicians.csv     #duplicates found in the dataset
│   ├── ores_error_log.txt                      #log file for errors when retrieving ORES predictions
│   ├── politicians_by_country_AUG.2024.csv     #original wikipedia politicians data
│   ├── politicians_with_quality.csv            #wikipedia politicians with quality ratings
│   ├── population_by_country_AUG.2024.csv      #population data
│   ├── population_expanded.csv                 #expanded population dataset with regions and countries
│   ├── wp_countries-no_match.txt               #countries not matched between the two datasets
│   └── wp_politicians_by_country.csv           #final merged dataset for analysis
│
├── LICENSE
└── README.md



## Data Sources

- **Wikipedia Politicians Dataset**: A list of Wikipedia article pages related to politicians from various countries, including country names, article URLs, revision IDs, and ORES-predicted article quality scores.
  
- **Population Dataset**: Data containing population information from the Population Reference Bureau, with hierarchical regions (continents and subregions) to categorize countries.

## Methodology

### 1. Data Cleaning and Merging

The project began with cleaning the Wikipedia politicians dataset and expanding the population dataset. We ensured consistency across country names, handling duplicates, and merging both datasets using country as the key. During the merging process, unmatched countries were identified and saved in the `wp_countries-no_match.txt` file.

### 2. Retrieving Article Quality Predictions

Using the ORES API, we retrieved quality predictions for each Wikipedia article about a politician. The quality ratings provided by ORES range from "FA" (featured article) to "Stub" (low quality). Articles classified as either "FA" or "GA" were considered "high quality." Articles that couldn't be scored were logged in the `ores_error_log.txt` file.

### 3. Calculating Per Capita Metrics

We calculated two key metrics:
- **Total Articles per Capita**: The number of Wikipedia articles about politicians for each country, normalized by population (in millions).
- **High-Quality Articles per Capita**: The number of "high quality" articles (as defined by ORES) for each country, normalized by population.

We also aggregated these metrics on a regional level, ensuring that countries were assigned to their closest region.

### 4. Ranking Results

The following tables were produced based on the calculated metrics:
- **Top 10 Countries by Coverage**: The countries with the highest total articles per capita.
- **Bottom 10 Countries by Coverage**: The countries with the lowest total articles per capita.
- **Top 10 Countries by High-Quality Articles**: The countries with the highest proportion of high-quality articles.
- **Bottom 10 Countries by High-Quality Articles**: The countries with the lowest proportion of high-quality articles.
- **Geographic Regions by Total Articles per Capita**: A ranked list of regions by total article coverage.
- **Geographic Regions by High-Quality Articles per Capita**: A ranked list of regions by high-quality article coverage.

### 5. Logging Errors and Missing Data

For each article where ORES couldn't provide a quality score, we logged the article's title and revision ID in `ores_error_log.txt`. We also calculated and reported the error rate for missing ORES predictions. Here the error rate is 0.11%, which is acceptable given the standard threshold of 1%.

## Results

The final results from the analysis, including the rankings of countries and regions by coverage and article quality, were saved in `wp_politicians_by_country.csv`. This file includes the country, region, population, article title, revision ID, and article quality for each politician included in the analysis.

## Research Implications

Before beginning this analysis, we anticipated a few types of bias inherent in the Wikipedia data. Given that English Wikipedia is a dominant source of information globally, we expected a strong bias toward countries where English is widely spoken or where there is significant internet penetration. We also suspected that wealthier countries or those with more political influence on the global stage (e.g., Western Europe and North America) would have more coverage and higher-quality articles compared to countries in Africa, Southeast Asia, or regions with lower internet penetration. Additionally, we anticipated that discrepancies in population data or country naming conventions might complicate the merging process, leading to further biases in the analysis.

### Biases Discovered

1. **Population Data Issues**: North America was missing from the population data, which introduced bias in the results. Several countries had population values listed as zero (as the values were in millions, so a 0 million would still mean the value could be high), causing issues when calculating per capita metrics, as division by zero leads to infinity, and these rows were discarded.
   
2. **Country Name Mismatches**: Inconsistencies in country names between the two datasets (e.g., Guinea-Bissau, China) required manual aggregation. Some discrepancies, like "Korea," where we had separate North and South Korea entries, posed challenges that could not be resolved as it would introduce new bias. Thus, these were excluded from the analysis.

3. **Merging Bias**: As expected, merging datasets led to significant data loss due to unmatched entries. Poor data maintenance exacerbated this, as many countries were filtered out during the merge.

### Quality Bias

High-quality articles were disproportionately focused on politicians from wealthier, English-speaking countries. This suggests a systemic bias where underrepresented countries not only had fewer articles but also lower-quality articles.

### Geographic Disparities

Regions like Western Europe and South America had far higher coverage and quality compared to regions such as Africa and Southeast Asia. These disparities reflect broader issues of global digital inequality, where countries with higher internet access and Wikipedia contributor activity have better representation, in addition to the poor data scraping.

## Potential Solutions

To address these disparities, future projects could:
- Supplement the English Wikipedia data with articles from non-English Wikipedias.
- Integrate data from global repositories.
- Incentivize contributions from underrepresented regions.
- Collaborate with local Wikipedia chapters to help balance representation.

## Conclusion

This project revealed significant challenges in working with large datasets and highlighted the importance of maintaining data consistency, accuracy, and transparency. The key takeaway is that data scraping methods should be improved and more thoroughly documented to ensure that any future data collection can follow the same well-defined pattern. A clear and standardized documentation process would allow researchers to avoid introducing new biases when combining or processing data from multiple sources.

Moreover, data cleaning is an essential part of any data science workflow. Discrepancies in country names, missing population values, and improperly formatted data had a substantial impact on the results. Careful cleaning and preprocessing of data are necessary to mitigate the risk of these issues influencing the final analysis. This highlights the critical need for better preprocessing techniques and systematic handling of inconsistencies across different datasets.

Another important consideration is the choice of the appropriate unit system when calculating metrics like articles-per-capita. Since population values were provided in millions, it was essential to adjust calculations to avoid misleadingly small or inflated numbers. Researchers need to carefully assess their unit choices, as failing to do so can lead to incorrect interpretations of results.

Overall, this project has underscored the importance of standardization, thorough data documentation, and attention to detail in data preprocessing. Without these practices, subsequent analyses may suffer from biases, misinterpretations, or inaccuracies. A strong foundation of well-cleaned and well-documented data is critical to ensuring that any insights drawn are meaningful, reproducible, and fair. In the long term, improving the data collection and processing pipeline will lead to more accurate and representative analyses, especially when dealing with complex global datasets like Wikipedia articles.
