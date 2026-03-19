![Skills_Network](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-PY0221EN-Coursera/images/image.png)  

<h1 align="center">Final Assignment: Data Engineering for a Consulting Firm</h1>

You are a data engineer at a data analytics consulting company. Your company prides itself in being able to efficiently handle data in any format on any database on any platform. Analysts in your office need to work with data on different databases, and data in different formats. While these analysts are good at analyzing data, they count on you to be able to move data from external sources into various databases, to be able to move data from one type of database to another, and be able to run basic queries on various databases.

# ***Exercise 1: Working with a MongoDB database***  

## ***Download sample data file***

Use the following command to download the data file in your Cloud IDE project directory.  

```bash
curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/labs/FinalProject/movies.json
```

## ***A sample movie document***  

```JSON
{
  _id: '9',
  title: 'The Lost City of Z',
  genre: 'Action,Adventure,Biography',
  Description: 'A true-life drama, centering on British explorer Col. Percival Fawcett, who disappeared while searching for a mysterious city in the Amazon in the 1920s.',
  Director: 'James Gray',
  Actors: 'Charlie Hunnam, Robert Pattinson, Sienna Miller, Tom Holland',
  year: 2016,
  'Runtime (Minutes)': 141,
  rating: 'unrated',
  Votes: 7188,
  'Revenue (Millions)': 8.01,
  Metascore: 78
}
```

## ***Task 1: Import movies.json into mongodb server into a database named entertainment and a collection named movies.***  

Now import the data from the `movies.json` file into MongoDB and verify that it is stored in the entertainment database under the movies collection.

Take a screenshot of the command you used and its output.  

## ***Task 2: Write a mongodb query to find the year in which most number of movies were released***  

Write and execute an aggregation query to count movies by release year, sort the results in descending order, and return the year with the highest count.  

<details>
  <summary>Click here for Hint</summary>

The `$group` stage can be used to group documents by a certain field. You can calculate the count of movies within the group using the $sum aggregation operator.
Your query should:  

  * Group movies by their release year.
  * Calculate the total count of movies for each year.

```JSON
db.movies.aggregate([
    {
        "$group": {
            "_id": "$FIELD_TO_GROUP_ON",
            "calculatedField": { "$group_operator": 1 }
        }
    }
])
```
</details>  

## ***Task 3: Write a mongodb query to find the count of movies released after the year 1999***  

Write and execute the query and verify that the output shows 99 movies released after 1999.  

## ***Task 4: Write a query to find out the average votes for movies released in 2007***  

Write and execute the query and verify that the output shows an average vote count of 192.5 for movies released in 2007.  

<details>
  <summary>Click here for Hint</summary>
  
use the `$match` operator to filter for movies released in 2007. And `$group` with `$avg`operator to find average votes.

```JSON
db.books.aggregate([
	{ $match : "filter criteria" },
	{ $avg: { _id: "$field", averageVotes: "syntax for $avg" } }
])
```  
</details>  

## ***Task 5: Export the fields `_id`, `title`, `year`, `rating` and `Director` from the `movies` collection into a file named `partial_data.csv`***    

Take a screenshot of the command you used and its output.
