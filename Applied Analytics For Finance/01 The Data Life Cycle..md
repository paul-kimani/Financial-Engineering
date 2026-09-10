For quantitative anlysis, the data won't come fully prepared, it has to undergo soe processes before its used for nay analysis, i.e..,
Raw data -> Cleaned data -> Prepared data -> Data + Results -> Archived data.

- **Raw Data:** This is essentially where we start, unprocessed and unfiltered.
- **Population vs Sample:** The population us the entire domain you are studying, while a sample is simply a subset of the former. For a sample to be useful (representative) it must be chosen randomly so thay every element has an equal chance of selection.
- **Metadata:** This is data about data, it acts as your documentation. It enables you to track your dataset and any transformations you make and also helos avoid nonsensical operations on that dataset. It enables research data to be understandable and guides reproducability of the results..
- **Exploratory Data Analysis(EDA):** This is the first step in profiling your dataset. You classify the dataset, validate it against your metadata, and check its distribution using visual tools like histograms or descriptive statistics(measures of centrality and dispersion).
#### From Messy to Model-Ready
For financial datasets, we are likely to find errors like outdated values (gaps in time series), manual entry mistakes, or shifts in measurement scales. So getting data ready for analysis ideally happens in 2 distinct phases:
1. **Data Cleaning(Wrangling):** This fixes any immediate errors.
	- **Missing Values:** You need to find and handle values that are explicitly marked(e.g 'NULL') or implicitly missing. Ypu can delete incomplete records or perhaps use imputation techniques to  fill them in.
	- **Outliers & Duplicates:** You identify extreme values (which are highly context dependent) and remove duplicate records that could bias your trading strategies or analysis.
2. **Data Preprocessing:** This prepares the clean data for actual modelling.
	- **Transformations:** You might normalize or scale numerical values, change data types(like binarization), or alter the dataset's entire structure via pivoting.
Once all this is done the data is officially ready to be fed into your algorithms.
