
# Age and Unemployment Rates

Contributors:

CHIU, CHEUK MAN (github.com ID: [cheukman1207](https://github.com/cheukman1207) )

BOYCE, HAYLEY (github.com ID: [hfboyce](https://github.com/hfboyce) )

## Overview:

To conduct our analysis, we want to know if an individual's age has a significant effect on getting employed. In order to expand on that question, in this project, for a set of developed countries, we will be determining if the mean unemployment rate changes based on age group. The exploratory question that we will attempt to answer in this analysis is  

*" For developed countries in the past 5 years, is there a difference in the mean unemployment rates in different age groups?"*.

The different age groups we will be comparing are 15-24, 25-54, 55-64. A large assumption we are making is that both observations of males and female are equally important. To help visualize our data, we created a violin plot with overlaying jitter in the hopes of giving a better idea on the distributions of data for each age group. To explore more closely, the 95% confidence intervals of the average unemployment rates for each age group give a better understanding of the differentiation of the means unemployment rates.
To answer this question we will use ANOVA to see if the selected age groups differ. Depending on the results of the ANOVA, further analysis using pairwise t-tests will be conducted to explore exactly which age groups differ from one another.

## Data:

Dataset: [Employment: Unemployment rate by sex and age group](https://stats.oecd.org/index.aspx?queryid=54743) by OECD.org.   
Also can be found in the [data](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/tree/master/data) directory of this repository.   
From the data we are using the following information:

| Variable Name | Description | Type |
| --- | --- | --- |
| Years | 2012-2016 | Discrete |
| Countries | 34 countries | Categorical |
| Value | Unemployment Rates | Numeric |
| Age.Group | 15-24, 25-54 or 55-64 | Categorical |



## Usage:

To reproduce our analysis you have a choice to do so with or without docker depending on if you want to install the dependencies on your own accord. To avoid installing them, we have conveniently provided a docker option. 

### With Docker (no need to download dependencies)

1. Download and login to [docker](https://www.docker.com/get-started)
2. Download/clone this repo, and locate yourself to the root of where this project is saved.

3. Create the Docker image by pulling our image from [docker hub](https://hub.docker.com/r/hfboyce/dsci_522-age-and-unemployment-rates/) with the following command:

```
docker pull hfboyce/dsci_522-age-and-unemployment-rates

```
4. To make all the files needed for the analysis and creates the report: (PATH_ON_YOUR_COMPUTER refers to the root of where the project is saved)

```
docker run --rm -v PATH_ON_YOUR_COMPUTER:/Age-and-Unemployment-Rates hfboyce/dsci_522-age-and-unemployment-rates:latest make -C '/Age-and-Unemployment-Rates' all
```

5. To delete all the files created from the scripts you can use the command:

```
docker run --rm -v PATH_ON_YOUR_COMPUTER:/Age-and-Unemployment-Rates hfboyce/dsci_522-age-and-unemployment-rates:latest make -C '/Age-and-Unemployment-Rates' clean
```


### Without Docker

1. Clone this repo, and locate yourself to the root of where this project is saved.

2. Install necessary dependencies

3. You may choose to use the [run_all.sh](https://github.com/hfboyce/DSCI_522-Age-and-Unemployment-Rates/blob/master/run_all.sh) script in your bash with:

```
bash run_all.sh
```

  Or use the [Makefile](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/blob/master/Makefile) with the following command to make all the files needed for the analysis and creates the report:

```
make all
```

4. To delete all the files created from the scripts you can use the command:

```
make clean
```

The run_all.sh and Makefile consist of the following commands:

```
#1 Import data and perform data wrangling
Rscript src/01_clean-data.R data/unemployment-age-gender.csv data/unemployment-age-gender-countries-filtered-clean.csv

#2  Create explanatory visualization
Rscript src/02_visualize-data.R data/unemployment-age-gender-countries-filtered-clean.csv img/

#3  Perform ANOVA
Rscript src/03_anova.R  data/unemployment-age-gender-countries-filtered-clean.csv results/anova-table.csv

#4  Perform pairwise T-tests
Rscript src/04_pairwise-ttest.R  data/unemployment-age-gender-countries-filtered-clean.csv results/pairwise-test-table.csv

#5  Creates the final report
Rscript -e "rmarkdown::render('doc/age-and-unemployment-rates-report.Rmd')"  
```


## Procedure:

The procedure for generating the final report and all the associated files is shown below. For reference a flowchart has been included:

Script 1- [01_clean-data.R](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/blob/master/src/01_clean-data.R):

- **Inputs**: unemployment-age-gender.csv    

- **Purpose**: This script takes the raw data  and will clean and filter it.

- **Outputs**:  unemployment-age-gender-countries-filtered-clean.csv   

Script 2 - [02_visualize-data.R](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/blob/master/src/02_visualize-data.R):

- **Inputs**: unemployment-age-gender-countries-filtered-clean.csv

- **Purpose**: This script creates exploratory visualizations for target audiences.

- **Outputs**:  histrogram.png, violin.png, mean-CI.png

Script 3 - [03_anova.R](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/blob/master/src/03_anova.R):

- **Inputs**: unemployment-age-gender-countries-filtered-clean.csv

- **Purpose**: This script takes the performs ANOVA tests on the `Age.Group` values.

- **Outputs**: anova-table.csv

Script 4 - [04_pairwise-ttest.R](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/blob/master/src/04_pairwise-ttest.R):

- **Inputs**: unemployment-age-gender-countries-filtered-clean.csv

- **Purpose**: This script creates a pairwise t-test table with conclusions explaining if we reject or fail to reject the null hypothesis.

- **Outputs**:  confidence-interval-estimate-table.csv, pairwise-test-table.csv

![](https://github.com/hfboyce/DSCI_522-Age-and-Unemployment-Rates/blob/master/img/Makefile.png)
###### Figure 1: Diagram Showing the workflow of this analysis

## Dependencies:

 R & R libraries:  (version 3.5.1 )   

 - [tidyverse](https://github.com/tidyverse)  (version 1.2.1)
 - [ggplot2](https://github.com/tidyverse/ggplot2)(version 3.1.0)  
 - [broom](https://github.com/tidymodels/broom) (version 3.1.0)

## Report:

The report of this analysis can be found here: [Age and Unemployment Rates Report](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/blob/master/doc/age-and-unemployment-rates-report.md)

## Releases:

- [v0.1](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/releases/tag/v0.1)
- [V2.0](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/releases/tag/V2.0)
- [V3.0](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/releases/tag/V3.0)
- [V4.0](https://github.com/UBC-MDS/DSCI_522-Age-and-Unemployment-Rates/releases/tag/V4.0)
