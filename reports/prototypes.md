# Prototypes

**NGSI-LD**

Postman example

* Create an Entity</br>
    method: `POST`</br>
    uri: `http://localhost:9090/ngsi-ld/v1/entities/`</br>
    body:</br>
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

* Get Entity by type</br>
    method: `GET`</br>
    uri: `http://localhost:9090/ngsi-ld/v1/entities?type=http://xmlns.com/foaf/0.1/Person`</br>
    headers: </br>
    `content-type:application/ld+json`</br>
    `Link:<https://json-ld.org/contexts/person.jsonld>; rel="http://www.w3.org/ns/json-ld#context"; type="application/ld+json"`</br>
    Response [200]:</br>
```
    [
      {
        "id": "http://people/People1",
        "type": "Person",
        "telephone": "333 33333333",
        "image": "http://nouri/img_p1.png",
        "nickname": "P1IsTheBest",
        "name": "P1",
        "@context": [
              "https://json-ld.org/contexts/person.jsonld",
              "https://uri.etsi.org/ngsi-ld/v1/ngsi-ld-core-context.jsonld"
           ]
        }
    ]
```

* Get Entity by type, no context header</br>
    method: `GET`</br>
    uri: `http://localhost:9090/ngsi-ld/v1/entities?type=http://xmlns.com/foaf/0.1/Person`</br>
    header: `content-type:application/ld+json`</br>
    Response [200]:</br>
```
 [
    {
        "id": "http://people/People1",
        "type": "http://xmlns.com/foaf/0.1/Person",
        "http://schema.org/telephone": "333 33333333",
        "http://xmlns.com/foaf/0.1/img": {
        "id": "http://nouri/img_p1.png"
        },
        "http://xmlns.com/foaf/0.1/nick": "P1IsTheBest",
        "name": "P1",
        "@context": [
          "https://uri.etsi.org/ngsi-ld/v1/ngsi-ld-core-context.jsonld"
        ],
    }
]
```

* Get Entity by ID</br>
    method: `GET`</br>
    uri: `http://localhost:9090/ngsi-ld/v1/entities/http://people/People1`</br>
    headers: </br>
    `content-type:application/ld+json`</br>
    `Link:<https://json-ld.org/contexts/person.jsonld>; rel="http://www.w3.org/ns/json-ld#context"; type="application/ld+json"`</br>
    Response [200]:</br>
```
    {
        "id": "http://people/People1",
        "type": "Person",
        "telephone": "333 33333333",
        "image": "http://nouri/img_p1.png",
        "nickname": "P1IsTheBest",
        "name": "P1",
        "@context": [
          "https://json-ld.org/contexts/person.jsonld",
          "https://uri.etsi.org/ngsi-ld/v1/ngsi-ld-core-context.jsonld"
        ]
    }
```


* Get Entity by ID, no context header</br>
    method: `GET`</br>
    uri: `http://localhost:9090/ngsi-ld/v1/entities/http://people/People1`</br>
    headers: </br>
    `content-type:application/ld+json`</br>
    Response [200]:</br>
```
{
    "id": "http://people/People1",
    "type": "http://xmlns.com/foaf/0.1/Person",
    "http://schema.org/telephone": "333 33333333",
    "http://xmlns.com/foaf/0.1/img": {
    "id": "http://nouri/img_p1.png"
    },
    "http://xmlns.com/foaf/0.1/nick": "P1IsTheBest",
    "name": "P1",
    "@context": [
      "https://uri.etsi.org/ngsi-ld/v1/ngsi-ld-core-context.jsonld"
    ]
}
```

* Get Entity by type and attrs</br>
    method: `GET`</br>
    uri: `http://localhost:9090/ngsi-ld/v1/entities?type=Person&attrs=name`</br>
    headers: </br>
    `content-type:application/ld+json`</br>
    Response [200]:</br>
```
[
  {
        "id": "http://people/People1",
        "type": "Person",
        "name": "P1",
        "@context": [
          "https://json-ld.org/contexts/person.jsonld",
          "https://uri.etsi.org/ngsi-ld/v1/ngsi-ld-core-context.jsonld"
        ]
    }
]
```

* Get Entity by type and attrs, no context header </br>
    method: `GET`</br>
    uri: `http://localhost:9090/ngsi-ld/v1/entities?type=http://xmlns.com/foaf/0.1/Person&attrs=http://xmlns.com/foaf/0.1/img`</br>
    headers: </br>
    `content-type:application/ld+json`</br>
    Response [200]:</br>
```
[
  {
        "id": "http://people/People1",
        "type": "http://xmlns.com/foaf/0.1/Person",
        "http://xmlns.com/foaf/0.1/img": {
        "id": "http://nouri/img_p1.png"
        },
        "@context": [
          "https://uri.etsi.org/ngsi-ld/v1/ngsi-ld-core-context.jsonld"
        ]
    }
]
```

TestSuite 

You can run TestSuite to verify the actual status of the implementation, [TestSuite Repo](https://github.com/FIWARE/NGSI-LD_TestSuite)
<hr></hr>

**SOLID**

This Demo has the aim of showing the possibility to have authorization on low level SPARQL querys. We have have deployed a web application to show how it works.

