# Market-Basket-Analysis
Market basket analysis with Apriori algorithm

The retailer wants to target customers with suggestions on itemset that a customer is most likely to purchase .I was given dataset contains data of a retailer; the transaction data provides data around all the transactions that have happened over a period of time. Retailer will use result to grove in his industry and provide for customer suggestions on itemset, we be able increase customer engagement and improve customer experience and identify customer behavior. I will solve this problem with use Association Rules type of unsupervised learning technique that checks for the dependency of one data item on another data  item.

 
### Introduction

Association Rule is most used when you are planning to build association in different objects in a set. It works when you are planning to find frequent patterns in a transaction database. It can tell you what items do customers frequently buy together and it allows retailer to identify relationships between the items. 

### An Example of Association Rules

Assume there are 100 customers, 10 of them bought Computer Mouth, 9 bought Mat for Mouse and 8 bought both of them.
- bought Computer Mouth => bought  Mat for Mouse
- support = P(Mouth & Mat) = 8/100 = 0.08
- confidence = support/P(Mat for Mouse) = 0.08/0.09 = 0.89
- lift = confidence/P(Computer Mouth) = 0.89/0.10 = 8.9
This just simple example. In practice, a rule needs the support of several hundred transactions, before it can be considered statistically significant, and datasets often contain thousands or millions of transactions.

### Strategy

- Data Import
- Data Understanding and Exploration
- Transformation of the data – so that is ready to be consumed by the association rules algorithm
- Running association rules
- Exploring the rules generated
- Filtering the generated rules
- Visualization of Rule 


### Dataset Description

- File name: Assignment-1_Data
- List name: retaildata
- File format: . xlsx
- Number of Row: 522065
- Number of Attributes: 7

	- BillNo: 6-digit number assigned to each transaction. Nominal.
	- Itemname: Product name. Nominal.
	- Quantity: The quantities of each product per transaction. Numeric.
	- Date: The day and time when each transaction was generated. Numeric.
	- Price: Product price. Numeric.
	- CustomerID: 5-digit number assigned to each customer. Nominal.
	- Country: Name of the country where each customer resides. Nominal.

<img width="492" alt="image" src="https://user-images.githubusercontent.com/91852182/145270162-fc53e5a3-4ad1-4d06-b0e0-228aabcf6b70.png">

### Libraries in R

First, we need to  load required libraries. Shortly I describe all libraries.

- arules - Provides the infrastructure for representing, 
manipulating and analyzing transaction data and patterns (frequent itemsets and association rules).
- arulesViz - Extends package 'arules' with various visualization.
techniques for association rules and item-sets. The package also includes several interactive visualizations for rule exploration.
- tidyverse - The tidyverse is an opinionated collection of  R packages designed for data science.
- readxl - Read Excel Files in R.
- plyr - Tools for Splitting, Applying and Combining Data.
- ggplot2 - A system for 'declaratively' creating graphics, based on "The Grammar of Graphics". You provide the data, tell 'ggplot2' how to map variables to aesthetics, what graphical primitives to use, and it takes care of the details.
- knitr - Dynamic Report generation in R.
- magrittr- Provides a mechanism for chaining commands with a new forward-pipe operator, %>%. This operator will forward a value, or the result of an expression, into the next function call/expression. There is flexible support for the type of right-hand side expressions.
- dplyr - A fast, consistent tool for working with data frame like objects, both in memory and out of memory.
- tidyverse - This package is designed to make it easy to install and load multiple 'tidyverse' packages in a single step.

<img width="481" alt="image" src="https://user-images.githubusercontent.com/91852182/145270210-49c8e1aa-9753-431b-a8d5-99601bc76cb5.png">

### Data Pre-processing

Next, we need to upload Assignment-1_Data. xlsx to R to read the dataset.Now we can see our data in R. 

<img width="255" alt="image" src="https://user-images.githubusercontent.com/91852182/145270229-514f0983-3bbb-4cd3-be64-980e92656a02.png">
<img width="476" alt="image" src="https://user-images.githubusercontent.com/91852182/145270251-6f6f6472-8817-435c-a995-9bc4bfef10d1.png">
 
After we will clear our data frame, will remove missing values.

<img width="285" alt="image" src="https://user-images.githubusercontent.com/91852182/145270286-05854e1a-2b6c-490e-ab30-9e99e731eacb.png">

To apply Association Rule mining, we need to convert dataframe into transaction data to make all items that are bought together in one invoice will be in one row. Below lines of code will combine all products from one BillNo and Date and combine all products from that BillNo and Date as one row, with each item, separated by (,)

<img width="311" alt="image" src="https://user-images.githubusercontent.com/91852182/145270333-b7848fce-53d9-4486-b2ae-c331f13b4275.png">

We don’t need BillNo and Date, we will make it as Null.
Next, you have to store this transaction data into .csv 

<img width="311" alt="image" src="https://user-images.githubusercontent.com/91852182/145270368-787a1721-8d93-4f69-8369-da3a70082e95.png">


This how should look transaction data before we will go to next step.

<img width="311" alt="image" src="https://user-images.githubusercontent.com/91852182/145270884-ca3d6d47-708f-4ab6-bc1d-0e17b912e1f3.png">

<img width="482" alt="image" src="https://user-images.githubusercontent.com/91852182/145270942-14bfed99-473e-444c-9c14-a215c2957bee.png">


At this step we already have our transaction dataset, and it shows the matrix of items which bought together. We can’t see here any rules and how often it was purchase together. Now let’s check how many transactions we have and what they are. We will have to have to load this transaction data into an object of the transaction class. This is done by using the R function read.transactions of the arules package. Our format of Data frame is basket. 

<img width="482" alt="image" src="https://user-images.githubusercontent.com/91852182/145270981-84b76556-380b-4a32-a2ee-465d192141e8.png">
 
Let’s have a view our transaction object by summary(transaction)

<img width="160" alt="image" src="https://user-images.githubusercontent.com/91852182/145271000-33fe3da8-6517-4d7a-a844-017022047ff6.png">

We can see 18193 transactions (rows) and 7698 items (columns). 7698 is the product descriptions and 18193 transactions are collections of these items.

<img width="498" alt="image" src="https://user-images.githubusercontent.com/91852182/145271025-9acd295d-6cef-44eb-8e5b-8ff12ddd798f.png">

The summary gives us some useful information:
- Density tells the percentage of non-zero cells in a sparse matrix. In other words, total number of items that are purchased divided by a possible number of items in that matrix. You can calculate how many items were purchased by using density: 18193x7698x0.002291294=337445
- Summary will show us most frequent items.
- Element (itemset/transaction) length distribution: It  will gave us how many transactions are there for 1-itemset, 2-itemset and so on. The first row is telling you a number of items and the second row is telling you the number of transactions.
For example, there is only 1546 transaction for one item, 860  transactions for 2 items, and there are 419 items in one transaction which is the longest.

Let’s check item frequency plot, we will generate an itemFrequencyPlot to create an item Frequency Bar Plot to view the distribution of objects based on itemMatrix (e.g., >transactions or items in >itemsets and >rules) which is our case.

<img width="267" alt="image" src="https://user-images.githubusercontent.com/91852182/145271096-4279c764-0d0c-41dc-87fc-0b13b217f566.png">

<img width="267" alt="image" src="https://user-images.githubusercontent.com/91852182/145271132-f3bf1374-06b8-4f71-b810-dbb098f6963c.png">

<img width="490" alt="image" src="https://user-images.githubusercontent.com/91852182/145271187-3f6023db-3820-4083-a6e9-cd74020acc70.png">

In itemFrequencyPlot(transaction,topN=20,type="absolute") first argument - our transaction object to be plotted that is tr. topN is allows us to plot top N highest frequency items. type can be as  type="absolute" or type="relative". If we will chouse absolute it will plot numeric frequencies of each item independently. If relative it will plot how many times these items have appeared as compared to others. As well I made it in colure for better visualization.

### Generating Rules
 
Next, we will generate rules using the Apriori algorithm. The function apriori() is from package arules. The algorithm employs level-wise search for frequent itemsets. Algorithm will generate frequent itemsets and association rules. We pass supp=0.001 and conf=0.8 to return all the rules that have a support of at least 0.1% and confidence of at least 80%. We sort the rules by decreasing confidence and will check summary of the rules.

<img width="462" alt="image" src="https://user-images.githubusercontent.com/91852182/145271236-3a82733a-216f-4db2-9f60-9b43c2016ea4.png">
 
The apriori will take (transaction) as the transaction object on which mining is to be applied. parameter will allow you to set min_sup and min_confidence. The default values for parameter are minimum support of 0.1, the minimum confidence of 0.8, maximum of 10 items (maxlen).

<img width="462" alt="image" src="https://user-images.githubusercontent.com/91852182/145271269-4565e292-212a-47cb-8bc1-5bfe7f1819a8.png">

Summary of rules give us clear information as:
- Number of rules: 97267
- The distribution of rules by length: a length of 6 items has the most 33296 and length of 2 items has lowest number of rules 111
- The summary of quality measures: ranges of support, confidence, and lift.
- The information on data mining: total data mined, and the minimum parameters we set earlier

Now, 97267 it a lot of rules. We will identify only top 10.

<img width="273" alt="image" src="https://user-images.githubusercontent.com/91852182/145271351-6977af6b-2d7a-4a81-a3c1-f160722b2897.png">

<img width="478" alt="image" src="https://user-images.githubusercontent.com/91852182/145271400-b74ec578-5bf5-47fd-a564-407eacfb6b43.png">


Using the above output, you can make analysis such as:
- 100% of the customers who bought 'ART LIGHTS ' also bought 'FUNK MONKEY'.
- 100% of the customers who bought 'BILLBOARD FONTS DESIGN ' also bought 'WRAP'.
We can limit the size and number of rules generated. we can set parameter in Apriori. If we want stronger rules, we must to increase the value of conf. and for more extended rules give higher value to maxlen. 

### Visualizing Association Rules

We have thousands of rules generated based on data, we will need a couple of ways to present our findings. We will use ItemFrequencyPlot to visualize association rules.

#### Scatter-Plot:

<img width="354" alt="image" src="https://user-images.githubusercontent.com/91852182/145271541-b9e4eb67-68c4-499b-b5c8-37f27bc27347.png">

A straight-forward visualization of association rules is to use a scatter plot using plot() of the arulesViz package. It uses Support and Confidence on the axes. In addition, third measure Liftis used by default to color (grey levels) of the points.

<img width="228" alt="image" src="https://user-images.githubusercontent.com/91852182/145271713-e995b95e-381a-4e00-83b9-d197b5e4a7a5.png">
<img width="201" alt="image" src="https://user-images.githubusercontent.com/91852182/145271728-e172716c-a3c7-4485-8c00-b5da2435f8b2.png">



#### Interactive Scatter-Plot:

We can have a look for each rule (interactively) and view all quality measures (support, confidence and lift). 

<img width="197" alt="image" src="https://user-images.githubusercontent.com/91852182/145271752-d5d0406e-a42d-4cb9-9c34-db14e51556e9.png">

<img width="223" alt="image" src="https://user-images.githubusercontent.com/91852182/145271774-70479173-6b0e-45bd-b403-b99af9747e31.png">

<img width="224" alt="image" src="https://user-images.githubusercontent.com/91852182/145271779-fa20c370-7e18-4a1f-b70e-47c03aa42033.png">

#### Graph - Based Visualization and Group Method:

Graph plots are a great way to visualize rules but tend to become congested as the number of rules increases. So, it is better to visualize a smaller number of rules with graph-based visualizations.	We can see as well group method for top 10 items.

<img width="215" alt="image" src="https://user-images.githubusercontent.com/91852182/145271807-c6328bf7-2b46-45cf-acec-2f4b858af85b.png">
<img width="217" alt="image" src="https://user-images.githubusercontent.com/91852182/145271812-d9ce223b-337e-44c6-bb8c-c75c77d4ee64.png">

   

  
### Conclusion
Based on the results of these calculations can be used as a recommendation for retail owners to arrange the arrangement of product catalogs and take strategic steps to improve product marketing.. By utilizing the association rules which are discovered as a result of the analyses, the retailer can apply effective marketing and sales promotion strategies, he will be able increase customer engagement and improve customer experience and identify customer behavior.

### Attached files 

- Assigment Part1 Retailer.R
- Assignment-1_Data.xlsx
