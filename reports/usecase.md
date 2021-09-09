# MVP - MONAS (Monitoring and Analysis System)

**Reference tasks**
> **Task 5 (Use case, KPI and MVP)**<br>*During phase 1, a close interaction with NGI-DAPSI members as well as with other funded projects or stakeholders will bring to the definition of the use case to be implemented in phase 2, the KPI to be evaluated and the MVP to be implemented.*

### Overview
During phase 1 we have established a partnership with an Italian company (ALMA Elettronica) who design, produce and sell custom hardware control boards for different markets. One of their most valuable product is the TXS1 device which is primary used to monitor and control the temperature of cast resin transformers. The device is equipped with a MODBUS TCP interface which allows to monitor and control the device over Internet. Cast resin transformers are used to supply power (400 KVA - 3 MVA) to very large buildings like malls, hospitals, stations and so on. Usually these huge and heavy devices are stored in remote and high secured locations, often difficult to be reached by technicians for inspection. What we propose is a typical Internet of Things Web application which performs a real-time monitoring of cast resin transformers's temperatures and detects anomalous conditions. Thanks to business intelligence tools, the application 

### Problem solution fit
We analyzed the problems and pains of the client and we have found that thanks to the DASI Breaker technology we can provide a solution that would relive the client's pains.

![Problem solution fit](../imgs/mvp/Diapositiva2.png)

### The solution and business model
The proposed solution is shown in the following figure.

![MONAS-Monitoring And Analysis System](../imgs/mvp/Diapositiva3.png)

The solution consists in providing a companion service with the TSX1 devices already sold by ALMA Elettronica to its customer (i.e., cast resin transformer maker). The customer will then be able to re-sell the service to its own customers. The solution enables the real time monitoring of the temperatures of each transformer (four temperatures are monitored for each transformer) in order to detect anomalous conditions and send real time notifications to the interested actors (e.g. maintenance operator). At the same time, the solution use business intelligence technologies to evaluate the life conditions of the transformers and to allow predictive maintenance.  

### Architecture
The MONAS architecture is following depicted.

![MONAS architecture](../imgs/mvp/Diapositiva4.png)

As shown in the above figure, the core part of the architecture is SEPA, enhanced with the results of the DASI Breaker project (marked in red). In particular, thanks to DASI Breaker the collection of real-time data (i.e., temperatures) can be performed in two ways: using [SOLID](https://solidproject.org/) or [NGSI-LD](https://www.etsi.org/deliver/etsi_gs/CIM/001_099/009/01.01.01_60/gs_cim009v010101p.pdf). The same two approaches can be used for what concerns the delivery of data to [Apache Superset](https://superset.apache.org/), the open source business intelligence tool adopted by MONAS. The implementation of the solution would allow to evaluate the pro and cons of the two approaches. An important result of DASI Breaker which would be essential for the implementation of MONAS is the support to the [Web Access Control (WAC)](https://github.com/solid/web-access-control-spec) mechanism implemented by SOLID to **preserve data privacy**. This mechanism has been ported inside SEPA so that, regardless the SPARQL endpoint, different access privileges can be granted to the clients (i.e., reading and writing access to graphs). The users and devices authorization is managed by [Keycloak](https://www.keycloak.org/). Along with the business intelligence analysis, a series of notification services could be implemented according to the client's needs.


### Ontology
Data interoperability is granted by the MONAS ontology shown in the following figure.  

![The MONAS ontology to enable data interoperability](../imgs/mvp/Diapositiva5.png)

The ontology is based on recognized and standard ontologies like [SOSA](https://www.w3.org/TR/vocab-ssn/) and [QUDT](http://www.qudt.org/2.1/catalog/qudt-catalog.html).
