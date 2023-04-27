---
title: "Combining Spreadsheets with Pandas"
date: 2020-12-22T05:15:47-05:00
draft: false
image: pandaspython.jpeg
tags: [
	"python",
	"development",
	"pandas",
	"excel",
	"howto"
]
categories: ["development",
				"howto",
				"python",
				"excel",
				"howto"
				]

---

# <center>Combining Spreadsheets With Pandas</center>

I was tasked with combining a bunch of spreadsheets that are automatically generated everynight.  The task is super easy with Pandas and Python.  So I took those 18 spreadsheets and made each one a different tab in one spreadsheet.  WHAM BAM I thought I was done and then I found out the requestor wanted each spreadsheet on the same tab just shoved together one after another.  So after some research I figured out Pandas can do this too!

First we have to import the libraries we need for this script.

```python
import pandas as pd
from pathlib import Path
import datetime
```

Next we need to set the dates pieces we need to pull the file names together since the files are all named SomethingSpecificDayMonthYear.xls.

```python
today = datetime.date.today()
yesterday = today - datetime.timedelta(days=1)
todayday = today.strftime("%d")
todayyear = today.strftime("%Y")
todaymonth = today.strftime("%m")
year = yesterday.strftime("%Y")
day = yesterday.strftime("%d")
month = yesterday.strftime("%m")
```

Onto the next part, you need to use the pathlib we imported to build where you have all the files saved you need to combine like below.  This will allow us to use a variable as the file path for future use.

```python
testxls1 = Path(r"\\testserver\testlocation\test1\D"+year+month+day+r".xls")
testxls2 = Path(r"\\testserver\testlocation\test2\D"+year+month+day+r".xls")
testxls3 = Path(r"\\testserver\testlocation\test3\D"+year+month+day+r".xls")
```

Next we will take the file names we setup earlier and read them into dataframes in Pandas so we can manipulate them easily.

```python
df1 = pd.read_excel(textxls1)
df2 = pd.read_excel(textxls2)
df3 = pd.read_excel(textxls3)
```

Next we will create a new filename with the pathlib we imported in the beginning.  We will be able to make this filename have the previous date information we calculated earlier.  We will use the Pandas ExcelWriter function to complete it.

```python
newpath = Path(r"\\testserver\testlocation\compiled"+year+month+day+".xlsx")
writer = pd.ExcelWriter(newpath)
```

Here we have the next bit of code to take the dataframes from each file we read in and put them in the file noted as the variable writer above.  The next agrument is the tab in the excel spreadsheet that we have named Compiled.  The next agrument is the row you want to start on in the spreadsheet.  Since I'm just putting them next to each other we will start each one at zero.  The last agrument is the column you will start at when putting the dataframe into the sheet.  You will need to know the size of your data so you can make it look reasonably easy to read for the requestor.

```python
df1.to_excel(writer, 'Compiled', startrow=0, startcol=0)
df2.to_excel(writer, 'Compiled', startrow=0, startcol=19)
df3.to_excel(writer, 'Compiled', startrow=0, startcol=38)
```

The last thing you have to do is save all the changes you've made with to the writer and your done!

```python
writer.save()
```

Thanks for checking out this article.  If you have any questions you can email me at dawalkerdev@gmail.com.