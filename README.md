
# Customer Segmentation Analysis Based on Dessert Consumption Behavior

In today's business landscape, conducting business by targeting the most suitable customer for your company is not simple. Because consumers tend to have diverse and different behaviors in each individual. Therefore, it is necessary to study and comprehend consumer behavior across various target groups to thrive in business operations. Then, divide consumers into segments and choose the beachhead. To choose business operations that are consistent with them.




## Objectives
This project has the intention to segment targeted customers based on the consumer behavior of customers. Using R language to analyze the data. Subsequencely, select the most suitable segment for starting and penetrating the market for a dessert product, which is a custard pudding. 
## Project Scope
To study the purchasing and consumption behaviors related to desserts. The survey focused on a sample of female undergraduate students in a university. Ultimately, 244 individuals were surveyed. And after the data cleaning process, 203 respondents were included in the analysis.
## Market Segmentation Analysis

Market segmentation analysis has a flow diagram as follows.

<div style="text-align:center;">
  <img src="https://github.com/dumriketupanya/Customer-Segmentation/raw/main/Pictures/Market%20Segmentation%20Analysis%20Process.jpg" alt="Market Segmentation Analysis Process" />
</div>

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
    boxplot(answer.gen$Amount_p_purchase_baht, horizontal = T, xlab = "Amount per purchase (Uncleaned)") 

#### remove outliers, then create cleaned data table.
    answer.prep <- answer.gen[answer.gen$Amount_p_purchase_baht <= upper.adj,]  # Remove outliers
    summary(answer.prep$Amount_p_purchase_baht)                                 # Summary statistics of the 'Amount_p_purchase_baht' variable
    
    # Create a box plot of cleaned data
    boxplot(answer.prep$Amount_p_purchase_baht, horizontal = T, xlab = "Amount of money per purchase (Cleaned)")

<p align="center">
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

<p align="center">
  <img src="https://github.com/dumriketupanya/Customer-Segmentation/raw/main/Pictures/boxplot%20-%20amount%20of%20money%20per%20purchase.png" width="400" />
  <img src="https://github.com/dumriketupanya/Customer-Segmentation/raw/main/Pictures/histogram%20-%20amount%20of%20money%20per%20purchase.png" width="400" /> 
</p>

####
    # Inspect "Buying Frequency"
    summary(answer.prep$Feq.)                                                      
    histogram(~ Feq., data = answer.prep, type = "density",
    xlab = "frequency", main = " Overall data")
    feq.table <- table(answer.prep$Feq.)
    pie(feq.table, main = " Overall data")                                              # Create pie chart
<p align="center">
  <img src="https://github.com/dumriketupanya/Customer-Segmentation/raw/main/Pictures/histogram%20-%20buying%20frequency.png" width="400" />
  <img src="https://github.com/dumriketupanya/Customer-Segmentation/raw/main/Pictures/pie%20chart%20-%20buying%20frequency.png" width="400" /> 
</p>

####
    # Inspect "Influenced emotion"
    # Create probability with maximum 15 emotional pairs   
    emon.table <- table(answer.prep$Influences_Emotion)                              
    pie(emon.table)

<p align="center">
  <img src="https://github.com/dumriketupanya/Customer-Segmentation/raw/main/Pictures/pie%20chart%20-%20influenced%20emotion.png" width="400" />
</p>




### Statistics function

#### Defined a function to calculate mean, med, mode function 

    get.fun.data <- function(v) {
        uniqv <- unique(v)                                              # Extract unique values
        mode <- uniqv[which.max(tabulate(match(v, uniqv)))]             # Calculate mode
        me <- mean(v)                                                   # Calculate mean
        med <- median(v)                                                # Calculate median
        me <- round(me, digits = 2)                                     # Round mean to two decimal places
    return(cat("Mean = ",me," Median = ",med," Mode = ",mode," "))}     # Return the result

#### Define a function to calculate absolute variation data (range, standard deviation, variance)
    get.abs.variation <- function(v) {
        r <- max(v) - min(v)                    # Calculate range
        sd.ans <- round(sd(v), digits = 2)      # Calculate standard deviation
        v <- round(sd.ans^2, digits = 2)        # Calculate variance
    return(cat("range = ", r, " standard deviation = ", sd.ans, " variance = ", v, " "))}       # Return the result


#### Define a function to calculate relative variation data (coefficient of range, coefficient of variation)
    get.rela.variation <- function(v) {
        co.r <- (max(v) - min(v)) / (max(v) + min(v))       # Calculate coefficient of range
        co.va <- sd(v) / mean(v)                            # Calculate coefficient of variation
        co.r <- round(co.r, digits = 2)                     # Round coefficient of range
        co.va <- round(co.va, digits = 2)                   # Round coefficient of variation
    return(cat("coefficient of range = ", co.r, "coefficient of variation = ", co.va, " "))}    # Return the result

#### Define a function to calculate various statistics and variations
By combining these three functions above, we will obtain a overall function
    
    get.all.data <- function(z) {
        get.fun.data(z)            # Calculate mean, median, and mode
        get.rela.variation(z)      # Calculate relative variation data
        get.abs.variation(z)       # Calculate absolute variation data
    }


#### Define a function to calculate probability distribution
    get.prob.data <- function(column, datalst) {
        tab.col <- table(column)                            # Create a frequency table for the column
        probability <- tab.col / nrow(datalst)              # Calculate probability for each value
        p <- round(probability, digits = 3)                 # Round probabilities to three decimal places
    }

#### Define a function to plot the standard normal curve
    std.nor.curve <- function(col.data, main.name) {
        x <- sort(col.data)                                 # Sort the column data
        z.score <- (x - mean(x)) / sd(x)                    # Calculate z-scores
        y <- dnorm(z.score)                                 # Calculate the density values
        plot(z.score, y, col = "blue", ylab = "density",    # Plot the standard normal curve
        main = main.name, type = "l")
        abline(v = 0, col = "red")                          # Add a vertical line at z = 0
    }

#### Define a function to calculate probability within a range in the standard normal curve
    prob.point.bet <- function(col.data, start.num, fin.num) {
        x <- sort(col.data)                                 # Sort the column data
        z.score <- (x - mean(x)) / sd(x)                    # Calculate z-scores
        z.point.start <- (start.num - mean(x)) / sd(x)      # Calculate z-score for the start number
        z.point.end <- (fin.num - mean(x)) / sd(x)          # Calculate z-score for the end number
        prob.start <- pnorm(z.point.start)                  # Calculate probability for the start number
        prob.end <- pnorm(z.point.end)                      # Calculate probability for the end number
        prob.sum <- prob.end - prob.start                   # Calculate the sum of probabilities within the range
    return(prob.sum)}



### Basic Statistic Properties

#### Find basic statistic properties in amount of the money per purchase ##
    dim(answer.prep.dis)                                    # Dimensions of the dataframe
    amp.pur <- answer.prep.dis$Amount_p_purchase_baht       # Extract the 'Amount_p_purchase_baht' column
    summary(amp.pur)                                        # Calculate summary statistics for the column

#### Find basic descriptive statistic data in amount of the money per purchase ##
    get.fun.data(amp.pur)                                   # Calculate mean, median, and mode
    get.abs.variation(amp.pur)                              # Calculate absolute variation data
    get.rela.variation(amp.pur)                             # Calculate relative variation data

### Probability Analysis and Normal Distribution Curve
#### Find a Probability
By Converting data set to continuous random variable.

    get.prob.data(amp.pur, answer.prep.dis)     # find probability of overall data
    std.nor.curve(amp.pur, "total")             # Create normal distribution curve

#### Find probability with z-value
    pnorm(0)                                    # 0.5 (mean)
    pnorm(1.80357965)                           # left area (from -infinite to point)
    pnorm(1.80357965, lower.tail = FALSE)       # right area (from +infinite to point)

#### Find z-value with probability
    qnorm(0.5)                                  # inverted of pnorm


### Extracting segments 

Due to the conditions regarding the scale of the segmentation variable (interval vs ordinal), it is appropriate to utilize only simple statistics for grouping this data. Frequency is chosen as the criterion, followed by an examination of the amount of money per purchase within each group.

#### Separating customers into different segments

    # Buying frequency: 6 per week or above
    group.feq.6.up <- answer.prep.dis[answer.prep.dis$Feq. == "above_6", ]
    dim(group.feq.6.up)
    group.6.amp <- group.feq.6.up$Amount_p_purchase_baht

    # Buying frequency: 6-5 per week
    group.feq.6.5 <- answer.prep.dis[answer.prep.dis$Feq. == "6_to_5", ]
    dim(group.feq.6.5)
    group.6.5.amp <- group.feq.6.5$Amount_p_purchase_baht

    # Buying frequency: 4-3 per week
    group.feq.4.3 <- answer.prep.dis[answer.prep.dis$Feq. == "4_to_3", ]
    dim(group.feq.4.3)
    group.4.3.amp <- group.feq.4.3$Amount_p_purchase_baht

    # Buying frequency: 2 per week or below
    group.feq.2.down <- answer.prep.dis[answer.prep.dis$Feq. == "below_2", ]
    dim(group.feq.2.down)
    group.2.amp <- group.feq.2.down$Amount_p_purchase_baht

Calling Function to analyzing the data and get the detailed descriptions for each customer buying frequency group.

    summary(amp.pur)
    summary(group.6.amp)
    summary(group.6.5.amp)
    summary(group.4.3.amp)
    summary(group.2.amp)

    get.fun.data(amp.pur)
    get.fun.data(group.6.amp)
    get.fun.data(group.6.5.amp)
    get.fun.data(group.4.3.amp)
    get.fun.data(group.2.amp)

    get.abs.variation(amp.pur)
    get.abs.variation(group.6.amp)
    get.abs.variation(group.6.5.amp)
    get.abs.variation(group.4.3.amp)
    get.abs.variation(group.2.amp)

    get.rela.variation(amp.pur)
    get.rela.variation(group.6.amp)
    get.rela.variation(group.6.5.amp)
    get.rela.variation(group.4.3.amp)
    get.rela.variation(group.2.amp)

    get.prob.data(amp.pur, answer.prep.dis)
    get.prob.data(group.6.amp, group.feq.6.up)
    get.prob.data(group.6.5.amp, group.feq.6.5)
    get.prob.data(group.4.3.amp, group.feq.4.3)
    get.prob.data(group.2.amp, group.feq.2.down)

### Data visualization
Visualize each segment individually for comparison.

#### Standard normal distribution curve
    par(mfrow = c(2,2))
    std.nor.curve(group.2.amp, "below 2 times per week")
    std.nor.curve(group.4.3.amp, "4 to 5 times per week")
    std.nor.curve(group.6.5.amp, "5 to 6 times per week")
    std.nor.curve(group.6.amp, "6 times or more per week")

#### Histogram
    par(mfrow = c(2,2))
    hist(group.feq.2.down$Amount_p_purchase_baht, main = "below 2 times per week", xlab = "amount of money per purchase")
    hist(group.feq.4.3$Amount_p_purchase_baht, main = "4 to 5 times per week", xlab = "amount of money per purchase")
    hist(group.feq.6.5$Amount_p_purchase_baht, main = "5 to 6 times per week", xlab = "amount of money per purchase")
    hist(group.feq.6.up$Amount_p_purchase_baht, main = "6 times or more per week", xlab = "amount of money per purchase")

#### Box plot
    par(mfrow = c(2,2))
    boxplot(group.feq.2.down$Amount_p_purchase_baht, horizontal = TRUE, main = "6 times or more per week")
    boxplot(group.feq.4.3$Amount_p_purchase_baht, horizontal = TRUE, main = "5 to 6 times per week")
    boxplot(group.feq.6.5$Amount_p_purchase_baht, horizontal = TRUE,  main = "4 to 5 times per week")
    boxplot(group.feq.6.up$Amount_p_purchase_baht, horizontal = TRUE , main = "below 2 times per week")

#### Pie chart
    par(mfrow = c(2,2))
    pie(summary(as.factor(group.feq.2.down$Amount_p_purchase_baht)), radius = 1, main = "below 2 times per week")
    pie(summary(as.factor(group.feq.4.3$Amount_p_purchase_baht)), radius = 1, main = "4 to 5 times per week")
    pie(summary(as.factor(group.feq.6.5$Amount_p_purchase_baht)), radius = 1, main = "5 to 6 times per week")
    pie(summary(as.factor(group.feq.6.up

### Describing segments and Strategic Application
These are the example for using descriptor variable to describe each segments.

#### Influenced channels

    # obtained for the most frequent group data.
    data.frame(table(group.feq.6.up$Influence_channel))

The data obtained for the most frequent group (6 times or more per week) in the influenced channel variable is shown below.
|  Source    | Frequency |
|:----------:|:---------:|
|  billboard |     5     |
|   online   |    39     |
| television |     6     |

Shows that if we want to get a customer group that has the most buying frequency. We should mainly focus on using online advertising instead of a billboard or a television. 

#### Influences Emotion

    # obtained for the most frequent group data.
    data.frame(table(group.feq.6.up$Influences_Emotion))

The data obtained for the most frequent group (6 times or more per week) in the pair of Influences Emotion variable is shown below.

|           Emotion Pair           | Frequency |
|:--------------------------------:|:---------:|
|         angry, disgusted         |     24    |
|             angry, happy         |     7     |
|               angry, sad         |     6     |
|         disgusted, happy         |    110    |
|           disgusted, sad         |     22    |
|     disgusted, surprised         |     7     |
|           fearful, angry         |     1     |
|       fearful, disgusted         |     2     |
|           fearful, happy         |     3     |
|         happy, surprised         |     8     |
|              sad, happy          |     12    |
|          sad, surprised          |     1     |

Same as before, We should mainly focus on using advertising or emotional marketing which can alleviate the disgusted emotion or arouse customers to be more happy.
