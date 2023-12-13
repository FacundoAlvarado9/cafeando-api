# Cafeando API

Welcome to the RestAPI part of the Cafeando Project.

As a little reminder, the Cafeando project was my semester project for the subject WebApp Engineering from the Universidad Nacional del Sur. It consists of a fictitious specialty coffee store.

## Where can I see it working?

There is a [live demo](https://cafeando-api.herokuapp.com/) of the RestAPI currently (Dec '23) running on Heroku.

## How does it work?

### Documentation

First things first: When you land on the live demo, you will see the documentation, which is made with Swagger. This documentation is native to the project and can be found on the file [swagger.yaml](/docs/swagger/swagger.yaml).

### Technologies used

There was the option to make this RestAPI using Laravel (the backend of the project was developed with Laravel). However, I chose to make it with NodeJS. Even though there were extra points for it, I was curious to experiment for the first time with Node.

Conection to the database: It connects directly to the database using Prisma. The main configuration file is the [Schema](/prisma/schema.prisma) and it contains the data necessary for the connection to the database and the data model definitions so that we can interact with the database with not much hurdle.

The main app uses ExpressJS.

Even though this app may use of some refactoring, I tried to be as modular as possible. On the [index file](/index.js) we can just find the main subroutes to the different resources. Let's see a simple example:

For the cities ("ciudades"), we only have a link to the routes related to the resource.

`app.use('/ciudades', ciudadesRoute)`

All the rest is delegated to this route module.

`router.get('', ciudadesController.getAllCiudades)
router.get('/:cod_postal', ciudadesController.getCiudadByCP)
router.use('/:cod_postal/sucursales', sucursalesRouter)`

Which is in this step, delegated to the controller, which will be responsible from getting the solicited data from the Prisma model. For example for the request of a city based on the postal code:

```
const getCiudadByCP = async (req, res, next) =>{
  const {cod_postal} = req.params

  try{
    const ciudad = await prisma.ciudades.findMany({
      where: {
        cod_postal: validateIDParam(cod_postal)
      },
    })

    sendResult(res, ciudad)
  } catch(error){
    next(error)
  }
}
```
