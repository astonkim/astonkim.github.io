---
layout: post
title: 데이터사이언스 개론 Study Note - 1
subtitle: 01. Data Preprocessing
categories: DGU Bachelor of Data Science
tags: [데이터사이언스_개론]
---

## Data Mining Process
## Pipeline

- The workflow of a typical data mining application
	- **Data collection**  (데이터 수집)
	- **Pre-processing**  (전처리: 활용을 위한 정제)
	- **Analytical processing and algorithms** (분석 처리와 알고리즘)
	- **Post-processing** (후처리)

![](https://i.imgur.com/QOnQFUF.png)

<br/>

## Major Tasks in Data Preprocessing

### Data cleaning  

- Fill in missing values, smooth noisy data, identify or remove outliers, and resolve inconsistencies (변수가 누락되어 있는 경우와 잘못 기록된 경우는 어떻게 처리하는지)

### Data integration

- Integration of multiple databases, data cubes, or files (여러 데이터를 합칠 때, 일관성을 해치지 않는 방법)

### Data reduction

- Dimensionality reduction, Data compression (데이터가 너무 클 경우 어떻게 줄이는가)

### Data transformation

- Discretization, Normalization (데이터를 어떻게 변형시키는가)

<br/>

## Data Cleaning

> **📙 NOTE: Data Quality: Why Preprocess the Data?**
> - **Measures for data quality**: A multidimensional view
> - **Accuracy**: 얼마나 정확하게 데이터가 수집이 되었는지. correct or wrong, accurate or not
> - **Completeness**: 기록이 잘 안된 것들은 없는지 not recorded, unavailable, ...
> - **Consistency**: 일관성이 있는지. some modified but some not, dangling, ...
> - **Timeliness**: 얼마나 주기적으로 업데이트가 되는지. timely update?
> - **Believability**: how trustable the data are correct?
> - **Interpretability**: how easily the data can be understood?

<br/>

**현실의 데이터들은 매우 다양하고 더럽다. "Data in the Real World Is Dirty."**


: Lots of potentially incorrect data, 
e.g. instrument faulty(기록하는 장치가 잘못된 경우), human or computer error(인간 또는 컴퓨터 오류), transmission err(전송 오류)


- **Incomplete**: lacking attribute values, lacking certain attributes of interest, or containing only aggregate data.  
(데이터들 사이에 missing value가 존재)
	- e.g. Occupation=“ ” (missing data)

- **Noisy**: containing noise, errors, or outliers  
(데이터 중간에 섞여있는 노이즈, 에러 또는 이상치.)
	- e.g. Salary=“−10” (an error)  

- **Inconsistent**: containing discrepancies in codes or names  
(두 feature 사이의 불일치.)
	- e.g. Age=“42”, Birthday=“03/07/2010”
	- Was rating “1, 2, 3”, now rating “A, B, C”
	- discrepancy between duplicate records  

- **Intentional**: 잘못된 일반화.
	- e.g. 7th of July is everyone's birthday.  

**=> 이런 것들을 적절하게 처리해서 깔끔한 데이터를 만들어야 한다.**

### Incomplete (Missing) Data

- Data is not always available
	- e.g. many tuples have no recorded value for several attributes, such as customer income in sales data
- Missing data may be due to
	- equipment malfunction
	- inconsistent with other recorded data and thus deleted   
	- data not entered due to misunderstanding
	- certain data may not be considered important at the time of entry
	- not register history or changes of the data
- Missing data may need to be inferred

#### Assumption for Missing Data

> **📙 NOTE: Notation**
> - Missing or Not for variable Y: M = 0 (observed), M=1 (missing)
> - Other observed variable X

<br/>

- **Missing Completely at Random (MCAR)**

	- Missingness is independent of attributes and occurs entirely at random: $$P(Y\vert M) = P(Y)$$
	- Unrealistically strong assumption in practice

<br/>

- **Missing at Random (MAR)**

	- Missingness is related to other attributes (missing이 될지 말지가 다른 변수들에 의해 영향을 받는다) : $$P(Y\vert X, M) = P(Y\vert X)$$
	- e.g. 설문조사에서 25세 미만인 사람들의 연봉 칸이 대체로 비어있다. -> 25세 미만은 학생이기 때문에 직업이 없어 연봉이 없을 가능성이 높다. (상관관계)
	- The missingness can be explained by the variables that are actually observed
	- e.g. Missing data on hobbies tend to be more common among individuals with higher incomes

<br/>

- **Missing Not at Random (MNAR)**

	- Missingness is related to unobserved measurements (related to the reason it's missing) : $$P(Y\vert X, M) ≠ P(Y\vert X)$$
	- Informative or non-ignorable missingness
	- e.g. Missing values in education level can imply lower education levels
	- missing된 것이 어떠한 변수에 의한 즉, 원인이 있는데 그 원인이 되는 변수가 데이터에 기록이 되지 않아 파악이 어려운 경우.

<br/>

![](https://i.imgur.com/Co8wqHO.png)

- y: Variable with missing values
- Blue: observed data
- Red: missing data

<br/>

#### How to Handle Missing Data?

- Eliminate data tuples or attributes (데이터 삭제)
	- Usually done when class label is missing (when doing classification)—not effective when the % of missing values per attribute varies considerably
	- 데이터가 너무 많은 경우에 가장 편하다.
- Fill in the missing value manually: tedious + infeasible?
- Fill in it automatically with (데이터 채우기)
	- a global constant : e.g., “unknown”, a new class?!
	- the attribute mean
	- the attribute mean for all samples belonging to the same class: smarter
	- **the most probable value: inference-based such as Bayesian formula or decision tree**

- Imputation Using **(Mean/Median) Values 평균**
	- 👍 Pros :
		- Easy & Fast
		- Works well with small numerical datasets
	- 👎 Cons :
		- 단점은 딱히 없지만 성능이 그렇게 좋진 않다.

![](https://i.imgur.com/dIyna7L.png)

- Imputation Using **(Most Frequent) or (Zero/Constant) Values 가장 빈도가 많은 값 또는 0**
	- 👍 Pros :
		- Works well with categorical features
	- 👎 Cons :
		- 큰 단점은 딱히 없다.

![](https://i.imgur.com/dZ4ezIT.png)

- Imputation Using **Inference-based Method (i.e., k-nn, multi-variate imputations, deep learning)** 데이터의 특징들을 이용하여 예측
	- 👍 Pros :
		- Can be **much more accurate 가장 정확하다** than the mean, median or most frequent imputation methods. (It depends on the dataset)
	- 👎 Cons :
		- **High computational costs with large datasets 모델을 또 하나 만들어야 하는 수고가 요구됨**
		- You have to specify the columns that contain information about the target column that will be imputed

![](https://i.imgur.com/RSbV6Y0.png)

### Noisy Data

- **Noise** : random error or variance in a measured variable
- **Incorrect attribute values** may be due to
	- faulty data collection instruments
	- data entry problems
	- data transmission problems
	- technology limitation
	- inconsistency in naming convention
- **Other data problems** which require data cleaning
	- duplicate records
	- incomplete data
	- inconsistent data
=> **Data Quality**에 관한 문제!!!


#### How to Handle Noisy Data?

- **Binning 구간화**
	- first sort data and partition into (equal-frequency) bins
	- then one can smooth by bin means, smooth by bin median, smooth by bin boundaries, etc. 
- **Regression 회귀**
	- smooth by fitting the data into regression functions 
- **Clustering 군집화**
	- detect and remove outliers  
- **Combined computer and human inspection**
	- detect suspicious values and check by human (e.g., deal with possible outliers)

## Data Integration

- **Data integration 데이터 통합**:  
	- Combines data from multiple sources into a coherent store (여러가지 소스의 데이터를 합치는 것)
- Schema integration 스키마 통합:  
	- Integrate metadata from different sources
- **Entity identification problem**:  
	- Identify real world entities from multiple data sources (각각의 sample들이 여러가지 데이터 소스에서 오기 때문에 이름 같은 것들이 다를 수 있다.)
		- e.g. Bill Clinton = William Clinton
- **Detecting and resolving data value conflicts**
	- For the same real world entity, attribute values from different sources are different
	- Possible reasons: different representations, different scales
		- e.g. metric vs. British units

### Handling Redundancy in Data Integration (데이터 통합에서 중복을 제거하는 방법)

- Redundant data occur often when integration of multiple databases
	- Object identification: The same attribute or object may have different names in different databases
	- Derivable data: One attribute may be a “derived” attribute in another table, e.g., annual revenue
- Redundant attributes may be able to be detected by correlation analysis and covariance analysis
- Careful integration of the data from multiple sources may help reduce/avoid redundancies and inconsistencies and improve mining speed and quality

#### Correlation Analysis (Nominal Data)

- **$$x^2$$ (chi-square) test**
	- **두 개의 변수가 있을 때, 그 변수의 예측값과 실측값들의 분포를 비교해서 얼마나 두 개의 다른 categorical variable이 서로 관련이 있는지를 본다.**
	- The larger the $$x^2$$ value, the more likely the variables are related
	- The cells that contribute the most to the $$x^2$$ value are those whose actual count is very different from the expected count
$$x^2 = \sum{(observed - Expected)^2\over Expected}$$

- [**Correlation does not imply causality. 상관관계는 인과관계를 함축하지 않는다.**](https://ko.wikipedia.org/wiki/%EC%83%81%EA%B4%80%EA%B4%80%EA%B3%84%EC%99%80_%EC%9D%B8%EA%B3%BC%EA%B4%80%EA%B3%84)
	- number of hospitals and number of car-theft in a city are correlated
	- Both are causally linked to the third variable: population

#### Chi-Square Calculation: An Example

|                          | Play chess | Not play chess | Sum (row) |
|:-------------------------|:----------:|:--------------:|:---------:|
| Like science fiction     |    250(90) |       200(360) |       450 |
| Not like science fiction |    50(210) |      1000(840) |      1050 |
| Sum(col.)                |        300 |           1200 |      1500 |  

$$
x^2 = {(250-90)^2 \over 90} + {(50-210)^2\over 210} + {(200-360)^2 \over 360} + {(1000-840)^2 \over 840} = 507.93
$$
- 첫 번째 변수(: 체스를 할 수 있는지 없는지)와 두 번째 변수(: SF를 좋아하는지 좋아하지 않는지)의 상관관계
- $x^2$ (chi-squared) calculation (numbers in parenthesis are expected counts calculated based on the data distribution in the two categories)

![](https://i.imgur.com/Dk0ybzB.png)

- Chi-square는 자유도 k(Degree of freedom)에 따라 달라진다.
- It shows that like_science_fiction and play_chess are correlated in the group
$$
p(x^2>507.93) \approx 0
$$

#### Correlation Analysis (Numeric Data)

- Correlation coefficient (also called **Pearson’s product moment coefficient**)
	- $n$ = the number of tuples (데이터의 개수), 
	- $\bar A$ and $\bar 𝐵$  = the respective means of $A$ and $B$ ($A$와 $B$의 평균)
	- $\sigma_A$ and $\sigma_B$ = the respective standard deviation of $A$ and $B$ ($A$와 $B$의 표준편차)
	- $\sum(a_ib_i)$ = the sum of the $AB$ cross-product.
$$
r_{A,B} = \frac {\sum_{i=1}^{n}(a_i - \bar A)(b_i - \bar B)}{(n-1)\sigma_A\sigma_B} = \frac {\sum_{i=1}^{n}(a_ib_i) - n\bar A\bar B}{(n-1)\sigma_A\sigma_B}
$$
- If $r_{A,B} > 0$, $A$ and $B$ are positively correlated ($A$’s values increase as $B$’s). The higher, the stronger correlation.
- $r_{A,B} = 0$: independent (독립)
- $r_{A,B} < 0$: negatively correlated

#### Visually Evaluating Correlation

![](https://i.imgur.com/1FeVcAm.png)

#### Covariance (Numeric Data)

- Covariance is similar to correlation
$$
Cov(A,B) = E((A - \bar A)(B - \bar B)) = \frac {\sum_{i=1}^{n}(a_i-\bar A)(b_i - \bar B)}{n}
$$
$$
Cov(A,B) = E(A \cdot B) - \bar A \bar B
\qquad r_{A,B} = \frac{Cov(A, B)}{\sigma_A \sigma_B}
$$
- **Positive covariance**: If $Cov(A,B) > 0$, then $A$ and $B$ both tend to be larger than their expected values.
- **Negative covariance**: If $Cov(A,B) < 0$ then if $A$ is larger than its expected value, $B$ is likely to be smaller than its expected value.
- **Independence**: $Cov(A,B = 0)$ but the converse is not true

#### Covariance: An Example

- Suppose two stocks $A$ and $B$ have the following values in one week: (2, 5), (3, 8), (5, 10), (4, 11), (6, 14).
- Question: If the stocks are affected by the same industry trends, will their prices rise or fall together?
	- $E(A)=(2+3+5+4+6)/5=20/5=4$
	- $E(B)=(5+8+10+11+14)/5=48/5=9.6$
	- $Cov(A,B) = (2×5+3×8+5×10+4×11+6×14)/5 − 4 × 9.6 = 4$
- Thus, $A$ and $B$ rise together since $Cov(A, B) > 0$.

## Data Reduction

### Data Reduction 1: Dimensionality Reduction

- **Curse of dimensionality 차원의 저주** 
	- **When dimensionality increases**, data becomes increasingly **sparse (희소해진다)**
	- Density and distance between points, which is critical to clustering, outlier analysis, becomes less meaningful
- **Dimensionality reduction 차원 축소**
	- Avoid the curse of dimensionality
	- Help eliminate irrelevant features and reduce noise
	- Reduce time and space required in data mining
	- Allow easier visualization
- **Dimensionality reduction techniques 차원 축소 기술**
	- Wavelet transforms
	- Principal Component Analysis

#### Attribute Subset Selection

- One way to reduce dimensionality of data 
- **Redundant attributes 중복된 변수 제거**
	- Duplicate much or all of the information contained in one or more other attributes
	- e.g., purchase price of a product and the amount of sales tax paid
- **Irrelevant attributes 상관없는 변수 제거**
	- Contain no information that is useful for the **data mining task** at hand
	- e.g., students' ID is often irrelevant to the task of predicting students' GPA

### Data Reduction 2: Numerosity Reduction

- **Clustering**
	- Partition data set into clusters based on similarity, and store cluster representation (e.g., centroid and diameter) only
	- Can be very effective if data is clustered but not if data is “smeared”
	- Can have hierarchical clustering and be stored in multi-dimensional index tree structures
- **Sampling**
	- Obtaining a small sample s to represent the whole data set $N$
	- Sampling is typically used in data mining when processing the entire set of data of interest is too expensive or time consuming.
	- Key principles for effective sampling are:
		- A sample is representative if it has approximately the same properties (of interest) as the original data set.
		- If a sample is representative, using the sample will work almost as well as using the entire data set.

#### Types of Sampling

- **Simple Random Sampling**
	- There is an equal probability of selecting any particular object
	- **Sampling without replacement 비복원 추출**
		- As each object is selected, it is removed from the population
	- **Sampling with replacement 복원 추출**
		- Objects are not removed from the population as they are selected for the sample
		- The same object can be picked up more than once
	- Simple random sampling may have very poor performance in the presence of skew
- **Stratified Sampling**
	- Split the data into several partitions; then draw a random sample from each partition (데이터를 뽑을 때, 파티션을 나눠서 뽑는 샘플링)

#### Sampling: With or without Replacement

![](https://i.imgur.com/sH6jXbV.png)

#### Sampling: Stratified Sampling

![](https://i.imgur.com/ypVPJli.png)

## Data Transformation

- A function that maps the entire set of values of **a given attribute** to a new set of replacement values s.t. each old value can be identified with one of the new values
- Methods
	- **Smoothing**: Remove noise from data (평균을 적용해서 노이즈를 제거하는 것 - ex.이동평균)
	- Attribute/feature construction
		- New attributes constructed from the given ones
	- Simple function: $xk$, $\log(1+x)$, $e^x$, $\left \vert x \right \vert$, $\dots$ 
		- 금융 데이터의 경우, 수치 그 자체보다 비율이 중요하기 때문에 대부분 $\log$ 를 취한다.
	- **Normalization 정규화**: Scaled to fall within a smaller, specified range (값을 어떤 범위 안에 넣어 주는 것 - 모든 데이터를 일관적인 형태로 관리하기 위함.)
		- min-max normalization
		- z-score normalization
		- normalization by decimal scaling
	- **Discretization**: Concept hierarchy climbing (연속적인 변수를 잘라서 나누는 것)

### Normalization

- **Min-max normalization**: to $newmin_A$, $newmax_A$
	- Ex. Let income range 12,000 to 98,000 normalized to $[0.0, 1.0]$
	- The 73,000 is mapped to $\frac{73600−12000}{98000-12000} (1 − 0) + 0 = 0.716$
$$
v\prime = \frac {v-min_A}{max_A - min_A} (newmax_A - newmin_A)+newmin_A
$$
- $v$ 라는 값에 내가 가진 attribute의 가장 작은 $min$ 값을 빼주고, $max - min$으로 나누어 주면 모든 값이 0~1 사이로 가게 된다. 모든 값들을 가장 큰 값을 1로 두고, 가장 작은 값을 0으로 두고, 그것을 linear하게 꾸미는 형태가 normalization 정규화이다.
- **Z-score normalization** ($\mu$: mean, $\sigma$: standard deviation):
	- Ex. Let $\mu=54,000$ , $\sigma=16,000$ . Then, $\frac{73600-54000}{16000}=1.225$
$$
v\prime = \frac{v - \mu_A}{\sigma_A}
$$
=> ex. 표준점수 (표준편차로 변환해서 내가 평균보다 얼마나 많이 앞서있는지($\sigma$) 알아본다)

### Discretization

- **Three types of attributes**
	- Nominal — values from an unordered set, e.g., color, profession
	- Ordinal — values from an ordered set, e.g., military or academic rank
	- Numeric — real numbers, e.g., integer or real numbers
- **Discretization: Divide the range of a continuous attribute into intervals**  
  (어떠한 기준을 가지고 나누는지가 중요한 관건이 된다)
	- Interval labels can then be used to replace actual data values
	- Reduce data size by discretization
	- **Supervised vs. unsupervised**
	- Split (top-down) vs. merge (bottom-up)
	- Discretization can be performed recursively on an attribute
	- Prepare for further analysis, e.g., classification  
	
	**=> 데이터를 단순화시키고 싶을 때 많이 쓰는 방법**
	
#### Simple Discretization: Binning

- **Equal-width (distance)** partitioning
	- Divides the range into N intervals of equal size: uniform grid
	- if $A$ and $B$ are the lowest and highest values of the attribute, the width of intervals will be: $W = (B –A)/N$
	- The most straightforward, but outliers may dominate presentation
	- Skewed data is not handled well
- **Equal-depth (frequency)** partitioning
	- Divides the range into N intervals, each containing approximately same number of samples
	- Good data scaling
	- Managing categorical attributes can be tricky

#### Binning Methods for Data Smoothing

- Sorted data for price (in dollars): 4, 8, 9, 15, 21, 21, 24, 25, 26, 28, 29, 34 
* Partition into **equal-frequency (equi-depth)** bins:
	* Bin 1: 4, 8, 9, 15  
	- Bin 2: 21, 21, 24, 25
	- Bin 3: 26, 28, 29, 34
* Smoothing by **bin means**:
	- Bin 1: 9, 9, 9, 9  
	- Bin 2: 23, 23, 23, 23  
	- Bin 3: 29, 29, 29, 29
* Smoothing by **bin boundaries**:
	- Bin 1: 4, 4, 4, 15  
	- Bin 2: 21, 21, 25, 25  
	- Bin 3: 26, 26, 26, 34

![](https://i.imgur.com/5IIrmW5.png)

### Binarization (One-hot-encoding)

- Binarization **maps a continuous or categorical attribute into one or more binary attributes**
	- Often convert a continuous attribute to a categorical attribute and then convert the categorical attribute to a set of binary attributes
	- Typically used for association analysis

![](https://i.imgur.com/LQBHFEg.png)

## Data Exploration

### Summary Statistics

- Summary statistics are numbers that summarize properties of the data
	- For **categorical attributes**
		- Frequency, Mode
	- For **continuous attributes**
		- Percentiles
		- Measures of Location: Mean and Median
		- Measures of Spread: Range and Variance

### Histograms

- Used to plot the distribution of the values of **a single attribute** (**한 가지 변수**가 어떻게 분포되어 있는지)
	- It divide the values into bins and shows a bar plot of the number of objects in each bin
	- The height of each bar indicates the number of objects
	- Shape of histogram depends on **the number of bins** (**$x$축을 얼마나 잘게 나눌지 결정하는 요소**인 bin 개수를 결정하는 것이 중요하다.)

![](https://i.imgur.com/ScSloAn.png)

- Two-Dimensional Histograms
	- Used to plot the joint distribution of the values of two attributes

![](https://i.imgur.com/N9aHC77.png)

### Box Plots

- Another way of displaying the **distribution 분포 of data**
- Box plots can be used to **compare 비교 attributes**

![](https://i.imgur.com/oN3sEB7.png)

![](https://i.imgur.com/HOtRLKc.png)

- 50th percentile : 100분위수가 50 (=median 중앙값)
- 25th, 75th percentile : 4분위수

### Scatter Plots

- Used to plot the **pairwise correlations** between attributes (**두 가지 변수**가 어떠한 관계를 가지고 있는지)
	- Each data object is depicted as a marker.
	- The position of a marker is determined by the values of its attributes.
	- While two-dimensional scatter plots are most common, three-dimensional scatter plots can also be utilized.
	- Often, additional attributes can be displayed by using the size, shape, and color of the markers that represent the objects
- Additional attributes can be incorporated into scatter plots through:
	- **Marker Size**: Representing an attribute using the size of markers.
	- **Marker Shape**: Differentiating markers based on an additional attribute.
	- **Marker Color**: Using color variation to convey information about another attribute.

![CvkfoFP.png](https://i.imgur.com/CvkfoFP.png)

- petal_length와 petal_width는 선형적 관계가 있음. (비례하는 형태의 데이터 구성)
- sepal_width와 sepal_length는 관련이 없다.

