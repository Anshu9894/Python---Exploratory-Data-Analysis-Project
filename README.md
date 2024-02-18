# Python---Exploratory-Data-Analysis-Project
## High Cancellation Rate Problem Analysis
![exterior](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/99a1a35d-1bbc-4d08-b0fb-b46c33f676fb)

## Problem Statement 
In the recent years, City Hotel and Resort Hotel have seen high cancellation rates. This has resulted in low revenue, sub-optimum resource utilization and ideal capacity. The goal is to identify the factors contributing to this phenomenon, address the bottlenecks and plan resources accordingly.

## Assumptions 
1. No unusual occurrences between 2015 and 2017 that would have a substantial impact on the data used.
2. The information is still current and can be used to analyze a hotel’s plans in an efficient manner.
3. There are no unanticipated negatives to the hotel employing any advised technique.
4. The hotels are not currently using any of the suggested solutions.

## Research Questions
1. What are the variables that affect hotel reservation cancellations?
2. What actions should be taken to monitor these cancellations?
3. What are the pricing and promotional decisions that should be taken?

## Research Questions
1. What are the variables that affect hotel reservation cancellations?
2. What actions should be taken to monitor these cancellations?
3. What are the pricing and promotional decisions that should be taken?

## Hypothesis
1. There is an direct correlation between the price and cancellations.
2. The cancellations and bookings are higher on weekends and holiday seasons.

## Procedure, Analysis and findings

### Importing Libraries
```import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import warnings
warnings.filterwarnings('ignore')
```
### Loading the Dataset
```
df = pd.read_csv('hotel_bookings 2.csv')
```
### Exploratory Data Analysis and Data Cleaning
#### Loading the first five rows to get an overview of the dataset
```
df.head()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/a43bb81c-425e-489b-8bda-df62be782b5c)

#### Loading the last five rows to get an overview of the dataset
```
df.tail()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/014fe38e-2743-4078-bb87-b2c93bef2783)

#### Finding the number of rows and columns in the dataset
```
df.shape
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/418df31b-e175-46a9-bd64-791a3a2e339b)

#### Finding the name of columns of the dataset
```
df.columns
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/215f9148-fd07-415b-9344-7251653741c3)

#### Finding the datatypes of the columns in the dataset
```
df.info()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/a6a139f8-bad3-48cc-8b2f-3bc015fba9c2)

#### Changing the datatype of the reservation_status_date from object to datetime64[ns]
```
df['reservation_status_date'] = pd.to_datetime(df['reservation_status_date'], format='%d/%m/%Y')
df.info()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/cbc0a804-af9b-4660-a422-3d8981235ea7)


#### Finding the statistics summary of the data
```
df.describe(include='object')
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/0e19e04a-d72d-4570-96f9-dbacbf9cc5f5)

#### Finding the unique entries in the columns and dividing the entries by a line
```
for col in df.describe(include = 'object').columns:
    print(col)
    print(df[col].unique())
    print('-'*50)
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/51b9801f-c21c-409b-bc03-3a15e237aeec)

#### Finding the number of null values in the columns
```
df.isnull().sum()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/7c073d87-0ccc-4a1c-a3b4-2a9f03b5e7b0)

#### Removing the two columns with a large number of null values
```
df.drop(['company','agent'], axis = 1, inplace = True)
df.dropna(inplace = True))

df.isnull().sum()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/febf3952-cf1a-46ea-9ec6-d8ab2f306885)

#### Summary of data statistics after removing null columns
```
df.describe()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/0956b1eb-180b-4aec-90f5-13c855b0395b)

#### It was observed from the summary statistics that there was an outlier in ADR and that can be confirmed with a boxplot
```
df['adr'].plot(kind='box')
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/0a2b6168-0bd2-4aeb-b886-865ede8a4243)

#### Defining the dataset as adr < 5000 so as to remove the outlier
```
df = df[df['adr']<5000]
```

## Data Analysis and Visualizations

#### Visualizing the overall cancellations for both the hotels
```
cancelled_per = df['is_canceled'].value_counts(normalize = True)
print(cancelled_per)

plt.figure(figsize = (5,4))
plt.title('Reservation Status Count')
plt.bar(['Not canceled','Canceled'],df['is_canceled'].value_counts(), edgecolor = 'k', width = 0.7)
plt.show()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/25be0d71-baa5-48a3-ad0e-0e02a95e9c13)

#### Visualizing the overall cancellations for both the hotels individually
```
plt.figure(figsize = (8,4))
ax1= sns.countplot(x='hotel', hue = 'is_canceled', data = df, palette = 'Blues')
legend_labels, _ = ax1.get_legend_handles_labels()
ax1.legend(bbox_to_anchor=(1, 1))
plt.title('Reservation Status in Different Hotels', size = 20)
plt.xlabel('Hotel')
plt.ylabel('Number of Reservations')
plt.legend(['Not Canceled', 'Canceled'])
plt.show()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/734ef88f-9c3f-4344-a0d3-1c750c98cc2c)

#### Normalizing the cancellations of the Resort Hotel and the City Hotel
```
resort_hotel = df[df['hotel'] == 'Resort Hotel']
resort_hotel['is_canceled'].value_counts(normalize = True))
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/f56345ff-84d5-4d23-9c69-e346e797b46e)

```
city_hotel = df[df['hotel'] == 'City Hotel']
city_hotel['is_canceled'].value_counts(normalize = True)
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/c7342399-1d8f-4c85-a4e8-6b950bee0c29)

#### Time series analysis to see average rates over the timeline of 3 years
```
resort_hotel = resort_hotel.groupby('reservation_status_date')[['adr']].mean()
city_hotel = city_hotel.groupby('reservation_status_date')[['adr']].mean()

plt.figure(figsize = (20,8))
plt.title('Average Daily Rate in City and Resort Hotel', fontsize = 30)
plt.plot(resort_hotel.index, resort_hotel['adr'], label = 'Resort Hotel')
plt.plot(city_hotel.index, city_hotel['adr'], label = 'City Hotel')
plt.legend(fontsize=20)
plt.show()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/15d784c7-b581-4ab3-b723-846b945b4a59)

#### Monthly Analysis of the Cancellations
```df['month'] = df['reservation_status_date'].dt.month
plt.figure(figsize = (16,8))
ax1 = sns.countplot( x = 'month', hue ='is_canceled', data = df, palette = 'bright' )
legend_labels, _ =ax1.get_legend_handles_labels()
ax1.legend(bbox_to_anchor=(1,1))
plt.title('Reservation status per month', size =20)
plt.xlabel('Month')
plt.ylabel('Number of Reservations')
plt.legend(['not canceled','canceled'])
plt.show
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/cae3efc7-9c9e-4233-a272-0dfca6410742)

#### Monthly Analysis of the ADR
```plt.figure(figsize = (15,8))
plt.title('ADR per month', fontsize = 30)
sns.barplot(x='month', y='adr', data=df[df['is_canceled'] == 1].groupby('month')[['adr']].sum().reset_index())
plt.show()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/28a06a79-a45c-47e3-9b6a-a4ecd52d089e)

#### Country wise Cancellations
```
cancelled_data = df[df['is_canceled'] == 1]
top_10_country = cancelled_data['country'].value_counts()[:10]
plt.figure(figsize= (8,8))
plt.title('Top 10 countries with reservation canceled')
plt.pie(top_10_country, autopct = '%.2f',labels = top_10_country.index)
plt.show()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/7cd291bc-d55e-4f90-a5c9-d4ec830879cb)

#### Reservations from different sources and normalization of the values 
```
df['market_segment'].value_counts()
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/6a509a20-cd20-4967-abb9-4b63259cbf27)

```
df['market_segment'].value_counts(normalize = True)
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/43d01e6b-5873-44a5-ba5e-62df05eaee94)

```
cancelled_data['market_segment'].value_counts(normalize = True)
```
![image](https://github.com/Anshu9894/Python---Exploratory-Data-Analysis-Project/assets/102878435/6f3444e5-2fcc-4771-80ba-0f2f12f0e0fc)

## Conclusion and Suggestions
