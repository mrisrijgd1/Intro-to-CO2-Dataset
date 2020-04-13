# Intro-to-CO2-Dataset
#This is an intro to `CO2`. This dataset has data from an experiment on the cold tolerance of the grass species Echinochloa crus-galli. With `dim` we can see the dimension of a matrix/data frame. We can see there are `84 observations` and `5 variables`
```{r}
dataset <- CO2
dim(dataset)
```

#We also see that the `variables` are `Plant`, `Type`, `Treatment`, `conc` and `uptake` . Variable `Plant` is and ordered factor. Variables `Type` and  `Treatment` are factors. Variables `conc` and  `uptake` are numbers.
```{r}
str(dataset)
```

## Plotting
#Now we will make a `boxplot`. This boxplot shows me. This boxplot is showing us that the concentration is from 0-1000. The 1 represents Qc1, Qn1, Mc1, Mn1, the 2 represents Qc2, Qn2, Mc2, Mn2 and the 3 represents Qc3, Qn3, Mc3, Mn3. The Concentration of CO2 varies most in plant type 1. Plant type 2 has the least concentration of CO2.
```{r}
boxplot(dataset$conc, dataset$Plant, 20, col="red")
```

#We will now make another boxplot. This plot compares the 12 different types of plants through the relation between the uptake and the concentration. We see that Mc3 has the most uptake of CO2. The most amount of uptake is 44.3 umol/m^2 sec (this is how you measure the uptake of CO2 in plants)and the concentration is 1000 mL/L(this is how you measure of the concentration of CO2 in Plants) in plant type Qc2.
```{r}
require(stats); require(graphics)

coplot(uptake ~ conc | Plant, data = CO2, show.given = FALSE, type = "b")
## fit the data for the first plant
fm1 <- nls(uptake ~ SSasymp(conc, Asym, lrc, c0),
   data = CO2, subset = Plant == "Qn1")
summary(fm1)
## fit each plant separately
fmlist <- list()
for (pp in levels(CO2$Plant)) {
  fmlist[[pp]] <- nls(uptake ~ SSasymp(conc, Asym, lrc, c0),
      data = CO2, subset = Plant == pp)
}
## check the coefficients by plant
print(sapply(fmlist, coef), digits = 3)
```

#Now we will create a Dot plot that shows us the uptake of CO2 in the 2 different types of Treatments in 2 different regions. the Most amount of uptake of CO2 in Plants is in the second type of treatment in the second region.
```{r}
dotchart(dataset$uptake, dataset$Treatment, cex=.7,
   main="Uptake of CO2 in Plants",
   xlab="Uptake", 
   ylab="Treatment")
```

#Now we will make another Dot plot. This dot plot is showing us the concentration of CO2 in the different regions Quebec and Mississippi. In Mississippi the concentration is higher than in Quebec.
```{r}
dotchart(dataset$conc, dataset$Type, cex=.7,
   main="Concentration of CO2 in the different Regions",
   xlab="Concentration", 
   ylab="Type")
```
