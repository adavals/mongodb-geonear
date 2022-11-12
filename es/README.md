# mongodb-geonear

Encontrar la tienda más cercana usando un conjunto de datos de tiendas con los campos logitud y latitud con MongoDB Compass 1.33.1 - MongoDB 5.013.

## Paso 1. Importar el conjunto de datos

El conjunto de datos de ejemplo es un [csv de tiendas Starbucks](https://github.com/beduExpert/A1-Introduccion-a-Bases-de-Datos-Santander2021/blob/experto/Sesion-08/Reto-01/datos/directory.csv)

En MongoDB Compass crea una base de datos llamada: Starbucks y una colección llamada: directory.

Ir a: Add Data --&gt; Import file --&gt; Select File --&gt; Import

![Step 1](https://github.com/adavals/mongodb-geonear/blob/main/img/Step1.png)

## Paso 2. Crear un campo de tipo arreglo con coordenadas y hacer un poco de limpieza, usando el shell de Mongo

Abrir MongoSH (Shell de Mongo) en la parte inferior de la ventana de MongoDB Compass e introducir los siguientes comandos:

- Seleccionar base de datos

```javascript
use Starbucks
```

![Step 2](https://github.com/adavals/mongodb-geonear/blob/main/img/Step2.png)

- Crear un nuevo campo de tipo arreglo en la colección directory utilizando los campos importados de longitud y latitud:

```javascript
db.directory.updateMany(
   { },
   [
      { $set: { coordinates1000: [ {$toDouble: "$Longitude"}, {$toDouble: "$Latitude"} ] }}
   ]
)
```

- Limpiar un registro de coordenadas nulo en el conjunto de datos para evitar errores al crear el índice

```javascript
db.directory.deleteMany( { $or: [{Longitude : null },{ Latitude : null }]})
```

## Paso 3. Crear un índice 2dsphere

Seleccionar la colección directory y:

Ir a: Indexes Tab --&gt; Create Index --&gt; Select field: coordinates1000 --&gt; Select type 2dsphere --&gt; Create Index

![Step 3](https://github.com/adavals/mongodb-geonear/blob/main/img/Step3.png)

## Paso 4. Pipeline para encontrar la tienda más cercana

Seleccionar la colección directory y:

Ir a: Aggregations tab -- &gt; agregar una etapa geoNear con una ubicación que quieras --&gt; agregar una etapa limit 1 para obtener únicamente el documento más cercano

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

## Resultado con mi ubicación

La tienda más cercana :coffee:

Aunque el nombre de la tienda no era correcto, si era la ubicación correcta de la tienda más cercana a mi según sus coordenadas. Parece que este conjunto de datos puede tener información incorrecta.
