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

To import `movies.json` into a MongoDB server in a database `named` entertainment and a collection named `movies`, check your password, the host, and the port, and run the following code (modify it accordingly to these parameters in your lab). 

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

