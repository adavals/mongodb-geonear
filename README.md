# mongodb-geonear
Finding the nearest store based on a stores dataset with longitude and latitude using MongoDB Compass.
Steps to follow:
## Step 1. Importing the dataset
The dataset sample is [Starbucks stores csv sample](https://github.com/beduExpert/A1-Introduccion-a-Bases-de-Datos-Santander2021/blob/experto/Sesion-08/Reto-01/datos/directory.csv)

On MongoDB Compass create a database called: Starbucks and a collection called: directory.

Go to Add Data --> Import file --> Select File --> Import

![Step 1](https://github.com/adavals/mongodb-geonear/blob/main/img/Step1.png)
## Step 2. Create a legacy array field with coordinates
Open MongoShell at the bottom of MongoDB Compass window and introduce the following commands:
1. Select database
use Starbucks
2. Create a new array field on the directory collection using the longitude and latitude fields imported
db.directory.updateMany(
   { },
   [
      { $set: { coordinates1000: [ $toDouble: "$Longitude", $toDouble: "$Latitude" ] }},
      { $set: { coordinatest: "$coordinates1000"}},
   ]
)
3. clean a null coordinates record in dataset to avoid error on creating index
db.directory.deleteMany( { $or: [{Longitude : null },{ Latitude : null }]})
## Step 3. Create a geospatial index
Select directory collection and:
Go to Create Index -->

## Step 4. Aggregation to find the nearest store
GeoNear


## Result with my location   19.4891442,-99.1323771

