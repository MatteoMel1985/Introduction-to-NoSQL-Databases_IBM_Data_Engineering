<h1 align="center">Setting up MongoDB in the lab environment</h1>  

Navigate to Skills Network Toolbox.  

![Skills Network](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/skills-network-toolbox.png)  

You will notice MongoDB listed there, but inactive, which means the database is not available to use.  

![MongoDB](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/inactive-mongodb.png)  

Once you click on the database, you will see more details and a button to start the database.  

![Database](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/inactive-mongodb-details.png)  

Clicking the start button runs a background process to configure and start your MongoDB server.  

![Starting](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/starting-mongodb.png)  

Soon, your server is ready for use. This deployment has access control enabled and MongoDB enforces authentication. So, take note of the password. You will need this password to login as `root` user.  

![Password](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/mongodb-active.png)  

You can now open a new terminal window.  

![Terminal](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/new-terminal.png)  

The new terminal window opens at the bottom of the screen as seen in the following image.  

![Example](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/terminal-launched.png)  

<h1 align="center">Setting up Cassandra in the lab environment</h1>  

Navigate again to the Skills Network Toolbox.  

![Skills Network](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/skills-network-toolbox.png)  

Cassandra is listed, but inactive, which means a database is not available to use. Open it in a new tab.

![Cassandra](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/inactive-cassandra.png)  

Once you click on the database, you will see more details and the button to start the database.

![Database2](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/inactive-cassandra-details.png)

Clicking the Start button runs a background process to configure and start your Cassandra server.

![Starting2](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/starting-cassandra.png)  

Shortly after that, your server is ready for use. This deployment has access control enabled and Cassandra enforces authentication. So, take note of the password as you will need the password to login as a `cassandra` user. 

![Cassandra User](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/images/cassandra-active.png)  

<h1 align="center">Exercise 1: Working with a MongoDB database</h1>  

## ***Download sample data file***

Download `movies.json` by running this command in the terminal.

```bash
curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMSkillsNetwork-DB0151EN-edX/labs/FinalProject/movies.json
```

Once done, you should see it appearing in the Explorer pane on the left.  

## ***Task 1***  

To import `movies.json` into a MongoDB server in a database named `entertainment` and a collection named `movies`, check your password, the host, and the port, and run the following code (modify it accordingly to these parameters in your lab). 

```bash
mongoimport \
  -u root \
  -p 'insert_your_password_here' \
  --authenticationDatabase admin \
  --db training \
  --collection bigdata \
  --file movies.json \
  --host 172.21.58.14 \
  --port 27017
```
Take a screenshot of the output and name it `Task 1.png`. 

![Task 1](https://github.com/MatteoMel1985/Introduction-to-NoSQL-Databases_IBM_Data_Engineering/blob/main/Tasks/Task%201.png?raw=true)

## ***Task 2***  

In the test, to write a MongoDB query to find the year in which most number of movies were released, you are suggested to use the `group` stage. However, before to do so, we must enter into `mongosh`. To do so, run the `MONGO_COMMAND` that you find in `Connection Information` from the terminal (do not use mine, ensure that the string is the one provided within your MongoDB). 

```bash
mongosh "mongodb://root:insert_your_password_here@172.21.58.14:27017/?authSource=admin"
```

Once inside, you must select the database `training`; hence, launch the following command.  

```bash
use training
```

Finally, we can run our aggregation query. 

```javascript
db.bigdata.aggregate([
  {
    $group: {
      _id: "$year",
      count: { $sum: 1 }
    }
  },
  {
    $sort: { count: -1 }
  },
  {
    $limit: 1
  }
])
```

Take a screenshot of the query and its result and name it `Task 2.png`.  

![Task 2](https://github.com/MatteoMel1985/Introduction-to-NoSQL-Databases_IBM_Data_Engineering/blob/main/Tasks/Task%202.PNG?raw=true)  

## ***Task 3***  

Now we are requested to write a query to find the number of films released after the year 1999. To do so, write the following query.  

```javascript
db.bigdata.aggregate([
  {
    $match: {
      year: { $gt: 1999 }
    }
  },
  {
    $count: "movies_after_1999"
  }
])
```

Take a screenshot of the output and save it as `Task 3.png`.  

![Task 3](https://github.com/MatteoMel1985/Introduction-to-NoSQL-Databases_IBM_Data_Engineering/blob/main/Tasks/Task%203.PNG?raw=true)  

## ***Task 4***  

Continuing our study, we are now tasked with writing a query that finds out the average votes for movies released in 2007. The integrated hint suggests that we use the `$match` operator to filter for films released in 2007 and `$group` with `$avg` operator to find average votes. Hence, the following command. 

```javascript
db.bigdata.aggregate([
  {
    $match: { year: 2007 }
  },
  {
    $group: {
      _id: null,
      averageVotes: { $avg: "$Votes" }
    }
  }
])
```

Take a screenshot of the output in the terminal and save it as `Task 4.png`.  

![Task 4](https://github.com/MatteoMel1985/Introduction-to-NoSQL-Databases_IBM_Data_Engineering/blob/main/Tasks/Task%204.png?raw=true)  

## ***Task 5***  

Finally, the last task with `MongoDB`. We must export the fields `_id`, `title`, `year`, `rating` and `Director` from the `movies` collection into a file named `partial_data.csv`.  

To do so we must exit `MongoDB` CLI, simply by typing the command  

```bash
exit
```

From now on, what we do is very similar to `Task 1`. as we will use the same URL from the `Connection Information`, but with the command `mongoexport`.  

```bash
mongoexport \
  --uri="mongodb://root:insert_your_password_here@172.21.58.14:27017/training?authSource=admin" \
  --collection bigdata \
  --type=csv \
  --fields _id,title,year,rating,Director \
  --out partial_data.csv
```

Finally, take a screenshot of the output and save it as `Task 5.png`.  

![Task 5](https://github.com/MatteoMel1985/Introduction-to-NoSQL-Databases_IBM_Data_Engineering/blob/main/Tasks/Task%205.png?raw=true)  

<h1 align="center">Exercise 2 - Working with a Cassandra database</h1>  

