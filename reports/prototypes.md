# Prototypes

**NGSI-LD**
Postman example

* Create an Entity

    method: `POST`
    
    uri: `http://localhost:9090/ngsi-ld/v1/entities/`
    
    body:
    
    ```
    {
        "id": "http://people/People1",
        "type": "Person",
        "name": "P1",
        "nickname": "P1IsTheBest",
      "image":"http://nouri/img_p1.png",
        "telephone":"333 33333333",
        "@context": [
           "https://uri.etsi.org/ngsi-ld/v1/ngsi-ld-core-context.jsonld",
          "https://json-ld.org/contexts/person.jsonld"
        ]
    }
    ```

    header: `content-type:application/ld+json`

* Get Entity by type, no context header

    method: `GET`

    uri: `http://localhost:9090/ngsi-ld/v1/entities?type=http://xmlns.com/foaf/0.1/Person`

    header: `content-type:application/ld+json`

* Get Entity by type, no context header

    method: `GET`

    uri: `http://localhost:9090/ngsi-ld/v1/entities?type=http://xmlns.com/foaf/0.1/Person`

    headers: 

    `content-type:application/ld+json`

    `Link:<https://json-ld.org/contexts/person.jsonld>; rel="http://www.w3.org/ns/json-ld#context"; type="application/ld+json"`
    
    
TestSuite 

You can run TestSuite to verify the actual status of the implementation, [TestSuite Repo](https://github.com/FIWARE/NGSI-LD_TestSuite)


**SOLID**
This Demo has the aim of showing the possibility to have authorization on low level SPARQL querys. We have have deployed a web application to show how it works.

