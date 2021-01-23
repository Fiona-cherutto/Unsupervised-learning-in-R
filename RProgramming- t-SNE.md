
# t-Distributed Stochastic Neighbor Embedding (t-SNE)


 
```{r}
# Loading our dataset

# train <- read.csv('http://bit.ly/CarreFourDataset')  


library("data.table") 
train <- fread('http://bit.ly/CarreFourDataset') 

```


```{r}
# Previewing our dataset

head(train)
View(train)

```


```{r}
# Installing Rtnse package

install.packages("Rtsne")

```


```{r}
# Loading our tnse library

library(Rtsne)
```


```{r}
### Changing time stamp to date time series
### specify format m/d/Y 
### convert the characters of date and time to  date time series
train$Date<- as.Date(train$Date, format="%m/%d/%Y",tz=Sys.timezone())
```

```{r}

### Changing time stamp to date time series
### specify format H:M:S
### convert the characters of date and time to  date time series
train$Time <- as.POSIXct(train$Time, format="%H:%M",tz=Sys.timezone())
```


```{r}
head(train)
```


```{r}
# Installing dplyr package
install.packages("dplyr")
install.packages("stringr")
```


```{r}
# Loading our tnse library
library(dplyr)
library(stringr)
```


```{r}
# Changing column names to lower and replacing spaces with an underscore for readability and easy reference
colnames(train) = tolower(str_replace_all(colnames(train), c(' ' = '_')))

# Checking whether the column names have been renames appropriately
print(colnames(train))
```



```{r}
# Curating the database for analysis 

customer_type <-train$customer_type
train$customer_type<-as.factor(train$customer_type)

```


```{r}
# For plotting

colors = rainbow(length(unique(train$customer_type)))
names(colors) = unique(train$customer_type)
names(colors)
```



```{r}
# Executing the algorithm on curated data

tsne <- Rtsne(train[,-1], dims = 2, perplexity=30, verbose=TRUE, max_iter = 500)

```


```{r}
# Getting the duration of execution

exeTimeTsne <- system.time(Rtsne(train[,-1], dims = 2, perplexity=30, verbose=TRUE, max_iter = 500))

```



```{r}
# Plotting our graph and closely examining the graph

plot(tsne$Y, t='n', main="tsne")
text(tsne$Y, labels=train$customer_type, col=colors[train$customer_type])


```
For this we checked for type of customer either normal or member and we can see that both are distributed equally.
Explore relations between features in a dataset with many features shows presence of relation.
