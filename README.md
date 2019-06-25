**store1**

This is for showing partial consuming behaviours

# import data
```
df<-read.csv("~/desktop/store1.csv)
```
load data manipulation packages
```
library(plyr)
library(dplyr)
```
read data structure
```
head(df,6)
str(head)
```
![](https://github.com/alansiu1929/store1/blob/master/head6.png)
![](https://github.com/alansiu1929/store1/blob/master/str.png)

# A. select dataframe we want
```
df2<-ddply(df,.(customer_id),summarise,gender=mean(gender), most_recent_order_date=max(as.Date(date)))
df3<-count(df,customer_id,name="order_count")
res<-inner_join(df2,df3,by="customer_id",type="left")
```
show 10 new dataframe results
```
head(res,10)
```
![](https://github.com/alansiu1929/store1/blob/master/dateframe.png)

# B. plot graph

load data visualisation package
```
library(ggplot2)
```

create new variable "week", we assume 2017-01-01 is the first day of the year
```
library(lubridate)
df4<-mutate(df,week=week(date))
```

plot graph for order counts per week
```
df5<-count(df4,week)
ggplot(df5,aes(week,n))+geom_line()+ylab("count")+ggtitle("count of orders per week")
```
![](https://github.com/alansiu1929/store1/blob/master/Rplot01.png)

with smooth effect
```
ggplot(df5,aes(week,n))+geom_line()+geom_smooth()+ylab("count")+ggtitle("count of orders per week")
```
![](https://github.com/alansiu1929/store1/blob/master/Rplot02.png)

# C. find mean value between genders
```
ddply(df,.(gender),summarise,mean(value))
```
![](https://github.com/alansiu1929/store1/blob/master/mean%20value.png)

# significance inference
create a data frame for testing
```
df6<-df%>%
filter(gender==0)%>%
select(value)
df7<-df%>%
filter(gender==1)%>%
select(value)
```
perform T-testing to prove signifance (using two-sided with CI 95%)
```
t.test(df6,df7)
```
![](https://github.com/alansiu1929/store1/blob/master/t-test.png)

p-value=0.04816<0.05
null hypothese is rejected and there is a difference of mean order value between genders

# D. confusion matrix
load package for confusion matrix
```
library(caret)
library(e1071)
```
change gender and predicted gender as factors
```
df$gender=as.factor(df$gender)
df$predicted_gender=as.factor(df$predicted_gender)
```
perform confusion matrix
```
confusionMatrix(df$predicted_gender,df$gender)
```
![](https://github.com/alansiu1929/store1/blob/master/confusion%20matrix.png)

The accuracy for the prediction is 0.6383. The quality of prediction is not strong enough.

# E. My favorite tool or technique
R studio is my favorite data analysis and data visualization tool because for plotting the trend over the time, ggplot can easily give people ideas with the flow of trend by adding geom_smooth() function after the plot, I have shown the example in the section B after the first plot.
