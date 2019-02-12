# t TEST FOR THE MEAN OF 1 SAMPLE






## Required Packages 


```r
library(tidyverse)    # Loads several very helpful 'tidy' packages
library(haven)        # Read in SPSS datasets
```


## Example Dataset - Cancer Experiment 

#### Source of Data {-}

Mid-Michigan Medical Center, Midland, Michigan, 1999: A  study of oral condition of cancer patients.

#### Description of the Study {-}

The data set contains part of the data for a study of oral condition of cancer patients conducted at the Mid-Michigan Medical Center.  The oral conditions of the patients were measured and recorded at the initial stage, at the end of the second week, at the end of the fourth week, and at the end of the sixth week.  The variables age, initial weight and initial cancer stage of the patients were recorded.  Patients were divided into two groups at random:  One group received a placebo and the other group received aloe juice treatment.

Sample size $n = 25$ patients with neck cancer. The treatment is **Aloe Juice**. 

#### Variables Included {-}

* `ID` patient identification number

* `trt` treatment group 
    + `0` *placebo* 
    + `1` *aloe juice*

* `age` patient's age, *in years*

* `weightin` patient's weight *(pounds)* at the initial stage

* `stage`	initial cancer stage
    + coded `1` through `4`

* `totalcin` oral condition at the *initial stage*
* `totalcw2` oral condition at the end of *week 2*
* `totalcw4` oral condition at the end of *week 4*
* `totalcw6` oral condition at the end of *week 6*








```r
head(cancer_raw)
```

```
# A tibble: 6 x 9
     ID   TRT   AGE WEIGHIN STAGE TOTALCIN TOTALCW2 TOTALCW4 TOTALCW6
  <dbl> <dbl> <dbl>   <dbl> <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
1     1     0    52    124      2        6        6        6        7
2     5     0    77    160      1        9        6       10        9
3     6     0    60    136.     4        7        9       17       19
4     9     0    61    180.     1        6        7        9        3
5    11     0    59    176.     2        6        7       16       13
6    15     0    69    168.     1        6        6        6       11
```




## 1 Sample Mean vs. historic control

example: Do the patients weigh more than 165 pounds at intake, on average?



```r
cancer_clean %>% 
  dplyr::pull(weighin) %>% 
  t.test(mu = 165)
```

```

	One Sample t-test

data:  .
t = 2.0765, df = 24, p-value = 0.04872
alternative hypothesis: true mean is not equal to 165
95 percent confidence interval:
 165.0807 191.4793
sample estimates:
mean of x 
   178.28 
```



## Change the Confidence Level

Find a 99% confience level for the population mean weight.


```r
cancer_clean %>% 
  dplyr::pull(weighin) %>% 
  t.test(mu = 165,
         conf.level = 0.99)
```

```

	One Sample t-test

data:  .
t = 2.0765, df = 24, p-value = 0.04872
alternative hypothesis: true mean is not equal to 165
99 percent confidence interval:
 160.3927 196.1673
sample estimates:
mean of x 
   178.28 
```


## Restrict to a Subsample

Do the patients with .dcoral[stage 3 & 4] cancer weigh more than 165 pounds at intake, on average?


```r
cancer_clean %>% 
  dplyr::filter(stage %in% c("3", "4")) %>% 
  dplyr::pull(weighin) %>% 
  t.test(mu = 165)
```

```

	One Sample t-test

data:  .
t = 0.82627, df = 5, p-value = 0.4463
alternative hypothesis: true mean is not equal to 165
95 percent confidence interval:
 137.0283 219.4717
sample estimates:
mean of x 
   178.25 
```

