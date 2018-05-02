# Querying Google Sheets 

Google's Query language is a fast and powerful way to analyze data in your Google sheets that borrows many of the patterns used in SQL. This tutorial will familiarize you with the basics of how to use queries to answer specific questions from a sample data set.

### Import Sample Data

First we need some sample data to play with. We'll use the import function to pull in a table of world population data from Wikipedia:

```
=IMPORTHTML("https://en.wikipedia.org/wiki/List_of_countries_by_population_(United_Nations)", "table", 3)
```

This function finds the 3rd table at the above URL, scrapes data from each cell, and imports it into your Google Sheet. Cool! But, let's do one more thing to make querying this data a little easier.

Click any cell with data to highlight it, then press CMD + A to highlight the entire table. With the whole table highlighted, click Data > Named Ranges to open the Named Ranges panel — it will fly out from the right.

A named range is a nickname you can give to a group of cells. Let's name ours 'countries'.

Great! Now we're ready to start querying!

Highlight cell J1 — we'll use this cell to type in all the queries below:



### Query Basics

Show my a copy of the entire data set:

```
=QUERY(countries, "select *",1)
```

Show me only the Countries and Region columns:

```
=QUERY(countries, "select B, C",1)
```



### Getting specific with the 'Where' Clause

The WHERE clause is a great way to start asking more specific questions from your dataset.

Show me countries with a population greater than or equal to 100 million:

```
=QUERY(countries, "SELECT B, E where E >= 100000000")
```

Show me countries with 'es' in their name:

```
=QUERY(countries, "SELECT B where B contains 'es'")
```

Show me countries *without* 'es' in their name:

```
=QUERY(countries, "SELECT B where not B contains 'es'")
```

Show me countries that start with 'S':

```
=QUERY(countries, "SELECT B where B STARTS WITH 'S'")
```

Show me countries that end with 's' (remember to be case sensitive:

```
=QUERY(countries, "SELECT B where B ENDS WITH 's'")
```

Show me countries that contain 'United':

```
=QUERY(countries, "SELECT B where B CONTAINS 'United'")
```

Let's get even more specific add in the AND clause! 

Show me all the countries in the Americas *that also have* a population less than 100,000:

```
=QUERY(countries, "select B, F WHERE C = 'Americas' AND F <= 100000", 1)
```

AND clauses can be strung together ad infinitum.

This query returns all the countries in the Americas with populations less than 100,000 and greater than 10,000:

```
=QUERY(countries, "select B, F WHERE C = 'Americas' AND F <= 100000 AND F >= 10000", 1)
```



### Aggregate Values

Maybe one of the first things you want to do when exploring a dataset is see some simple aggregations: the max, min, and average values in certain fields. Let's find the max, min, and average 2017 population for the countries in our dataset:

```
=QUERY(countries, "select max(F), min(F), avg(F)", 1)
```

That's cool, 

With a slight modification we can see the max, min, and average populations of each continent:

```
=QUERY(countries, "select C, max(F), min (F), avg(F) GROUP by C", 1)
```

'Count' and 'Group By' together are another great way to explore a data set. Combined, they allow you to count up items in one or more columns and see and aggregate view.

Let's use these two clauses to see how many countries in our list are in each region:

```
=QUERY(countries, "SELECT C, count(B) GROUP by C", 1)
```

Let's add in the 'ORDER BY' clause to show us a list of all regions, ordered ascending from highest to lowest:

```
=QUERY(countries, "SELECT C, count(B) GROUP by C ORDER by C asc", 1)
```

The 'PIVOT' clause will aggregate values into a single row.

For example, if we want to show the sum of all 2017 populations by region we could use:

```
=QUERY(countries, "SELECT sum(F) pivot C", 1)
```



### Doing Some Maths

You can also analyze your data by doing math operations as part of your queries.

Let's now show a list of countries with their 2017 population as a percentage of the total world population:

```
=QUERY(countries, "select B, C, (F / 7550262101) * 100", 1)
```

Our percentage data looks cool, but the column header is super readable. We could click into the cell and rename it — but we can also set the name as part of the query by adding a 'label' parameter:

```
=QUERY(countries, "select B, C, (F / 7550262101) * 100 Label (F / 7550262101) * 100 'Percentage'", 1)
```



### References

Google Query Language Documentation: https://developers.google.com/chart/interactive/docs/querylanguage

Google Sheets Query Function overview by Coding Is For Losers: https://codingisforlosers.com/google-sheets-query-function/#why



