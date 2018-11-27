# Influenza Vaccine/Genetics Analysis

---

This year in the flu tent line, I overhear talk that gets my nerd gears moving. Patients are chatting about whether the shot will work. 

Worrying about the flu is a valid concern - and as mother of small children, I've never thought twice about getting our shots. Afterall, the vaccine is currently our only technology for defense.

But why is our only technology still somewhat faulty? Influeza was discovered early in the last century. The vaccine was created around the time of WWII.

###**Why, in modern times, do we still have to wonder about the vaccine’s efficacy every season?**

---

## Some Background

One well understood cause for reduced vaccine efficacy is the time lag between when vaccine strains are chosen and manufactured, and then publicly distributed. This takes months - ample time for influenza to mutate its way out of vaccine formulations. 

Though interestingly, even when well-matched to circulating strains, vaccines can still provide low efficacy. A recent paper by [Lewnard and Coby](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6027411/) explains how: current vaccine strains can cross-react with prior vaccinations and influenza-caused illnesses to either boost or reduce immunological responses, or oddly enough,  the vaccine just flat out doesn't work on some people. 

One great point the authors make is that if mutations were the *sole reason* for decreased vaccine efficacy, we'd be winning the seasonal flu game by now. In other words, there are other confounding factors in the mix that we don't yet understand. 

How then to improve our chances of a better working vaccine? The paper suggests studying the "effect of past influenza exposures on host repsonses to vaccination..." as this will "improve not just seasonal vaccine effectiveness but also predictions of season severity, viral evolution, and the design of universal vaccines..." 

These are certainly big picture problems that we need to address, and are the inspiration for my mini-analysis that follows.

---

## Project Details

Here I will show how to look at large subsets of data using influenza records from last season. This first part of the analysis will be a general assessment of the data (e.g. demographics, categorizing strains and subtypes), as I often like to get my bearings before figuring out what useful information can be extracted.

A common problem I encounter in my work is combing through massive amounts of data - it can be records, like what I'm going to be working with below, or tons of sequences that I need to investigate for say, a consensus alignment. I figured out pretty early on that Excel wasn't cutting it for investigating 20,000 genetic sequences at once. 

Using Python for data analysis, I'm going to be initially answering:

   ### 1. What strains were included in the 2017-18 vaccine?
   ### 2. What subtypes and strains were most common in the US in 2017-18?
   
A follow up on this analysis will answer: 

   ### 3. How similar were the recommended vaccine strains to the reported sequences during the 2017-18 season?
   
