# Influenza Vaccine/Genetics Analysis (currently being updated)

---

This year in the flu tent line, I overhear talk that piques my interest. Patients are chatting about whether the shot will work. 

Worrying about the flu is a valid concern - and as mother of small children, I've never thought twice about getting our shots. After all, the vaccine is currently our only technology to fend off the flu.

But why is our only technology still somewhat faulty? Influenza was discovered early in the last century. The vaccine was created around the time of WWII.

### **Why, in modern times, do we still have to wonder every season about the flu vaccine’s efficacy?**

---

## Influenza Vaccine Background

One well understood cause for reduced vaccine efficacy is the time lag. It takes months from when vaccine strains are chosen and manufactured, and then publicly distributed - ample time for influenza to mutate its way out of vaccine formulations. 

Though interestingly, even when well-matched to circulating strains, vaccines can still provide low efficacy. A recent paper by [Lewnard and Coby](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6027411/) explains a couple reasons why; first, current vaccine strains can cross-react with prior vaccinations and influenza-caused illnesses to either boost or reduce immunological responses, and second, the vaccine just flat out doesn't work on some people. 

One great point the authors make is that if mutations were the sole reason for decreased vaccine efficacy, statistically speaking, we'd be winning the seasonal flu game by now. But we’re not, which is an indication that there are other confounding factors in the mix that we don't yet understand. 

How then to improve our chances of a better working vaccine? One of the suggestions from the authors is to study the "effect of past influenza exposures on host responses to vaccination."

And with this, my data analysis begins.

---

## Project Details

Here I will show how to look at large subsets of data using influenza records from last season. This first part of the analysis will be a general assessment of the data (e.g. demographics, categorizing strains and subtypes), as I often like to get my bearings before figuring out what useful information can be extracted.

A common problem I encounter in my work is combing through massive amounts of data - it can be records, like what I'm going to be working with below, or tons of sequences that I need to investigate for say, a consensus alignment. I figured out pretty early on that Excel wasn't cutting it for investigating 20,000 genetic sequences at once. 

Using Python for data analysis, I'm going to be initially answering:

   ### 1. What strains were included in the 2017-18 vaccine?
   ### 2. What subtypes and strains were most common in the US in 2017-18?
   
A follow up on this analysis will answer: 

   ### 3. How similar were the recommended vaccine strains to the reported sequences during the 2017-18 season?

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

## Question 2 and 3 will require [analyzing records from a flu database, which is in the attached notebook.](https://github.com/Adrianapip/Influenza_Vaccine/blob/master/Influenza_1129_1.ipynb)
