# Data-Science-Practicum-I
This project is based on my current job role as a Quality Assurance Tech in crop testing. Each test we conduct has a start date and an end date. Ideally, the sample should only have a day average of seven before we send the results to the customer. I set up this project to replicate the way a data analyst would take the number of results that stay within the seven-day average and determine if our department was staying within the quota or whether there might be issues that need to be addressed. These issues could be whether overtime should be allowed, whether we need to hire more techs or pull techs from other departments to help with the workflow, or if there is a problem with the testing process that needs to be investigated. 
 
## Project Overview
This project works in three sections. First, I collect the data and obtain the export file, which had just over 100 samples listed. While I could use this data for private projects, I do not have permission to release it publicly. To compensate for this, I created a secondary output file with similar data, and ran the algorithm through the secondary output as well. Additionally, I created a third file that had 110 samples. This third file had some samples with where start or end dates were purposely left out to mimic either a program or human error. This third file was also run through the algorithm. 
Next, I clean and organize the data so that it will be more efficient and visually appealing to graph. This starts with calculating the difference between the start and end dates, phasing out the data of the start and end dates and leaving only the difference, and then organizing the data calculated from the difference to be able to be visualized through a graphical format.
Finally, I graph the data into a format that is legible to both people who have experience in data analytics and those without experience. After some experimentation I determined that a bar chart would best show the comparison between the samples that pass the seven day quota versus the samples that run over eight days and fail. 

## Data
#### Source: 
The original data is property of the company that produced it, and as a result can be publicly displayed but not re-distributed. Data of a similar year and month will be re-created via Microsoft Excel for the purpose of being publicly distributed and tested. It is that data that will be further discussed. A third output file of data will also be displayed. This third file has missing or corrupted data in order to ensure the algorithm works with non-perfect data. 
#### Scope: 
The data consists of just over 100 tests ran between August 1, 2019 and August 31, 2019. Only test dates that had both a start date between August 1 and August 31 are used. The third data files are also run through the analysis with missing start and end dates to showcase events of possible human error. The third data file also ran from August 1, 2019 to August 31, 2019, with 110 test samples in the file. 

## Process
This process was completed on a Windows 10 system, with Jupyter Notebook and Python 3.9.0.
#### 1.	Gather the data. 
The original data was entered into a macro through Microsoft Excel, the output file being an xlsx. 
The data fitting the criteria is saved into an output file that should then be converted into a csv or other readable format for Python. The original data file was converted from an xlsx to a csv using the Microsoft Excel program.
Once the data is compiled, it can be imported into either Python, Python Shell, or Jupyter Notebook. The data file included was written in Jupyter Notebook. 
I have included a scraper that can be used to scrape data from social media sites like Twitter to gather data for analysis if an output is not readily available. 
Loading the data into Jupyter Notebook can be done with 

```
df=pd.read_csv(‘C:/Users/Student/FileFolder/data.csv’, parse_dates=[0,1])
```

#### 2.	Clean and organize the data.  
Formatting the two columns of data into three columns with the start date and end date remaining, in addition to the diff column which now shows the calculated difference between the start date and end date. 

```
df[‘diff’]=(df[‘End date’] – df[‘Start date’]).dt.days
```

Drops all blank or N/A data boxes.

```
df.dropna(inplace=True)
```

Places the cleaned data into organized columns labeled as diff, under, and over. This also removes the start and end date headers and data. The data is renamed from df to category. 

```
category=df.groupby(‘diff’)\ 
   .agg(under=pd.NamedAgg(column='diff', 
    aggfunc=lambda x: (x < 8).sum()), 
    over=pd.NamedAgg(column='diff',  
    aggfunc=lambda x: (x > 7).sum()))\ .reset_index()
  ```
  
Remove the diff label from the category dataset and display the result. 

```
category_p=category[[‘under’,’over’]]
category_p.head()
 ```
    
The column heads are changed from under and over to pass and fail.

```
category_p=columns=[‘pass’,’fail’]
```

The categories of pass and fail both have their sums calculated and then displayed.

```
category_pf=category_p.sum()
category_pf.head()
```
#### 3.	Visualizing the results. 
Plots the results of the pass and fail sums into a bar graph. 

```
my_plot = category_pf.plot(kind='bar',title="Pass/Fail results of seven-day-averages in Department") my_plot.set_xlabel("Pass or Fail Results") my_plot.set_ylabel("Amount")
```


