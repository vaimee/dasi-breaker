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

A prototype web application is available to show the possibility to have authorization at SPARQL queries/update level.

We have used:
* An Angular web application as the client
* The SEPA architecture for the Web Access Control, Token Validation and Query Parsing
* The Identity Provider Keycloak
* Blazegraph as Triple Store

The prototype web application let us log in through Keycloak using the OpenID Connect protocol. Once we are authenticated we can send the SPARQL query that we want through a simple editor, only if we have the necessary permission we will be able to perform the query and download the data (if any), otherwise we will receive an error. Simple example queries are already prepared and easy to use.

<img width="661" alt="SEPA_secure" src="https://user-images.githubusercontent.com/18251575/132925028-3849a994-9e19-4b39-9d8c-f3e78f310f42.png">



