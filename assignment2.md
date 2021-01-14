
# Assignment 2
For this assignment you'll be looking at 2017 data on immunizations from the CDC. Your datafile for this assignment is in [assets/NISPUF17.csv](assets/NISPUF17.csv). A data users guide for this, which you'll need to map the variables in the data to the questions being asked, is available at [assets/NIS-PUF17-DUG.pdf](assets/NIS-PUF17-DUG.pdf). **Note: you may have to go to your Jupyter tree (click on the Coursera image) and navigate to the assignment 2 assets folder to see this PDF file).**

## Question 1
Write a function called `proportion_of_education` which returns the proportion of children in the dataset who had a mother with the education levels equal to less than high school (<12), high school (12), more than high school but not a college graduate (>12) and college degree.

*This function should return a dictionary in the form of (use the correct numbers, do not round numbers):* 
```
    {"less than high school":0.2,
    "high school":0.4,
    "more than high school but not college":0.2,
    "college":0.2}
```



```python
import pandas as pd 
DF = pd.read_csv("assets/NISPUF17.csv")
#DF["EDUC1"].unique()
DF.head()
#print(len(DF))
def proportion_of_education():
    r = len(DF)
    a = len(DF.loc[DF["EDUC1"]==1])/r
    b = len(DF.loc[DF["EDUC1"]==2])/r
    c = len(DF.loc[DF["EDUC1"]==3])/r
    d = len(DF.loc[DF["EDUC1"]==4])/r
    
    dic = {"less than high school":a,
          "high school":b,
          "more than high school but not college":c,
          "college":d}
   
    
    
    return dic
```


```python
assert type(proportion_of_education())==type({}), "You must return a dictionary."
assert len(proportion_of_education()) == 4, "You have not returned a dictionary with four items in it."
assert "less than high school" in proportion_of_education().keys(), "You have not returned a dictionary with the correct keys."
assert "high school" in proportion_of_education().keys(), "You have not returned a dictionary with the correct keys."
assert "more than high school but not college" in proportion_of_education().keys(), "You have not returned a dictionary with the correct keys."
assert "college" in proportion_of_education().keys(), "You have not returned a dictionary with the correct keys."

```

## Question 2

Let's explore the relationship between being fed breastmilk as a child and getting a seasonal influenza vaccine from a healthcare provider. Return a tuple of the average number of influenza vaccines for those children we know received breastmilk as a child and those who know did not.

*This function should return a tuple in the form (use the correct numbers:*
```
(2.5, 0.1)
```


```python
DF = pd.read_csv("assets/NISPUF17.csv")
#P_NUMFLU column for influenza vaccines count
def average_influenza_doses():
    import numpy as np
    r = len(DF)
    A = DF.loc[DF["CBF_01"]==1] #Received breastmilk
    B = DF.loc[DF["CBF_01"]==2] #Did not received breastmilk
    a = np.mean(A["P_NUMFLU"])
    b = np.mean(B["P_NUMFLU"])
    
    

    
    return (a, b)

#print(average_influenza_doses())
```

    (1.8799187420058687, 1.5963945918878317)



```python
assert len(average_influenza_doses())==2, "Return two values in a tuple, the first for yes and the second for no."

```

## Question 3
It would be interesting to see if there is any evidence of a link between vaccine effectiveness and sex of the child. Calculate the ratio of the number of children who contracted chickenpox but were vaccinated against it (at least one varicella dose) versus those who were vaccinated but did not contract chicken pox. Return results by sex. 

*This function should return a dictionary in the form of (use the correct numbers):* 
```
    {"male":0.2,
    "female":0.4}
```

Note: To aid in verification, the `chickenpox_by_sex()['female']` value the autograder is looking for starts with the digits `0.0077`.


```python
DF = pd.read_csv("assets/NISPUF17.csv")




#print(len(DF.loc[DF["P_NUMVRC"]== nan]))
""""
print(len(DF.loc[DF["P_NUMVRC"] >= 1.]))

print(len(DF.loc[DF["P_NUMVRC"] < 1.]))

#print(len(DF.dropna(columns="P_NUMVRC")))



print(DF["P_NUMVRC"].count())


print(len(DF.loc[DF["P_NUMVRC"] >= 1.])+len(DF.loc[DF["P_NUMVRC"] < 1.]))
"""
def chickenpox_by_sex():
    
    
    r = DF["P_NUMVRC"].count()
    
    A = DF.loc[DF["P_NUMVRC"] >= 1.] #those who were vacinated
    
    A1 = A.loc[A["HAD_CPOX"]==1]     #those who were vacinated and had catapora
    A2 = A.loc[A["HAD_CPOX"]==2]     #those who were vacinated and had not catapora
    A1m = A1.loc[A1["SEX"]==1]       #A1 male, e.g., vacinated + catapora
    A1f = A1.loc[A1["SEX"]==2]       #A1 female, e.g., vacinated + catapora
    
    A2m = A2.loc[A2["SEX"]==1]       #A2 male, e.g., vacinated + NOT catapora
    A2f = A2.loc[A2["SEX"]==2]       #A2 female, e.g., vacinated + NOT catapora
    
    
    '''Calculate the ratio of the number of children who contracted chickenpox but were vaccinated against it
    (at least one varicella dose) versus those who were vaccinated but did not contract chicken pox. Return results by sex. 
    '''
    
    
    #print(len(A1m), len(A1f))
    #print(len(A2m), len(A2f))
    #print(A["HAD_CPOX"].unique(), len(A))
    
    
    
    
    
    a = len(A1m)/len(A2m)
    b = len(A1f)/len(A2f)
    
    dic = {"male": a, 
          "female": b}
    
    
    return dic
    # YOUR CODE HERE
    #raise NotImplementedError()
    
print(chickenpox_by_sex())


```

    {'male': 0.009675583380762664, 'female': 0.0077918259335489565}



```python
assert len(chickenpox_by_sex())==2, "Return a dictionary with two items, the first for males and the second for females."

```

## Question 4
A correlation is a statistical relationship between two variables. If we wanted to know if vaccines work, we might look at the correlation between the use of the vaccine and whether it results in prevention of the infection or disease [1]. In this question, you are to see if there is a correlation between having had the chicken pox and the number of chickenpox vaccine doses given (varicella).

Some notes on interpreting the answer. The `had_chickenpox_column` is either `1` (for yes) or `2` (for no), and the `num_chickenpox_vaccine_column` is the number of doses a child has been given of the varicella vaccine. A positive correlation (e.g., `corr > 0`) means that an increase in `had_chickenpox_column` (which means more no’s) would also increase the values of `num_chickenpox_vaccine_column` (which means more doses of vaccine). If there is a negative correlation (e.g., `corr < 0`), it indicates that having had chickenpox is related to an increase in the number of vaccine doses.

Also, `pval` is the probability that we observe a correlation between `had_chickenpox_column` and `num_chickenpox_vaccine_column` which is greater than or equal to a particular value occurred by chance. A small `pval` means that the observed correlation is highly unlikely to occur by chance. In this case, `pval` should be very small (will end in `e-18` indicating a very small number).

[1] This isn’t really the full picture, since we are not looking at when the dose was given. It’s possible that children had chickenpox and then their parents went to get them the vaccine. Does this dataset have the data we would need to investigate the timing of the dose?


```python
DF = pd.read_csv("assets/NISPUF17.csv")
import scipy.stats as stats
import numpy as np
import pandas as pd


def corr_chickenpox():
    #Cleaning the DataFrame
    B = DF.loc[DF["HAD_CPOX"].isin([1,2])] #Selecting only those who had catapora, e.g,  simply excluding Nan and others
    
    A = B.loc[DF["P_NUMVRC"] >= 0.]        #Selecting only valid values for calculating, excluding NaN
    
    #print(B["P_NUMVRC"].unique())
    #print(A["P_NUMVRC"].unique())
    
   

    # here is some stub code to actually run the correlation
    corr, pval=stats.pearsonr(A["HAD_CPOX"],A["P_NUMVRC"])
    print(corr, pval)
    # just return the correlation
    return corr

    # YOUR CODE HERE

    
    
    
    

```


```python
assert -1<=corr_chickenpox()<=1, "You must return a float number between -1.0 and 1.0."

```

    0.07044873460147986 2.7780263182916748e-18



```python

```
