# mongodb-geonear
Finding the nearest store based on a stores dataset with longitude and latitude fields using MongoDB Compass 1.33.1 - MongoDB 5.013.

## Step 1. Import the dataset
The dataset sample is [Starbucks stores csv sample](https://github.com/beduExpert/A1-Introduccion-a-Bases-de-Datos-Santander2021/blob/experto/Sesion-08/Reto-01/datos/directory.csv)

On MongoDB Compass create a database called: Starbucks and a collection called: directory.

Go to Add Data --> Import file --> Select File --> Import

![Step 1](https://github.com/adavals/mongodb-geonear/blob/main/img/Step1.png)

## Step 2. Create an array field with coordinates using Mongo shell and some cleaning
Open MongoSH (Mongo shell) at the bottom of MongoDB Compass window and introduce the following commands:
- Select database
```javascript
use Starbucks
```
![Step 2](https://github.com/adavals/mongodb-geonear/blob/main/img/Step2.png)

- Create a new array field on the directory collection using the longitude and latitude fields imported:
```javascript
db.directory.updateMany(
   { },
   [
      { $set: { coordinates1000: [ {$toDouble: "$Longitude"}, {$toDouble: "$Latitude"} ] }}
   ]
)
```
- Clean a null coordinates record in dataset to avoid error on creating index
```javascript
db.directory.deleteMany( { $or: [{Longitude : null },{ Latitude : null }]})
```

## Step 3. Create a 2dsphere index
Select directory collection and:

Go to Indexes Tab --> Create Index --> Select field: coordinates1000 --> Select type 2dsphere --> Create Index

![Step 3](https://github.com/adavals/mongodb-geonear/blob/main/img/Step3.png)

## Step 4. Pipeline to find the nearest store
Select directory collection and:

Go to Aggregations tab -- > add a geoNear stage with a location you want --> add a limit 1 stage to get just the nearest document

```javascript
[{
 $geoNear: {
  near: {
   type: 'Point',
   coordinates: [
    -99.1323771,
    19.4891442
   ]
  },
  distanceField: 'distancia1000',
  spherical: false
 }
}, {
 $limit: 1
}]
```

![Step 4](https://github.com/adavals/mongodb-geonear/blob/main/img/Step4.png)

## Result with my location
The nearest store :coffee:

Although the name of the store was not correct, it was the correct location of the store closest to my location according its coordinates. It seems this dataset may have some incorrect information.

