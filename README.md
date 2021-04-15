# RAML Cheat Sheet
RAML Cheat Sheet with the most needed stuff..


<br><br>






# Guides


<br><br>


## RAML 100
- https://raml.org/developers/raml-100-tutorial




































<br><br>
____________________________________________
____________________________________________
<br><br>

# Converter
- https://github.com/raml-org/webapi-parser

## RAML 1.0 to RAML 1.0 - Inline
```javascript
const wap = require('webapi-parser').WebApiParser
const path = require('path')


/**
 * RAML 1.0 to RAML 1.0 - INLINE
 */
async function main () {
  const inPath = path.join(__dirname, 'test.raml')
  const model = await wap.raml10.parse(`file://${inPath}`)

  const outPath = path.join(__dirname, './generated.raml')
  console.log('Generating file to:', outPath)

  await wap.raml10.generateFile(model, `file://${outPath}`)
}

main()
```


<br><br>


## RAML 1.0 to Openapi 3.0
```javascript
const wap = require('webapi-parser').WebApiParser
const path = require('path')

/**
 * RAML 1.0 to Openapi 3.0
 */

async function main () {
  const inPath = path.join(__dirname, 'test.raml')
  const model = await wap.raml10.parse(`file://${inPath}`)

  const outPath = path.join(__dirname, './generated.json')
  console.log('Generating file to:', outPath)

  await wap.oas30.generateFile(model, `file://${outPath}`)
}

main()
```





<br><br>


## RAML 1.0 to Openapi 2.0
```javascript
const wap = require('webapi-parser').WebApiParser
const path = require('path')

/**
 * RAML 1.0 to Openapi 2.0
 */

async function main () {
  const inPath = path.join(__dirname, 'test.raml')
  const model = await wap.raml10.parse(`file://${inPath}`)

  const outPath = path.join(__dirname, './generated.json')
  console.log('Generating file to:', outPath)

  await wap.oas20.generateFile(model, `file://${outPath}`)
}

main()
```





<br><br>


## RAML 1.0 to Postman 2.0
```javascript
/**
 * RAML 1.0 to Postman 2.0
 */
const fs = require('fs'),
const Converter = require('raml1-to-postman'),
const ramlSpec = fs.readFileSync('data-repository-services.raml', {encoding: 'UTF8'});

Converter.convert({ type: 'string', data: ramlSpec },
  {}, (err, conversionResult) => {
    if (!conversionResult.result) {
      console.log('Could not convert', conversionResult.reason);
    }
    else {
      console.log('The collection object is: ', conversionResult.output[0].data);
      fs.writeFileSync('export.json', JSON.stringify(conversionResult.output[0].data))
    }
  }
);
```








































<br><br>
____________________________________________
____________________________________________
<br><br>



# Include inline Data
```javascript
#%RAML 1.0
/123/Template:
  get:
    description: Get a list of templates
    responses:
      200:
        description: Returns list of templates
        body:
          application/json:
            example: |
              {
                "songs": [
                  {
                    "songId": "550e8400-e29b-41d4-a716-446655440000",
                    "songTitle": "Get Lucky"
                  },
                  {
                    "songId": "550e8400-e29b-41d4-a716-446655440111",
                    "songTitle": "Loose yourself to dance"
                  },
                  {
                    "songId": "550e8400-e29b-41d4-a716-446655440222",
                    "songTitle": "Gio sorgio by Moroder"
                  }
                ]
              }
```

<br><br>

# Include external Data
```javascript
#%RAML 1.0
/123/Template:
  get:
    description: Get a list of templates
    responses:
      200:
        description: Returns list of templates
        body:
          application/json:
            example: !include examples/Template/get.res.json
```

































































<br><br>
____________________________________________
____________________________________________
<br><br>



# ResourceTypes
- ResourceTypes is like resource in that it can specify the descriptions, methods, and its parameters. Resource that uses resourceTypes can inherit its nodes. ResourceTypes can use and inherit from other resourceTypes.

<br><br>


ResourceType is basically a template that is used to define the descriptions, methods, and parameters that can be used by multiple resources without writing the duplicate code or repeating code.

<br><br>


Let's consider an example. Resource songs want to implement two methods like GET and POST. Each HTTP method will have responseTypes, description, usage, etc.

<br><br>


Now, we have other requirements that resource artists want to implement two methods like GET and POST. So, we will make use of resourceTypes to implement the GET and POST methods and that can be used by both resources songs and artist and it can even be used by other resources in the future.




```javascript
#%RAML 1.0
baseUri: https://mocksvc.mulesoft.com/mocks/f1bfb7c0-370c-43ea-b6c2-5ec56bbf7513 # 
title: Songs

resourceTypes: 
  collection:
    usage: This API is used by <<resourcePathName>>
    description: API is used ti retrieve and create <<resourcePathName>>
    get:
      description: This API is used to retrieve all details about <<resourcePathName>>
      responses: 
        200:
          body: 
            application/json:
              example: |
                <<exampleReference1>>
    post:
      description: This API is used to create <<resourcePathName>>
      body: 
        application/json:
          example: |
            <<exampleReference2>>
      responses: 
        201:
          body: 
            application/json:
              example: |
                {"message":"<<resourcePathName>> created"}

/songs:
  type: 
    collection:
        exampleReference1: |
          {"Songs":[
          {"SongID":1,"SongName":"London Dreams"},
          {"SongID":2,"SongName":"German Whip"}
          ]}
        exampleReference2: |
          {"SongID":1,"SongName":"London Dreams","Singer":"David"}
/artists:
  type:
    collection:
        exampleReference1: |
          {"Artists":[
          {"ArtistID":1,"ArtistName":"David"},
          {"ArtistID":2,"ArtistName":"John"}
          ]}
        exampleReference2: |
          {"ArtistID":1,"ArtistName":"John","D.O.B":"27 July 1978"}

```







<br><br><br><br>




## Declaring ResourceTypes With HTTP Methods GET and POST
```javascript
resourceTypes: 
  collection:
    usage: This API is used by <<resourcePathName>>
    description: API is used ti retrieve and create <<resourcePathName>>
    get:
    post:
```

<br><br>

## Defining the HTTP POST and GET Method ResponseType and Description
```javascript
resourceTypes: 
  collection:
    usage: This API is used by <<resourcePathName>>
    description: API is used ti retrieve and create <<resourcePathNam>>
    get:
      description: This API is used to retrieve all details about <<resourcePathName>>
      responses: 
        200:
          body: 
            application/json:
              example: |
                <<examplereference1>>
    post:
      description: This API is used to create <<resourcePathName>>
      body: 
        application/json:
          example: |
            <<exampleReference2>>
      responses: 
        201:
          body: 
            application/json:
              example: |
                {"message":"<<resourcePathName>> created"}
```



<br><br>

## Calling ResourceTypes From Resources
```javascript
/songs:
  type: 
    collection:
        exampleReference1: |
          {"Songs":[
          {"SongID":1,"SongName":"London Dreams"},
          {"SongID":2,"SongName":"German Whip"}
          ]}
        exampleReference2: |
          {"SongID":1,"SongName":"London Dreams","Singer":"David"}
/artists:
  type:
    collection:
        exampleReference1: |
          {"Artists":[
          {"ArtistID":1,"ArtistName":"David"},
          {"ArtistID":2,"ArtistName":"John"}
          ]}
        exampleReference2: |
          {"ArtistID":1,"ArtistName":"John","D.O.B":"27 July 1978"}
```












































































































<br><br>
____________________________________________
____________________________________________
<br><br>



# Traits
- Traits is like function and is used to define common attributes for HTTP method (GET, PUT, POST, PATCH, DELETE, etc) such as whether or not they are filterable, searchable, or pageable.

<br><br>

Traits can be declared in the same RAML file or you can create different files for creating the traits.

<br><br>

Here, you will see how we can implement a response in traits and how it can be called by multiple resources and resourceTypes.


```javascript
#%RAML 1.0
baseUri: https://mocksvc.mulesoft.com/mocks/f1bfb7c0-370c-43ea-b6c2-5ec56bbf7513 # 
title: Songs

traits: 
    responseMessage:
      responses: 
        200:
          body: 
            application/json:
              example: |
                {"message":"<<resourcePathName>> Created"}
resourceTypes: 
  collection:
    usage: This API is used by <<resourcePathName>>
    description: API is used ti retrieve and create <<resourcePathName>>
    get:
      description: This API is used to retrieve all details about <<resourcePathName>>
      responses: 
        200:
          body: 
            application/json:
              example: |
                <<exampleReference1>>
    post:
      description: This API is used to create <<resourcePathName>>
      body: 
        application/json:
          example: |
            <<exampleReference2>>
      is: [responseMessage]

/songs:
  type: 
    collection:
        exampleReference1: |
          {"Songs":[
          {"SongID":1,"SongName":"London Dreams"},
          {"SongID":2,"SongName":"German Whip"}
          ]}
        exampleReference2: |
          {"SongID":1,"SongName":"London Dreams","Singer":"David"}
/artists:
  type:
    collection:
        exampleReference1: |
          {"Artists":[
          {"ArtistID":1,"ArtistName":"David"},
          {"ArtistID":2,"ArtistName":"John"}
          ]}
        exampleReference2: |
          {"ArtistID":1,"ArtistName":"John","D.O.B":"27 July 1978"}
/albums:
  post:
    body: 
      application/json:
        example: |
          {"AlbumsID":2,"AlbumNameName":"German Whip"}
    is: [responseMessage]
```

<br><br>

## Guides
- https://www.youtube.com/watch?v=lM18GU6blI4





<br><br><br><br>

## Calling Traits From Resources
- Traits can be called by resources using the "is" keyword.
```javascript
#%RAML 1.0
baseUri: https://mocksvc.mulesoft.com/mocks/f1bfb7c0-370c-43ea-b6c2-5ec56bbf7513 # 
title: Songs

traits: 
    responseMessage:
      responses: 
        200:
          body: 
            application/json:
              example: |
                {"message":"Data Created"}

/albums:
  post:
    body: 
      application/json:
        example: |
          {"AlbumID":1,"AlbumName":"German Whip"}
    is: [responseMessage]
```







<br><br>



## Calling Traits From ResourceTypes
- Traits can be called by resources using the "is" keyword.
```javascript
traits: 
    responseMessage:
      responses: 
        200:
          body: 
            application/json:
              example: |
                {"message":"<<resourcePathName>> Created"}
resourceTypes: 
  collection:
    usage: This API is used by <<resourcePathName>>
    description: API is used ti retrieve and create <<resourcePathName>>
    get:
      description: This API is used to retrieve all details about <<resourcePathName>>
      responses: 
        200:
          body: 
            application/json:
              example: |
                <<exampleReference1>>
    post:
      description: This API is used to create <<resourcePathName>>
      body: 
        application/json:
          example: |
            <<exampleReference2>>
      is: [responseMessage]
```
