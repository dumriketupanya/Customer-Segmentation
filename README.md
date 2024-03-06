
# Customer Segmentation Analysis Based on Dessert Consumption Behavior

In today's business landscape, conducting business by targeting the most suitable customer for your company is not simple. Because consumers tend to have diverse and different behaviors in each individual. Therefore, it is necessary to study and comprehend consumer behavior across various target groups to thrive in business operations. Then, divide consumers into segments and choose the beachhead. To choose business operations that are consistent with them.




## Objectives
This project has the intention to segment targeted customers based on the consumer behavior of customers. Using R language to analyze the data. Subsequencely, select the most suitable segment for starting and penetrating the market for a dessert product, which is a custard pudding. 
## Project Scope
To study the purchasing and consumption behaviors related to desserts. The survey focused on a sample of female undergraduate students in a university. Ultimately, 244 individuals were surveyed. And after the data cleaning process, 203 respondents were included in the analysis.
## Market Segmentation Analysis

Market segmentation analysis has a flow diagram as follows.

![Market Segmentation Analysis Process](https://github.com/dumriketupanya/Customer-Segmentation/raw/main/Pictures/Market%20Segmentation%20Analysis%20Process.jpg)

We chose the commonsense segmentation method because it required less capital, faster, and less complexity.

We will mainly focus on the details of how to identify the ideal target audience and how to select variables for the data collection process.

### Specifying the ideal target segment
1. **Knock-out criteria** are considered a "Must have" characteristic and those elements need to show up in the selected customer groups. An important criteria are as follows:
####
| Criteria | Description     | 
| :-------- | :------- | 
| homogeneous | Share similar characteristics within the group. | 
| distinct | Group must be clearly different from other groups. | 
| Large enough | Groups are have enough members | 
| Matching | Selected group is consistent with the organization's competitive strengths. | 
| Identifiable | Customer does really exist. | 
| Reachable | Can reach customers. | 

2. **Attractiveness criteria** considered a "Should have" characteristic and used to measure the attractiveness of each group. Scores will be given based on how interesting the group is.

### Variables for the data collection process
This step requires defining the variables for use in the data collection process. There are two types of variables that must be created as follows:

1. **Segmentation variables** are used to divide different groups in the sample data from each other.
2. **Descriptor variables** are used to describe the characteristics of the sample and are not used for grouping.
####
Those criteria can be divided into various topics as follows:
1. Geographic segmentation
2. Socio-demographic segmentation
3. Psychographic segmentation
4. Behavioral segmentation



## Questionnaire 
The questionnaire has a component as a follows.

| Name                           | Type of Item           | Type of data  | Measurement scale | Range     |
|--------------------------------|------------------------|----------------|-------------------|-----------|
| Area                           | Fixed (Bangkok)              | Qualitative    | Nominal scale     | N/A       |
| Gender                         | Fixed (Female)              | Qualitative    | Nominal scale     | N/A       |
| Occupation                     | Fixed (Undergraduates)              | Qualitative    | Nominal scale     | N/A       |
| Age                            | Fixed (18-24)    | Quantitative   | Interval scale    | N/A     |
| Purchase frequency             | Segmentation variable | Quantitative   | Ordinal scale     | Infinity |
| Amount of money per purchase  | Segmentation variable | Quantitative   | Interval scale    | Infinity |
| Emotional engagement           | Descriptor variable    | Qualitative    | Nominal scale     | Multiple  |
| The utilization function      | Descriptor variable    | Qualitative    | Ordinal scale     | Multiple  |
| Selective attention           | Descriptor variable    | Qualitative    | Nominal scale     | Multiple  |
| Consumer behavior related with route | Descriptor variable | Qualitative    | Ordinal scale     | Multiple  |
| Brand awareness                | Descriptor variable    | Qualitative    | Ordinal scale     | Multiple  |
| Dogmatism                      | Descriptor variable    | Qualitative    | Nominal scale     | Binary    |
| Ethnocentrism                  | Descriptor variable    | Qualitative    | Nominal scale     | Binary    |
| Social character               | Descriptor variable    | Qualitative    | Nominal scale     | Binary    |
| Channel                        | Descriptor variable    | Qualitative    | Nominal scale     | Multiple  |
| Reference group                | Descriptor variable    | Qualitative    | Nominal scale     | Multiple  |
| The value-expression function | Descriptor variable    | Qualitative    | Nominal scale     | Binary    |
| Prior experience              | Descriptor variable    | Qualitative    | Nominal scale     | Binary    |


## Data Analysis with R

#### import data

    answer <- read.csv('fileName.csv', stringsAsFactors = TRUE)

###  Data Cleaning
#### inspect data and data table

    dim(answer)         # check the dimenstion of data table
    colnames(answer)    # check all column name
    summary(answer)     # skimming through overall details of each data

#### filter data

    gender.choose <- answer$Gender == "female"      # include "female" only
    answer.gen <- answer[gender.choose,]            # stores the result in a new data frame called 'answer.gen'

#### detect outliers in "Amount per purchase (Baht)"
    library("lattice")                                  # Load the lattice library for plotting graph      
    summary(answer.gen$Amount_p_purchase_baht)          # Summary statistics of the 'Amount per purchase (Baht)' variable
    qua <- quantile(answer.gen$Amount_p_purchase_baht)  # Calculate quantiles
    qua.3 <- qua[4]                                     # Find the 3rd quantile
    
    # Calculate the upper adjacent value for outlier detection
    # Upper adjacent value = 3rd quantile + 1.5 * interquartile range
    upper.adj <- IQR(answer.gen$Amount_p_purchase_baht) + qua.3

    # Create a box plot of overall data
    boxplot(answer.gen$Amount_p_purchase_baht, horizontal = T, xlab = "Amount per purchase") 

#### remove outliers, then create cleaned data table.
    answer.prep <- answer.gen[answer.gen$Amount_p_purchase_baht <= upper.adj,]  # Remove outliers
    summary(answer.prep$Amount_p_purchase_baht)                                 # Summary statistics of the 'Amount_p_purchase_baht' variable
    
    # Create a box plot of cleaned data
    boxplot(answer.prep$Amount_p_purchase_baht, horizontal = T, xlab = "Amount of money per purchase")

<p float="left">
  <img src="https://github.com/dumriketupanya/Customer-Segmentation/raw/main/Pictures/box%20plot%20of%20uncleaned%20data.png" width="400" />
  <img src="https://github.com/dumriketupanya/Customer-Segmentation/raw/main/Pictures/box%20plot%20of%20cleaned%20data.png" width="400" /> 
</p>


###  Descriptive analysis
#### Inspect segmentation variable and overall data 

    # Inspect "Amount per purchase"
    summary(answer.prep$Amount_p_purchase_baht)                                         # Summary statistics
    histogram(~ Amount_p_purchase_baht, data = answer.prep, type = "density",           # Create a histogram
    main = "Overall data", xlab = "Amount of money per purchase")                       
    boxplot(answer.prep$Amount_p_purchase_baht, horizontal = TRUE,                      # Create a boxplot
    main = "Overall data", xlab = "Amount of money per purchase")                       

//PIC
####
    # Inspect "Buying Frequency"
    summary(answer.prep$Feq.)                                                      
    histogram(~ Feq., data = answer.prep, type = "density",
    xlab = "frequency", main = " Overall data")
    feq.table <- table(answer.prep$Feq.)
    pie(feq.table, main = " Overall data")                                              # Create pie chart
//PIC
####
    # Inspect "Influenced emotion"
    # Create probability with maximum 15 emotional pairs   
    emon.table <- table(answer.prep$Influences_Emotion)                              
    pie(emon.table)
//PIC    


