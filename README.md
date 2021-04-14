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
```yaml
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
```yaml
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
```yaml
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

