![Logo](imgs/dasi_breaker_full.png)

DASI Breaker is a semantic open interoperability solution to break down data silos and enable the development of data portability applications and services with native support for RDF and SPARQL. At low level, DASI Breaker supports OpenID Connect for client authentication and it allows to define authorization policies though the W3C Access Control List (ACL) ontology. At a higher level, an interoperable access to data is granted by open APIs like NGSI-LD, Linked Data Platform 1.0, and Solid Protocol.

The project is funded by EU under the Grant Agreement No.: 871498 Call: H2020-ICT-2018-2020

The following figure decipts the system architecture:
![Architecture](imgs/Dasi-Breaker-architecture.png)

The implementation is based on the following solutions and technologies:
1. [SEPA](https://github.com/arces-wot/SEPA)
2. [Community Solid Server](https://github.com/solid/community-server)
3. [Scorpio NGSI-LD broker](https://github.com/ScorpioBroker/ScorpioBroker)
4. [Titanium JSON-LD 1.1 Processor & API](https://github.com/filip26/titanium-json-ld)

The draft specifications of SEPA follow:
1. [SPARQL Event Processing Architecture](http://mml.arces.unibo.it/TR/sepa.html)
2. [SPARQL 1.1 Secure Event (SE) Protocol](http://mml.arces.unibo.it/TR/sparql11-se-protocol.html)
3. [SPARQL 1.1 Subscribe language](http://mml.arces.unibo.it/TR/sparql11-subscribe.html)
4. [JSON SPARQL Application Profile (JSAP)](http://mml.arces.unibo.it/TR/jsap.html)
