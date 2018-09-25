# GA_customer-revenue-prediction

# Loading of packages -----------------------------------------------------
packages <- c("dplyr",
              "magrittr",
              "data.table",
              "tidytext",
              "ggplot2",
              "lubridate",
              "reshape2",
              "knitr", 
              "wordcloud",
              "RColorBrewer",
              "stingr")
lapply(packages, require, character.only = TRUE)

install.packages("jsonlite")
library(jsonlite)

# loading of data ---------------------------------------------------------
train <- read.csv("~/kaggle/train.csv")

#convert to strings first
train[c("device", "geoNetwork","totals","trafficSource")] <- lapply(train[c("device", "geoNetwork","totals","trafficSource")], as.character)
jsoncol <- c("device", "geoNetwork","totals","trafficSource")

for (i in 1:length(jsoncol))
{
  print(jsoncol[i])
}

#convert to normal text columns
new.df <- train  %>%
  rowwise() %>%
  do(data.frame(fromJSON(.$totals, flatten = T))) %>%
  ungroup() 
