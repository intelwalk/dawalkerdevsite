---
title: "Getting Started With SQLite3 And Golang"
date: 2020-08-05T05:15:47-05:00
draft: false
image: featured-imaged.jpg
tags: [
	"golang",
	"development",
	"howto"
]
categories: ["development",
				"golang",
				"howto"
				]

---

# <center>Connecting to a SQLite Database using Golang</center>

Connecting to a SQLite database is pretty easy with Golang. Below I will show you a basic example of how to access data and print it out to the command line.

Here will we will need to import the necessary packages so we can access the database in our code. We need to use the underscore notation for the sqlite3 package since we only need it for side-affects in our program.

```go
import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/mattn/go-sqlite3"
)
```

This will allow us to print our data out after we pull it out of the database into struct we will talk about next. We need to have a struct so we easily manipulate the data in our code. Below I have this basic struct for us to use for this example.

```go
type TestData struct {
	Index string
	Name  string
	Price float64
}
```

After we create this struct in our main.go file we can then start preparing to connect to the database. First we need to let our program know what type of database file and name we will be working with as shown below.

```go
database, err := sql.Open("sqlite3", "./test.db")
if err != nil {
	log.Fatal(err)
}
```

At this time we can also start pumping out log messages for some specific details incase we have a failure to open our database file and need to trouble shoot it more before moving on to the next piece.

```go
rows, err := database.Query("Select * from TestData;")
if err != nil {
	log.Fatal(err)
}
```

This will allow our query results to be stored in the rows variable and also allow us to trouble shoot with the err if we get an error in return when attempting to retrieve our data. We also need to remember that Query takes a string, be it a string variable or parenthesis separated string.

```go
defer rows.Close()
```

The defer command on rows will deallocate these resources when we are through with them. This usually needs to be done anytime we are dealing with database resources. The next part we will finally be getting to some data.

```go
var testdatas []TestData
var record TestData
```

Surely you will be naming convention will be on spot when your working with your program, but for this example we will keep the names simple. I will create a slice of our TestData struct that will hold all over our test data that we will be retrieving. The next variable will be the one I use for the single record per row I'm reading in and then appending (adding) ito the slice. Slice's are pretty much arrays with undetermined size allocated.

```go
for rows.Next() {
	rows.Scan(&record.Index, &record.Name, &record.Price)
	testdatas = append(testdatas, record)
}
```

As you see I'm reading each row in a for loop which is ideal in these situations and changing my variable record's data values. Then you use the append function as shown to add it to the slice. Loop and do it all over again until you have no more rows of data left.

```go
fmt.Printf("%v", testdatas)

```

This will then print out the values from your slice so you can confirm the results of the program.

```go
[{1 Laptop 814.45} {2 Monitor 211.54} {3 Desk 211.54} {4 Chair 345.65} {5 Mouse 45.67}]
```

You can check out the source code at https://github.com/intelwalk/golang-sqlite3example

Thanks for checking this article out!
