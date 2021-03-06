
[<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/banner.png" width="880" alt="Visit QuantNet">](http://quantlet.de/index.php?p=info)

## [<img src="https://github.com/QuantLet/Styleguide-and-Validation-procedure/blob/master/pictures/qloqo.png" alt="Visit QuantNet">](http://quantlet.de/) **MVAclususcrime** [<img src="https://github.com/QuantLet/Styleguide-and-Validation-procedure/blob/master/pictures/QN2.png" width="60" alt="Visit QuantNet 2.0">](http://quantlet.de/d3/ia)

```yaml

Name of QuantLet : MVAclususcrime

Published in : Applied Multivariate Statistical Analysis

Description : 'Interprets the 7 crime variables of the dataset as contingency table and computes
the distance between the US states depending on these variables for building appropriate clusters.'

Keywords : cluster-analysis, contingency-table, distance, sas

See also : 'MVAclus8p, MVAclusbank, MVAspecclust, MVAclusfood, MVAclusbh, SMSclus8p, SMSclus8pd,
SMSclus8pmst2, SMSclusbank, SMScluscomp, SMScluscrime, SMScluscrimechi2, SMSclushealth'

Author : Simon Trimborn, Awdesch Melzer

Author[SAS] : Svetlana Bykovskaya

Submitted : Wed, October 01 2014 by Awdesch Melzer

Submitted[SAS] : Wen, April 6 2016 by Svetlana Bykovskaya

Datafile : uscrime.rda

Output : Distance matrix for the 7 crime variables.

```


### R Code:
```r

# clear all variables
rm(list = ls(all = TRUE))
graphics.off()

# load data
load("uscrime.rda")
X = uscrime[, 3:9]

row.s    = apply(X, 1, sum)        # row sums
column.s = apply(X, 2, sum)        # column sums
mat.s    = sum(X)                  # matrix sum
D        = matrix(0, nrow = 50, ncol = 50)

# distance for rows
for (i in 1:(dim(X)[1] - 1)) {
    for (j in (i + 1):dim(X)[1]) {
        for (z in 1:dim(X)[2]) {
            D[i, j] = 1/(column.s[z]/mat.s) * ((X[i, z]/row.s[i]) - (X[j, z]/row.s[j]))^2
        }
    }
}
D 

```

### SAS Code:
```sas

* Import the data;
data uscrime;
  infile '/folders/myfolders/data/uscrime_sas.dat';
  input temp1-temp11;
run;

proc iml;
  * Read data into a matrix;
  use uscrime;
    read all var _ALL_ into x; 
  close uscrime;
  
  x  = x[, 3:9];
  n1 = nrow(x);
  n2 = ncol(x);
  
  rows = x[,+];
  cols = x[+,];
  mat  = x[+,+];  
  D    = j(50, 50, 0);
  
  do i = 1 to n1 - 1;
    do j = i + 1 to n1;
      do z = 1 to n2;
         D[i, j] = 1/(cols[z]/mat) * ((x[i, z]/rows[i]) - (x[j, z]/rows[j])) ## 2;
      end;
    end;
  end;
  
  print D;
quit;
```
