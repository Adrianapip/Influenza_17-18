# Influenza Vaccine/Genetics Analysis (currently being updated)

---

This data analysis is inspired by my annual task of getting flu shots. As a mother of small children, I've never thought twice about getting our shots. It just gets done, and we hope for the best. As we wait, I overhear chatter among other patients about whether the shot will work.

I began to think of my beloved microbiology factoids  - wasn’t influenza was discovered early in the last century? The vaccine was created around the time of WWII. Why are we still worrying about the vaccine’s efficacy every season?

To answer this question, I thought it’d be interesting to gather and analyze some influenza datasets from last season (17-18). 

---

This project aims to tackle two common issues I often encounter. 

First is the type of data. Most analyses are quite thorough in that they deeply examine either the demographics (i.e., ages, gender, location, etc.) OR sequence data (in depth topics such as mutation rates or phylogenetic studies). Both in-depth analyses are highly useful in their respective fields, but is sometimes too detailed for a general overview of the topic. 

Second is the amount of data. I found out pretty early on that the amount of data required for these analyses will often exceed common spreadsheet application’s (e.g., Excel) capabilities. It was time to move onto more powerful, streamlined methods. 

In this project, I’m going to lightly examine and combine both demographics/sequence data in one tidy analysis using Python (and some essential libraries, such as matplotlib, BioPython, and Pandas.) The purpose is to create a reproducible template applicable to any introductory study of infectious disease data. As someone with a background of biochemistry, microbiology, and a bit of computational biology, I often perform these types of analyses to establish a strong foundation of general knowledge in both epidemiology and genetics. 

---

## Influenza Vaccine Background

One well understood cause for reduced vaccine efficacy is the time lag. It takes months from when vaccine strains are chosen and manufactured, and then publicly distributed - ample time for influenza to mutate its way out of vaccine formulations. 

Though interestingly, even when well-matched to circulating strains, vaccines can still provide low efficacy. A recent paper by [Lewnard and Coby](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6027411/) explains a couple reasons why; first, current vaccine strains can cross-react with prior vaccinations and influenza-caused illnesses to either boost or reduce immunological responses, and second, the vaccine just flat out doesn't work on some people. 

One great point the authors make is that if mutations were the sole reason for decreased vaccine efficacy, statistically speaking, we'd be winning the seasonal flu game by now. But we’re not, which is an indication that there are other confounding factors in the mix that we don't yet understand. 

How then to improve our chances of a better working vaccine? One of the suggestions from the authors is to study the "effect of past influenza exposures on host responses to vaccination."

My background analysis findings are that there are factors in the seasonal flu season that we haven’t figured out yet, and to keep studying past seasons to gain a better understanding and predicting outbreak severities. 

To that end, in combination with the general questions outlined above, I have come up with some key questions I hope to answer:

1. What strains were included in the 2017-18 vaccine?

2. What subtypes and strains were most common in the US in 2017-18?

3. How similar were the recommended vaccine strains to the reported sequences during the 2017-18 season?

---

### Question 1: What strains were included in the 2017-18 vaccine?

Last flu season (2017-18) was severe compared to previous years. The CDC classified the season as having an overall "High" severity across all age groups. Previous years yielded largely low to moderate seasons.

(What exactly does “High Severity” mean? There are many factors that contribute to this classification, but mainly it’s based on flu-related hospitalizations and deaths. To explore the details further, here are excellent resources/graphs tracking this data: the US monitoring system [FluView](https://gis.cdc.gov/grasp/fluview/fluportaldashboard.html) and the [WHO's FluNet](https://www.who.int/influenza/resources/charts/en/).)

Early in 2017, the WHO announced its recommended composition of the influenza vaccine:

* A/Michigan/45/2015 (H1N1) pdm09-like virus
* A/Hong Kong/4801/2014 (H3N2)-like virus
* B/Brisbane/60/2008-like virus
* B/Phuket/3073/2013-like virus (for quadrivalent vaccine)

For these recommendations, an expert panel examines:
* recent surveillance data of the virus; 
* clinical antibody studies of currently circulating viruses; and 
* predictive models of genetic/antigenic changes.

Each country then formulates their own vaccine based on regional requirements and observations.

## Question 2 and 3 will require [analyzing records from a flu database, which is in the attached notebook.](https://github.com/Adrianapip/Influenza_Vaccine/blob/master/Influenza_1204.ipynb)
