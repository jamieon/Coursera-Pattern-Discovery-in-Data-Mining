---
title: "week2"
output: html_document
---
#### Problem 1.
Suppose a school collected some data on students’ preference for hot dogs(HD) vs. hamburgers(HM). We have the following 2×2 contingency table summarizing the statistics. If lift is used to measure the correlation between HD and HM, what is the value for lift(HD, HM)?  

|                | HD          | ¬HD          | Sigma_row    |
|:--------------:|:-----------:|:------------:|:------------:|
| **HM**         |      40     |     24       |  64          | 
| **¬HM**        |      210    |    126       |  336         |
| **Sigma_col**  |      250    |    150       |  400         |



```r
lift <- function(input) {
    sigmaRow <- sum(input[1:2])
    sigmaCol <- sum(input[1], input[3])
    sigmaAll <- sum(input)
    suppAUB <- input[1] / sigmaAll
    suppA <- sigmaCol / sigmaAll
    suppB <- sigmaRow / sigmaAll
    lift <- suppAUB / (suppA * suppB)
    return(lift)
}
input <- c(40, 24, 210, 126)
lift(input)
```

```
## [1] 1
```

```r
input <- c(300, 700, 1200, 800)
lift(input)
```

```
## [1] 0.6
```

#### Problem 2.
Suppose we are interested in analyzing the purchase of comics (CM) and fictions(FC) in the transaction history of a bookstore. We have the following 2 × 2 contingency table summarizing the transactions. If χ2 is used to measure the correlation between CM and FC, what is the χ2 score?  

|                | CM          | ¬CM          | Sigma_row    |
|:--------------:|:-----------:|:------------:|:------------:|
| **FC**         |      300    |     700      |  1000        | 
| **¬FC**        |     1200    |     800      |  2000        |
| **Sigma_col**  |     1500    |    1500      |  3000        |


```r
chiSquare <- function(input) {
    sigmaRow1 <- sum(input[1:2])
    sigmaCol1 <- sum(input[1], input[3])
    sigmaAll <- sum(input)
    exp1 <- sigmaCol1 * sigmaRow1 / sigmaAll
    exp3 <- sigmaCol1 - exp1
    exp2 <- sigmaRow1 - exp1
    exp4 <- sigmaAll - sigmaCol1 - exp2
    expInput <- c(exp1, exp2, exp3, exp4)
    chiSq <- 0
    for (i in 1:4) {
        chiSq = chiSq + (input[i] - expInput[i])^2 / expInput[i]
    }
    return(chiSq)
}
input <- c(300, 700, 1200, 800)
chiSquare(input)
```

```
## [1] 240
```

```r
input <- c(700, 300, 500, 1500)
chiSquare(input)
```

```
## [1] 562.5
```
#### Problem 3 & 4.
What is the value range of the XXX measure?  
Which of the following measures is null invariant?  

|                | Range         | Null-Invariant |
|:--------------:|:-------------:|:--------------:|
| **ChiSquare**  |   [0, inf]    |     N          | 
| **Lift**       |   [0, inf]    |     N          |
| **AllConf**    |   [0, 1]      |     Y          |
| **Jaccard**    |   [0, 1]      |     Y          |
| **Cosine**     |   [0, 1]      |     Y          |
| **Kulczynski** |   [0, 1]      |     Y          |
| **MaxConf**    |   [0, 1]      |     Y          |

#### Problem 5.  
Both Kulcyzynski and Cosine are null invariant measures and thus not sensitive to the null transactions, which vary dramatically for different supermarkets.  

#### Problem 6.  
A store had 100,000 total transactions in Q4 2014. 10,000 transactions contained eggs, while 5,000 contained bacon. 2000 transactions contained both eggs and bacon. Which of the following choices for the value of ε is the smallest such that {eggs, bacon} is considered a negative pattern under the null-invariant definition?  

```r
nullVar <- function(input) {
    # Support based
    supA <- input[2] / input[1]
    supB <- input[3] / input[1]
    supAUB <- input[4] / input[1]
    print(supA * supB - supAUB < 0)
    
    # Null-invariant
    pAB <- input[4] / input[3]
    pBA <- input[4] / input[2]
    result <- (pAB + pBA) / 2 
    return(result)
}
input <- c(100000, 10000, 5000, 2000)
nullVar(input) # choose the smallest number that's greater than this
```

```
## [1] TRUE
```

```
## [1] 0.3
```

```r
input <- c(100000, 10000, 5000, 600)
nullVar(input) # choose the smallest number that's greater than this
```

```
## [1] TRUE
```

```
## [1] 0.09
```

#### Problem 7.   
|    Pat-ID    |      Item-Sets      |   Support    |
|:------------:|:-------------------:|:------------:|
|      P1      |   {A, C, E, S}      |   205227     | 
|      P2      |  {F, A, C, E, S}    |   205211     |
|      P3      |  {F, A, C, E, T, S} |   101758     |
|      P4      |  {F, A, C, T, S}    |   161563     |
|      P5      |   {A, C, T, S}      |   161576     |


Which of the following patterns in Table 1 is δ-covered by {F, A, C, E, T, S} for δ=0.4? Select all that apply.  


```r
deltaCover <- function(input, p1, delta) {
    idList <- c("P1", "P2", "P4", "P5")
    for (i in 1:length(input)) {
        dist <- 1 - p1 / input[i]
        if(dist <= delta) {
            print(idList[i])
        }
    }
}
p1 <- 101758
delta <- 0.4
input <- c(205226, 205211, 161563, 161576)
deltaCover(input, p1, delta)
```

```
## [1] "P4"
## [1] "P5"
```

#### Problem 8.
| Transactions   | # of Transactions  |
|:--------------:|:------------------:|
| (abe)          |   100              |
| (bcf)          |   100              |
| (acf)          |   100              | 
| (abcef)        |   100              |

(e) is a 0.5-core pattern of (abe) => (abe) is a (2, 0.5)-robust pattern  
(e) is a 0.5-core pattern of (abcef) => (abcef) is a (4, 0.5)-robust pattern  
(f) is a 0.5-core pattern of (acf) and (bcf) => (acf) is a (2, 0.5)-robust pattern, (bcf) is a (2, 0.5)-robust pattern   

