---
title       : How Antibacterial Dose, Dosing Frequency and Drug Parameters affect treatment efficacy?
author      : Hechuan
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     :  [mathjax]    # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Why you should know Pharmacokinetic-Pharmacodynamic Modeling and Simulation?

1. An important tool for rational drug development and drug use
2. Characterizing the relationships between dose, concentration, and desired effects and side effects 
3. Help clinical dose selection and new antibiotics regimen development

* Next slide will describe how PK-PD model differential equations can be produced in R through `deSolve` package

--- .class #id 

## How the PK-PD model was created in R? 


```r
library(deSolve)
DES <- function(T, A, THETA) {
        KA  <- THETA[1]
        K10 <- THETA[2]
        Kgrowth_base <- THETA[3]
        Kdeath <- THETA[4]
        Emax <- THETA[5]
        EC50 <- THETA[6]
        gamma <- THETA[7]
        dA <- vector(length = 3)
        dA[1] = -KA*A[1]		#Absorption compartment
        dA[2] =  KA*A[1] -K10*A[2]	#Central compartment  
        dA[3] =  A[3]*Kgrowth_base*(1-A[3]/500000000)-
                (Kdeath+Emax*A[2]**gamma/(A[2]**gamma+EC50**gamma))*A[3] # Bacteria Comapartment
        list(dA)
}
```


--- .class #id 

## How Does The App Help Understand How Dose and Dosing Frequncy affect treatment efficacy of Antibiotics:

<left><img src=./assets/img/0mg.png height='48%' width='48%' style='margin:0px; border: 2px solid #FF0000'/></center>
<right><img src=./assets/img/100mgBID.png height='48%' width='48%' style='margin:0px; border: 2px solid #FF0000'/></center>
Bacterial count profiles without treatment (Left plot) and with 100 mg antibiotics twice a day for 10 days (Right plot)

<center>https://hechuan.shinyapps.io/PK-PD-Modeling-of-Antibiotics/</center>


--- .class #id 

## How Does The App Help Understand How Drug parameters Affect treatment efficacy:

<left><img src=./assets/img/100mgBID.png height='48%' width='48%' style='margin:0px; border: 2px solid #FF0000'/></center>
<right><img src=./assets/img/100mgBID_1.png height='48%' width='48%' style='margin:0px; border: 2px solid #FF0000'/></center>
Both bacterial count profiles were under the treatment of 100mg twice a day, but drug related parameters (Emax and EC50) were different in the two plots, compared to the left plot, the drug in the right plot have poorer efficacy and potency (lower Emax, and higher EC50).  
<center>https://hechuan.shinyapps.io/PK-PD-Modeling-of-Antibiotics/</center>



