**store1**

This is for showing partial consume behaviours

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
# select dataframe we want
```
df2<-ddply(df,.(customer_id),summarise,gender=mean(gender), most_recent_order_date=max(as.Date(date)))
df3<-count(df,customer_id,name="order_count")
res<-inner_join(df2,df3,by="customer_id",type="left")
```
show 10 new dataframe results
```
head(res,10)
```

# plot graph

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
with smooth effect
```
ggplot(df5,aes(week,n))+geom_line()+geom_smooth()+ylab("count")+ggtitle("count of orders per week")
```
# find mean value between genders
```
ddply(df,.(gender),summarise,mean(value))
```

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
p-value=0.04816<0.05
null hypothese is rejected and there is a difference between mean order value between genders

# confusion matrix
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
The accuracy for the prediction is 0.6383. The quality of prediction is not strong enough.
