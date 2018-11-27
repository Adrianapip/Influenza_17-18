
# Introduction

Fall is undoubtedly my favorite time of year. I look forward to the earthy smell of leaves in the crisp air and hibernating in cozy sweaters. Also on my fall checklist - my seasonal shot of pumpkin spice latte and of course, the flu vaccine - both of which tend to get a bad rap.

This year while herding my children through the flu tent line, I overhear talk that gets my nerd gears moving. Patients are chatting about whether the shot will work. There was lots of shrugging, combined with “who knows?” as we obediently moved through line to expose our shoulders. 

Worrying about the flu shot is a valid concern (though never enough to deter one from getting the shot, but that’s another story for another time). Afterall, the vaccine is our sole preemptive, immunological-based defense from the seasonal flu. 

As a mother of small children, I've never thought twice about getting flu shots. But why are we still concerned about the vaccine’s efficacy every season?

My general understanding is that influenza seasons remain largely unpredictable; it's hard to extract from forecast data exactly which viruses in the next season will prevail (read: influenza's sped-up version of survival of the fittest). The keyword here being **exactly**; we have to wisely choose strains before the season starts because the vaccine takes months to produce.

I realize that this is a simplified answer to a hard question. Influenza research is a difficult field, with problems that have eluded the most experienced researchers. Nonetheless, I thought it'd be a fun and relevant topic to tackle by looking into influenza genetics and how it affects the seasonal vaccine. 

As the current season is underway at the time of this writing, for a complete analysis I will look at last season’s data.

Here's what I'm interested in answering in this analysis:

   ##### 1. What strains were included in the 2017-18 vaccine?
   ##### 2. What subtypes and strains were most common in the US in 2017-18?
   ##### 3. How similar were the recommended vaccine strains to the reported sequences during the 2017-18 season?
   
This article will go over answering the first two questions and I will have a follow up with sequence data in the next part of this series.

---

# Question 1: What strains were included in the 2017-18 vaccine? 

Early in 2017, the WHO announced its [recommended composition of the influenza vaccine](http://www.who.int/influenza/vaccines/virus/recommendations/2017_18_north/en/): 

* A/Michigan/45/2015 (H1N1)pdm09-like virus
* A/Hong Kong/4801/2014 (H3N2)-like virus
* B/Brisbane/60/2008-like virus
* B/Phuket/3073/2013-like virus (for quadrivalent vaccine)

An expert panel sources and studies recent surveillance data of the virus, clinical antibody studies of currently circulating viruses, and predictive models of genetic/antigenic changes to come up with these recommendations. Each country then formulates their own vaccine based on regional requirements and observations.

Last flu season (2017-18) was severe compared to previous years. The CDC classifed the season as having an overall ["High" severity] across all age groups(https://www.cdc.gov/flu/professionals/classifies-flu-severity.htm). Previous years yielded largely low to moderate seasons. 

What exactly does “High Severity” mean? There are many factors that contribute to this classification, but mainly it’s based on flu-related hospitalizations and deaths. To explore the details further, here are excellent resources/graphs tracking this data: the US monitoring system [FluView](https://gis.cdc.gov/grasp/fluview/fluportaldashboard.html) and the [WHO's FluNet](https://www.who.int/influenza/resources/charts/en/).

---

# Question 2: What subtypes and strains were most common in the US in 2017-18?

Answering questions 2 and 3 will require looking into the sequence databases.

#### Data source

The datasets in this project are from the [Influenza Research Database](https://www.fludb.org/brc/home.spg?decorator=influenza), a viral data repository that pulls from [NCBI GenBank](https://www.ncbi.nlm.nih.gov/genbank/) and [NCBI RefSeq](https://www.ncbi.nlm.nih.gov/refseq/).

** Note that this analysis is performed only on the data obtained from the Influenza Research Database. It therefore may not be a complete analysis of the 2017-18 flu season as not all US flu cases may have been reported in this database. 


## Preparing the Dataframes


```python
# Import data analysis libraries 
import numpy as np
import pandas as pd

# Import visualization libraries
import seaborn as sns
sns.set_style('whitegrid')
sns.set_context('paper', font_scale=1.)
import matplotlib.pyplot as plt
%matplotlib inline 
```


```python
# Load the flu datasets, read fields by tabs
df_fluA = pd.read_csv('flu_A.tsv', sep='\t')
df_fluB = pd.read_csv('flu_B.tsv', sep='\t')
```


```python
# Remove spaces in column names in df_fluA and check 
df_fluA.columns = [c.replace(' ', '_') for c in df_fluA.columns]
df_fluA.columns
```




    Index(['Segment', 'Protein_Name', 'Sequence_Accession', 'Complete_Genome',
           'Segment_Length', 'Subtype', 'Collection_Date', 'Host_Species',
           'Country', 'State/Province', 'Flu_Season', 'Strain_Name',
           'Curation_Report', 'Unnamed:_13'],
          dtype='object')




```python
# Remove spaces in column names in df_fluB and check
df_fluB.columns = [c.replace(' ', '_') for c in df_fluB.columns]
df_fluB.columns
```




    Index(['Segment', 'Protein_Name', 'Sequence_Accession', 'Complete_Genome',
           'Segment_Length', 'Subtype', 'Collection_Date', 'Host_Species',
           'Country', 'State/Province', 'Flu_Season', 'Strain_Name',
           'Curation_Report', 'Unnamed:_13'],
          dtype='object')



Since there are two flu types (A and B), I prefer to analyze seperate datasets for each, rather than merging the two dataframes. 


```python
# Let's take a look at the first few rows of data for Flu A
df_fluA.head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Segment</th>
      <th>Protein_Name</th>
      <th>Sequence_Accession</th>
      <th>Complete_Genome</th>
      <th>Segment_Length</th>
      <th>Subtype</th>
      <th>Collection_Date</th>
      <th>Host_Species</th>
      <th>Country</th>
      <th>State/Province</th>
      <th>Flu_Season</th>
      <th>Strain_Name</th>
      <th>Curation_Report</th>
      <th>Unnamed:_13</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6</td>
      <td>NaN</td>
      <td>MH083303</td>
      <td>Yes</td>
      <td>1433</td>
      <td>H1N1</td>
      <td>01/02/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>A/Alabama/01/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>HA</td>
      <td>MH083301</td>
      <td>Yes</td>
      <td>1752</td>
      <td>H1N1</td>
      <td>01/02/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>A/Alabama/01/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7</td>
      <td>-N/A-,-N/A-</td>
      <td>MH083304</td>
      <td>Yes</td>
      <td>1002</td>
      <td>H1N1</td>
      <td>01/02/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>A/Alabama/01/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# And here is the first few rows for Flu B
df_fluB.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Segment</th>
      <th>Protein_Name</th>
      <th>Sequence_Accession</th>
      <th>Complete_Genome</th>
      <th>Segment_Length</th>
      <th>Subtype</th>
      <th>Collection_Date</th>
      <th>Host_Species</th>
      <th>Country</th>
      <th>State/Province</th>
      <th>Flu_Season</th>
      <th>Strain_Name</th>
      <th>Curation_Report</th>
      <th>Unnamed:_13</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>PB2</td>
      <td>MH135882</td>
      <td>Yes</td>
      <td>2367</td>
      <td>-N/A-</td>
      <td>01/22/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/02/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>PA</td>
      <td>MH135884</td>
      <td>Yes</td>
      <td>2276</td>
      <td>-N/A-</td>
      <td>01/22/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/02/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>NP</td>
      <td>MH135886</td>
      <td>Yes</td>
      <td>1816</td>
      <td>-N/A-</td>
      <td>01/22/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/02/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7</td>
      <td>-N/A-,-N/A-</td>
      <td>MH135888</td>
      <td>Yes</td>
      <td>1155</td>
      <td>-N/A-</td>
      <td>01/22/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/02/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>8</td>
      <td>-N/A-,-N/A-</td>
      <td>MH135889</td>
      <td>Yes</td>
      <td>1060</td>
      <td>-N/A-</td>
      <td>01/22/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/02/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>-N/A-,-N/A-</td>
      <td>MH135887</td>
      <td>Yes</td>
      <td>1530</td>
      <td>-N/A-</td>
      <td>01/22/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/02/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4</td>
      <td>HA</td>
      <td>MH135885</td>
      <td>Yes</td>
      <td>1853</td>
      <td>-N/A-</td>
      <td>01/22/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/02/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1</td>
      <td>PB1</td>
      <td>MH135883</td>
      <td>Yes</td>
      <td>2340</td>
      <td>-N/A-</td>
      <td>01/22/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/02/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>8</th>
      <td>7</td>
      <td>-N/A-,-N/A-</td>
      <td>MH135896</td>
      <td>Yes</td>
      <td>1155</td>
      <td>-N/A-</td>
      <td>02/06/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/03/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>5</td>
      <td>NP</td>
      <td>MH135894</td>
      <td>Yes</td>
      <td>1816</td>
      <td>-N/A-</td>
      <td>02/06/2018</td>
      <td>IRD:Human</td>
      <td>USA</td>
      <td>Alabama</td>
      <td>17-18</td>
      <td>B/Alabama/03/2018</td>
      <td>Pass</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



---

## Exploring the Data

Now that the data has been properly extracted and converted into dataframes, let's take a closer look at the data.


```python
df_fluA.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 24407 entries, 0 to 24406
    Data columns (total 14 columns):
    Segment               24407 non-null int64
    Protein_Name          21351 non-null object
    Sequence_Accession    24407 non-null object
    Complete_Genome       24407 non-null object
    Segment_Length        24407 non-null int64
    Subtype               24407 non-null object
    Collection_Date       24407 non-null object
    Host_Species          24407 non-null object
    Country               24407 non-null object
    State/Province        24407 non-null object
    Flu_Season            24407 non-null object
    Strain_Name           24407 non-null object
    Curation_Report       24407 non-null object
    Unnamed:_13           0 non-null float64
    dtypes: float64(1), int64(2), object(11)
    memory usage: 2.6+ MB


Approximately 24K records, however, there are multiple Host Species: 


```python
df_fluA['Host_Species'].unique()
```




    array(['IRD:Human', 'IRD:Dog', 'IRD:Ruddy Turnstone/Avian', 'IRD:Swine'],
          dtype=object)



I want to look at sequences isolated only from Humans. 

Let's make new dataframes called:

df_fluA_Human and df_fluB_Human



```python
df_fluA_Human = df_fluA[df_fluA['Host_Species'] == 'IRD:Human' ]
df_fluB_Human = df_fluB[df_fluB['Host_Species'] == 'IRD:Human']
df_fluA_Human.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 23375 entries, 0 to 24406
    Data columns (total 14 columns):
    Segment               23375 non-null int64
    Protein_Name          20448 non-null object
    Sequence_Accession    23375 non-null object
    Complete_Genome       23375 non-null object
    Segment_Length        23375 non-null int64
    Subtype               23375 non-null object
    Collection_Date       23375 non-null object
    Host_Species          23375 non-null object
    Country               23375 non-null object
    State/Province        23375 non-null object
    Flu_Season            23375 non-null object
    Strain_Name           23375 non-null object
    Curation_Report       23375 non-null object
    Unnamed:_13           0 non-null float64
    dtypes: float64(1), int64(2), object(11)
    memory usage: 2.7+ MB


Now that we have flu that has been sequenced from Humans, let's take a look at:

##### General Info
* How many records: 'n_records' 
* What were the top 5 states reporting: 'top5_states'
* What states had the least reporting: 'bottom5_states'

##### Subtype Details
* How many unique subtypes are there: 'n_uni_subtypes'
* What are those subtypes: 'uni_subtypes'



```python
n_records = len(df_fluA_Human)
top5_states = df_fluA_Human['State/Province'].value_counts().head(5)
bottom5_states = df_fluA_Human['State/Province'].value_counts().tail(5)
n_uni_subtypes = df_fluA_Human['Subtype'].nunique()
uni_subtypes = df_fluA_Human['Subtype'].unique()

print("\033[1m" + "General Info" + "\033[0m")
print("Total number of flu A records: {}".format(n_records))
print("Top 5 states reporting Flu A:\n{}\n".format(top5_states))
print("Bottom 5 states reporting Flu A:\n{}\n".format(bottom5_states))
print("\033[1m" + "Subtype Details" + "\033[0m")
print("Number of unique subtypes reported: {}".format(n_uni_subtypes))
print("Those subtypes are: {}".format(uni_subtypes))
```

    [1mGeneral Info[0m
    Total number of flu A records: 23375
    Top 5 states reporting Flu A:
    California    2228
    Florida       1035
    Iowa           928
    Texas          808
    Illinois       743
    Name: State/Province, dtype: int64
    
    Bottom 5 states reporting Flu A:
    Nevada            205
    Vermont           168
    Nebraska          160
    South Carolina    120
    Rhode Island       72
    Name: State/Province, dtype: int64
    
    [1mSubtype Details[0m
    Number of unique subtypes reported: 2
    Those subtypes are: ['H1N1' 'H3N2']


For Flu A, there are only two subtypes: H1N1 and H3N2. Let's plot out when these entries were submitted ('Collection_Date) to get a look at the 2017-18 flu season by month.


```python
# Let's check the type of the Collection_Date column

type(df_fluA_Human.loc[:,'Collection_Date'].iloc[0])
```




    str




```python
# since the Collection_Date is a string, convert the column to datetime objects 

df_fluA_Human = df_fluA_Human.copy()  
df_fluA_Human['Collection_Date'] = pd.to_datetime(df_fluA_Human['Collection_Date'])
```


```python
# check the conversion
type(df_fluA_Human['Collection_Date'].iloc[0])
```




    pandas._libs.tslibs.timestamps.Timestamp




```python
# Now create a new column for just the month of each record. This will be the x-axis of my graph

df_fluA_Human['Month'] = df_fluA_Human['Collection_Date'].apply(lambda time: time.month)
```


```python
# There it is, the last column

df_fluA_Human.columns
```




    Index(['Segment', 'Protein_Name', 'Sequence_Accession', 'Complete_Genome',
           'Segment_Length', 'Subtype', 'Collection_Date', 'Host_Species',
           'Country', 'State/Province', 'Flu_Season', 'Strain_Name',
           'Curation_Report', 'Unnamed:_13', 'Month'],
          dtype='object')




```python
# Look at the data in the Month column from a random sample

df_fluA_Human['Month'].sample(5)
```




    7852     11
    22312     1
    18320    12
    8115      4
    1272      3
    Name: Month, dtype: int64




```python
# Looks like the month is an int, but I'd like it to read month names
import calendar

df_fluA_Human['Month'] = df_fluA_Human['Month'].apply(lambda x: calendar.month_abbr[x])
```


```python
df_fluA_Human['Month'].sample(5)
```




    9608     Mar
    21943    Apr
    22171    Feb
    21425    Jan
    6199     Oct
    Name: Month, dtype: object




```python
# Plots the records by month, starting with the beginning of the flu season in Oct 2017

order = ['Oct', 'Nov', 'Dec','Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']

sns.countplot(x='Month', data=df_fluA_Human, hue='Subtype', order = order, palette='rainbow').set_title('Reported Flu A Subtypes in US during 2017-18')

# relocate the legend
plt.legend(bbox_to_anchor = (1.05, 1), loc =2, borderaxespad=0.)
```




    <matplotlib.legend.Legend at 0x1a22963f28>




![png](influenza_1_files/influenza_1_26_1.png)


### Partial answer to Question 2: For Flu A, the only reported subtypes are H1N1 and H3N2 with H3N2 being reported at higher rates. There is a peak of cases in Dec 2017 for H3N2 and Jan 2018 for H1N1.

As for the strains, there are a bunch reported:



```python
df_fluA_Human["Strain_Name"].nunique()
```




    2333



Because there are so many strains, a figure won't work, but we can list them to get an idea the most common. This may prove useful later when we need to look at exact sequences.


```python
df_H1N1_strains = df_fluA_Human[df_fluA_Human["Subtype"] == "H1N1"]
H1N1_strains = df_H1N1_strains['Strain_Name'].value_counts().head(10)
df_H3N2_strains = df_fluA_Human[df_fluA_Human["Subtype"] == "H3N2"]
H3N2_strains = df_H3N2_strains['Strain_Name'].value_counts().head(10)

print("\033[1m" + "Most Common Strains per Subtype" + "\033[0m")
print("H1N1 strains:\n{}\n".format(H1N1_strains))
print("H3N2 strains:\n{}\n".format(H3N2_strains))

```

    [1mMost Common Strains per Subtype[0m
    H1N1 strains:
    A/New Jersey/13/2018     32
    A/New York/34/2017       24
    A/Louisiana/62/2017      24
    A/Iowa/34/2017           24
    A/Illinois/37/2017       24
    A/Louisiana/64/2017      24
    A/Hawaii/54/2017         24
    A/Virginia/36/2017       24
    A/Connecticut/31/2017    24
    A/Mississippi/35/2017    24
    Name: Strain_Name, dtype: int64
    
    H3N2 strains:
    A/Kansas/14/2017         39
    A/Wisconsin/04/2018      32
    A/New York/53/2017       24
    A/California/113/2017    24
    A/Connecticut/32/2017    24
    A/Missouri/33/2017       24
    A/California/159/2017    24
    A/Nevada/20/2017         24
    A/Washington/308/2017    24
    A/Wyoming/33/2017        24
    Name: Strain_Name, dtype: int64
    


Now, lets perform the same analysis for Influenza Type B.


```python
df_fluB_Human.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 11281 entries, 0 to 11280
    Data columns (total 14 columns):
    Segment               11281 non-null int64
    Protein_Name          11281 non-null object
    Sequence_Accession    11281 non-null object
    Complete_Genome       11281 non-null object
    Segment_Length        11281 non-null int64
    Subtype               11281 non-null object
    Collection_Date       11281 non-null object
    Host_Species          11281 non-null object
    Country               11281 non-null object
    State/Province        11281 non-null object
    Flu_Season            11281 non-null object
    Strain_Name           11281 non-null object
    Curation_Report       11281 non-null object
    Unnamed:_13           0 non-null float64
    dtypes: float64(1), int64(2), object(11)
    memory usage: 1.3+ MB



```python
n_records = len(df_fluB_Human)
top5_states = df_fluB_Human['State/Province'].value_counts().head(5)
bottom5_states = df_fluB_Human['State/Province'].value_counts().tail(5)
n_uni_subtypes = df_fluB_Human['Subtype'].nunique()
uni_subtypes = df_fluB_Human['Subtype'].unique()

print("\033[1m" + "General Info" + "\033[0m")
print("Total number of flu B records: {}".format(n_records))
print("Top 5 states reporting Flu B:\n{}\n".format(top5_states))
print("Bottom 5 states reporting Flu B:\n{}\n".format(bottom5_states))
print("\033[1m" + "Subtype Details" + "\033[0m")
print("Number of unique subtypes reported: {}".format(n_uni_subtypes))
print("Those subtypes are: {}".format(uni_subtypes))
```

    [1mGeneral Info[0m
    Total number of flu B records: 11281
    Top 5 states reporting Flu B:
    California    1096
    Montana        408
    Florida        398
    Arizona        358
    Wisconsin      339
    Name: State/Province, dtype: int64
    
    Bottom 5 states reporting Flu B:
    District of Columbia    80
    South Carolina          80
    Kansas                  72
    Rhode Island            48
    Nevada                  24
    Name: State/Province, dtype: int64
    
    [1mSubtype Details[0m
    Number of unique subtypes reported: 1
    Those subtypes are: ['-N/A-']


Note that there is only one subtype reported, which is unnamed. We can still take a look at the montly reporting dates and most common strains.


```python
df_fluB_Human = df_fluB_Human.copy()  
df_fluB_Human['Collection_Date'] = pd.to_datetime(df_fluB_Human['Collection_Date'])
```


```python
# check the conversion
type(df_fluB_Human['Collection_Date'].iloc[0])
```




    pandas._libs.tslibs.timestamps.Timestamp




```python
# Create a new column for just the month of each record. This will be the x-axis of my graph

df_fluB_Human['Month'] = df_fluB_Human['Collection_Date'].apply(lambda time: time.month)
```


```python
# Looks like the month is an int, but I'd like it to read month names
import calendar

df_fluB_Human['Month'] = df_fluB_Human['Month'].apply(lambda x: calendar.month_abbr[x])
```


```python
#Plots the records by month, starting with the beginning of the flu season in Oct 2017

order = ['Oct', 'Nov', 'Dec','Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']

sns.countplot(x='Month', data=df_fluB_Human, hue='Subtype', order = order, palette='rainbow').set_title('Reported Flu B Subtypes in US during 2017-18')

# relocate the legend
plt.legend(bbox_to_anchor = (1.05, 1), loc =2, borderaxespad=0.)
```




    <matplotlib.legend.Legend at 0x1a23105a58>




![png](influenza_1_files/influenza_1_39_1.png)


Here's a look at just Flu B for the season. Although interesting to see how it compares to Flu A, it'd be nicer if we could get a look at Flu A and B together in one figure. Let's do that now.


```python
df_AllFlu = pd.merge(df_fluA_Human, df_fluB_Human, how='outer')
```


```python
#Plotting out all three subtypes
order = ['Oct', 'Nov', 'Dec','Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep']

sns.countplot(x='Month', data=df_AllFlu, hue='Subtype', order = order, palette='rainbow').set_title('Reported Flu Subtypes in US during 2017-18')

# relocate the legend
plt.legend(bbox_to_anchor = (1.05, 1), loc =2, borderaxespad=0.)
```




    <matplotlib.legend.Legend at 0x1a1f3caef0>




![png](influenza_1_files/influenza_1_42_1.png)


### Here is a cumulative look at all three subtypes: Flu A H1N1, Flu A H3N2, and Flu B 'N/A'. What's interesting is how Flu B persists during the end while Flu A is slowly waning out for the season.

Although these figures answer Question 2, it'd be fun to take it one step further and look at these data using a map.

First thing we have to do is convert the state names to abbreviations so they read into the map correctly.

Here's a dictionary of state names to abbreviations thanks to [rogerallen](https://gist.github.com/rogerallen/1583593)



```python
us_state_abbrev = {
    'Alabama': 'AL',
    'Alaska': 'AK',
    'Arizona': 'AZ',
    'Arkansas': 'AR',
    'California': 'CA',
    'Colorado': 'CO',
    'Connecticut': 'CT',
    'Delaware': 'DE',
    'Florida': 'FL',
    'Georgia': 'GA',
    'Hawaii': 'HI',
    'Idaho': 'ID',
    'Illinois': 'IL',
    'Indiana': 'IN',
    'Iowa': 'IA',
    'Kansas': 'KS',
    'Kentucky': 'KY',
    'Louisiana': 'LA',
    'Maine': 'ME',
    'Maryland': 'MD',
    'Massachusetts': 'MA',
    'Michigan': 'MI',
    'Minnesota': 'MN',
    'Mississippi': 'MS',
    'Missouri': 'MO',
    'Montana': 'MT',
    'Nebraska': 'NE',
    'Nevada': 'NV',
    'New Hampshire': 'NH',
    'New Jersey': 'NJ',
    'New Mexico': 'NM',
    'New York': 'NY',
    'North Carolina': 'NC',
    'North Dakota': 'ND',
    'Ohio': 'OH',
    'Oklahoma': 'OK',
    'Oregon': 'OR',
    'Pennsylvania': 'PA',
    'Rhode Island': 'RI',
    'South Carolina': 'SC',
    'South Dakota': 'SD',
    'Tennessee': 'TN',
    'Texas': 'TX',
    'Utah': 'UT',
    'Vermont': 'VT',
    'Virginia': 'VA',
    'Washington': 'WA',
    'West Virginia': 'WV',
    'Wisconsin': 'WI',
    'Wyoming': 'WY',
}
```


```python
# Now lets look at a map of collections per state, by subtype. First we have the sum the number of records by state.
# Should create a new dataframe with aggregate data by state. Want total number records, total number per subtype. Is this a pivot table?


df_byState = df_fluA_Human.groupby(by=['State/Province', 'Subtype']).size().reset_index(name='counts')


```


```python
us_state_abbrev = {state: abbrev for abbrev, state in us_state_abbrev.items()}

df_byState['abbrev'] = df_byState['State/Province'].map(us_state_abbrev)
df_byState.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State/Province</th>
      <th>Subtype</th>
      <th>counts</th>
      <th>abbrev</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>H1N1</td>
      <td>232</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Alabama</td>
      <td>H3N2</td>
      <td>207</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alaska</td>
      <td>H1N1</td>
      <td>208</td>
      <td>AK</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alaska</td>
      <td>H3N2</td>
      <td>462</td>
      <td>AK</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Arizona</td>
      <td>H1N1</td>
      <td>216</td>
      <td>AZ</td>
    </tr>
  </tbody>
</table>
</div>



Now going to plot it out on map


```python
# install plotly and allow us to view inside the notebook

import plotly.graph_objs as go 
from plotly.offline import init_notebook_mode,iplot,plot
init_notebook_mode(connected=True) 
```


<script>requirejs.config({paths: { 'plotly': ['https://cdn.plot.ly/plotly-latest.min']},});if(!window.Plotly) {{require(['plotly'],function(plotly) {window.Plotly=plotly;});}}</script>



```python
#Let's do H1N1 cases first.

#df_byState_H1N1 = df_byState_H1N1.copy()
df_byState_H1N1 = df_byState[df_byState['Subtype'] == 'H1N1']
df_byState_H1N1.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State/Province</th>
      <th>Subtype</th>
      <th>counts</th>
      <th>abbrev</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alabama</td>
      <td>H1N1</td>
      <td>232</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alaska</td>
      <td>H1N1</td>
      <td>208</td>
      <td>AK</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Arizona</td>
      <td>H1N1</td>
      <td>216</td>
      <td>AZ</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Arkansas</td>
      <td>H1N1</td>
      <td>80</td>
      <td>AR</td>
    </tr>
    <tr>
      <th>8</th>
      <td>California</td>
      <td>H1N1</td>
      <td>607</td>
      <td>CA</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_H1N1 = dict(
        type = 'choropleth',
        colorscale = 'Greens',
        reversescale = True,
        locations = df_byState_H1N1['abbrev'],
        z = df_byState_H1N1['counts'],
        locationmode = 'USA-states',
        text = ['State/Province'],
        marker = dict(line = dict(color = 'rgb(255,255,255)',width = 1)),
        colorbar = {'title':'reported cases'}
            ) 
```


```python
layout = dict(title = 'Reported/Sequenced Influenza H1N1 for 2017-2018',
              geo = dict(scope='usa',
                         showlakes = True,
                         lakecolor = 'rgb(85,173,240)')
             )
```


```python
choromap = go.Figure(data = [data_H1N1],layout = layout)
iplot(choromap,validate=False)
```


<div id="e968b8a5-61b5-4bad-8882-274f7f242846" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("e968b8a5-61b5-4bad-8882-274f7f242846", [{"colorbar": {"title": "reported cases"}, "colorscale": "Greens", "locationmode": "USA-states", "locations": ["AL", "AK", "AZ", "AR", "CA", "CO", "CT", "DE", null, "FL", "GA", "HI", "ID", "IL", "IN", "IA", "KS", "KY", "LA", "ME", "MD", "MA", "MI", "MN", "MS", "MO", "MT", "NE", "NV", "NH", "NJ", "NM", "NY", "NC", "ND", "OH", "OK", "OR", "PA", "RI", "SC", "SD", "TN", "TX", "UT", "VT", "VA", "WA", "WV", "WI", "WY"], "marker": {"line": {"color": "rgb(255,255,255)", "width": 1}}, "reversescale": true, "text": ["State/Province"], "z": [232, 208, 216, 80, 607, 112, 152, 112, 104, 299, 96, 184, 96, 256, 144, 288, 56, 120, 200, 80, 248, 72, 184, 152, 192, 160, 120, 16, 24, 80, 152, 56, 151, 128, 160, 80, 168, 208, 264, 16, 24, 72, 88, 264, 192, 40, 175, 192, 144, 152, 128], "type": "choropleth", "uid": "12f9162a-ed18-11e8-ba9d-a45e60ee8a67"}], {"geo": {"lakecolor": "rgb(85,173,240)", "scope": "usa", "showlakes": true}, "title": "Reported/Sequenced Influenza H1N1 for 2017-2018"}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>



```python
# Here is a map for H3N2

df_byState_H3N2 = df_byState[df_byState['Subtype'] == 'H3N2']
df_byState_H3N2.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State/Province</th>
      <th>Subtype</th>
      <th>counts</th>
      <th>abbrev</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Alabama</td>
      <td>H3N2</td>
      <td>207</td>
      <td>AL</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alaska</td>
      <td>H3N2</td>
      <td>462</td>
      <td>AK</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Arizona</td>
      <td>H3N2</td>
      <td>392</td>
      <td>AZ</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Arkansas</td>
      <td>H3N2</td>
      <td>277</td>
      <td>AR</td>
    </tr>
    <tr>
      <th>9</th>
      <td>California</td>
      <td>H3N2</td>
      <td>1621</td>
      <td>CA</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_H3N2 = dict(
        type = 'choropleth',
        colorscale = 'Blues',
        reversescale = True,
        locations = df_byState_H3N2['abbrev'],
        z = df_byState_H3N2['counts'],
        locationmode = 'USA-states',
        text = ['State/Province'],
        marker = dict(line = dict(color = 'rgb(255,255,255)',width = 1)),
        colorbar = {'title':'reported cases'}
            ) 
```


```python
layout = dict(title = 'Reported/Sequenced Influenza H3N2 for 2017-2018',
              geo = dict(scope='usa',
                         showlakes = True,
                         lakecolor = 'rgb(85,173,240)')
             )
```


```python
choromap = go.Figure(data = [data_H3N2],layout = layout)
iplot(choromap,validate=False)
```


<div id="a537be11-8274-4225-b7f3-482dd8cdcb81" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("a537be11-8274-4225-b7f3-482dd8cdcb81", [{"colorbar": {"title": "reported cases"}, "colorscale": "Blues", "locationmode": "USA-states", "locations": ["AL", "AK", "AZ", "AR", "CA", "CO", "CT", "DE", null, "FL", "GA", "HI", "ID", "IL", "IN", "IA", "KS", "KY", "LA", "ME", "MD", "MA", "MI", "MN", "MS", "MO", "MT", "NE", "NV", "NH", "NJ", "NM", "NY", "NC", "ND", "OH", "OK", "OR", "PA", "RI", "SC", "SD", "TN", "TX", "UT", "VT", "VA", "WA", "WV", "WI", "WY"], "marker": {"line": {"color": "rgb(255,255,255)", "width": 1}}, "reversescale": true, "text": ["State/Province"], "z": [207, 462, 392, 277, 1621, 232, 276, 264, 176, 736, 280, 398, 264, 487, 231, 640, 231, 198, 216, 183, 463, 160, 270, 240, 120, 400, 464, 144, 181, 192, 207, 232, 359, 285, 271, 152, 224, 175, 312, 56, 96, 383, 207, 544, 277, 128, 295, 311, 184, 320, 208], "type": "choropleth", "uid": "41249088-ed18-11e8-ba9d-a45e60ee8a67"}], {"geo": {"lakecolor": "rgb(85,173,240)", "scope": "usa", "showlakes": true}, "title": "Reported/Sequenced Influenza H3N2 for 2017-2018"}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>


That's enough fun for now. I will tackle the sequences and how they related to this data in the next part of this series.

  
  
