# Report for DASI-BREAKER
**Reference tasks**
> **Task 1 (Web Access Control)**<br>*Current SEPA implementation exploits the graph level access control offered by Virtuoso Open Source through the OpenID Connect interface provided by Keycloak. This task aims to extend this mechanism to make it independent from the underline SPARQL endpoint via the Web Access Control (WAC) specification.*
>
> **Task 2 (Linked Data Protocol 1.0)**<br>*The task aims to implement LDP 1.0 on top of SEPA. The task will produce a first implementation.*
>
> **Task 3 (NGSI-LD)**<br>*This task aims to implement the ETSI NGSI-LD HTTP binding API. The current Java implementation provided by Scorpio released by NEC under the BSD license would be considered, along with already available open source JSON-LD libraries. The task outcomes will be: a first release where the entities/attributes providing and consumption are supported.*
>
>**Task 4 (Solid Protocol)**<br>*This task aims to implement the Solid Protocol [14]. The task will benefit of Task #1 outcome for what concerns the user access control. A first prototype will be delivered at M5 and a full protocol implementation at M7.*
<!--- ## **INTRODUCTION**

alla fine -->

## **ARCHITECTURE**

The architecture of DASI-BREAKER project, shown in the following figure, is designed to be microservices-oriented and it is composed of three main components, and: (i) the Community Solid Server, (ii) the Scorpio Broker, and (iii) the SEPA (SPARQL Event Processing Architecture). The first component is an open-source implementation of the Solid specifications that provides an LDP API to our system with identity management, access control, etc. The second component is an open-source NGSI-LD context broker that implements the full NGSI-LD API specification. Both of these services use the third component as their SPARQL backend to store and retrieve data.

These APIs are not directly exposed on the internet but are hidden behind Traefik: a reverse proxy and load balancer which handles the routing of requests automatically, allowing the entire system to scale easily.

![Architecture](RackMultipart20210909-4-1giibgc_html_1a78e30c38bbe68e.png)

## **SOLID**

### **ACTIVITIES**

#### Contributions to Community Solid Server

##### Group feature for WAC

We have added the possibility for a user (WebID) to be part of a group of users. Now it&#39;s possible to give permission for a certain resource (read, write, append, control) to an entire group.

For example the following file describes a group with only one member (http://localhost:3000/user\_group#me)
```turtle
@prefix acl: <http://www.w3.org/ns/auth/acl#>;.
@prefix dc: <http://purl.org/dc/terms/>;.
@prefix vcard: <http://www.w3.org/2006/vcard/ns#>;.
@prefix xsd: <http://www.w3.org/2001/XMLSchema#>;.

<#test> a vcard:Group;
vcard:hasUID <urn:uuid:8831CBAD-1111-2222-8563-F0F4787E5398:ABGroup>;
dc:created "2013-09-11T07:18:19Z"^^xsd:dateTime;
dc:modified "2015-08-08T14:45:15Z"^^xsd:dateTime;
vcard:hasMember <http://localhost:3000/user\_group#me>. 
```

the acl of a resource can specify the permissions of a group in the following way

```turtle
@prefix acl:  <http://www.w3.org/ns/auth/acl#>;.
@prefix foaf: <http://xmlns.com/foaf/0.1/>;.

<#authorization>
 a acl:Authorization;
acl:accessTo <./myfile.ttl>;
acl:mode acl:Read;
acl:mode acl:Write;
acl:agentGroup <http://localhost:3000/test\_group#test>. 
```

Our PR has been accepted and merged into version 2.0.0 of the Community Solid Server:

[https://github.com/solid/community-server/pull/900](https://github.com/solid/community-server/pull/900)

[https://github.com/solid/community-server/pull/923](https://github.com/solid/community-server/pull/923)

contribuing to one of the biggest project reguarding Solid and receiving appreciation for our work (&quot;not just a new feature, but an improvement to the codebase overall.&quot; Ruben Verborgh).

##### Keycloak Integration with Community Solid Server

Community Solid Server has a build-in identity provider, but it is still at early age of development. Being able to use another identity provider that is powerful and open source, like Keycloak, could be very useful. So we have found a way to integrate the WebID-OIDC protocol used by the Community Solid Server with Keycloak.

[CSS\_\_\_Keycloak.pdf](https://drive.google.com/file/d/1APn7vDKzhhZPS9c3HkGmlY6MnQbW30o9/view?usp=sharing)

#### Contributions to SEPA

##### Web Access Control algorithm implementation

We have implemented the Access Control mechanism described by the solid specification (WAC) inside SEPA architecture. This will have a a dual usage:

- We can delegate the Web Access Control that was done by the community solid server to SEPA, this will lead to some advantages that we will describe in the following.
- We can use the access control mechanism for SPARQL query sent to SEPA.

let&#39;s dive into this two points.

##### WAC endpoint for Community Solid Server

We built an endpoint inside SEPA (/wac), so that the community solid server can delegate the access control mechanism to SEPA. So we keep separate from the community solid server all the logic about how to check if a user (WebID) has the requested permission for a resource. In this way we can have this logic even in a different server, and in the future we could deploy parallelism to have faster responses.

The decision to implement WAC inside SEPA also has an other benefit, we could use the publish-subscribe mechanism, that make SEPA unique, for subscribing to the LOG of the access of every resource, so that the owner knows every type of interaction of other users with his resources.

##### SEPA WAC Standalone

With the implementation of the logic of the Web Access Control, now SEPA support the authentication and authorization for SPARQL query. In this way it is possible to limit the permissions granted to specific users or group of users to make queries about specified resources.

### **RESULTS**

- Descrizione dei risultati ottenuti (basato esattamente sulle specifiche di progetto)
- Link a documentazione esterna/demo/repository
- Link: [https://pod.dasibreaker.vaimee.it](https://pod.dasibreaker.vaimee.it/)

## **NGSI-LD**

### **ACTIVITIES**

The activity for enabling NGSI-LD functionalities inside our final architecture can be mainly divided into three different steps: the evaluation of all the possible strategies for achieving the task, the implementation of the missing parts requested for this release, and the final testing phase. In the first case, we examined several alternatives, trying to understand which one was more suitable for our architecture and more generally for our architecture. In the second part, we had to dig into the open-source repositories of the tool we decided to use, in order to understand how to operate for including the new functionalities. Lastly, we made several tests with the aim of ascertaining that our implementation actually works. We encountered several obstacles in all of these phases, often because of the lack of accurate documentation in the open-source tools we used for certain tasks. We started a proactive process with the community, opening new issues and starting new discussions in order to fix or understand the problem. In some cases, like with the Scorpio broker, we forked the repository since we made a small refactor and we would like to open a Pull Request at the end of the project in order to contribute to the open-source community.

#### Preliminary evaluation

In the preliminary evaluation phase, we identified two possibilities for bringing NGSI-LD into our architecture. The first one was to implement all the NGSI-LD features as an integrated module in the SEPA architecture. More in detail, this translates into the implementation of a SEPA module that would map each NGSI-LD API to the corresponding SEPA primitive. Although a first attempt was already made with good results [[link](https://ieeexplore.ieee.org/abstract/document/8711888?casa_token=3FONYctWxroAAAAA:ftIK0D0RWPXUJ0PxL7PImsmlLgvG07gWcAA8bdMMmbNZOHG5XPpaiQlxAoL0zvWfGi_fgAdC)], and we were able to reproduce the results claimed in the previous work, we decided to reject this approach since we had no guarantee about the success of this choice due especially to the huge amount of different functionalities that should have been implemented and included in an already existing software project, and to the fact that this solution would tend to increase the coupling between services, making the SEPA almost a monolithic component of the whole project architecture. This would definitely be in contrast with our design of a microservices-oriented architecture.

The second possibility was to enable the NGSI-LD functionalities by taking advantage of an already existing open-source solution. In this way, we can benefit from the fact that it is a separate project -hence a different codebase, with different issues and maintainers- that can easily be mapped to an external service and included in our architecture. We evaluated two possibilities: the Scorpio Broker and the Orion broker. We did several preliminary tests and we decided to start working on the first one, since Orion introduced the LD support only on a fork of the official repository and does not seem to be mature enough for our purpose. Furthermore, it is written in C++, making the integration harder with other libraries we needed and that are written in JAVA.

#### Implementation

In our final architecture, the SEPA plays a crucial role in storing and accessing the resources: these can be passed through LDP or NGSI-LD and retrieved as well by both technologies: for instance, a new resource could be added to the SEPA by using an NGSI-LD _POST entity_ and retrieved through LDP.

For this reason, in order to enable such kinds of functionality in our solution, we need to design and implement a connection between Scorpio and the SEPA. Scorpio can be used as a standalone service, hence it is able to store and retrieve data by using an internal mechanism, i.e., an internal instance of a database, for these operations. Given the fact that it handles semantic data, we expected Scorpio to use a semantic repository: in this sense, our task should have been to enable the dynamic change of the connection with a different endpoint, i.e., the SEPA&#39;s one.

Nevertheless, we found out that Scorpio relies on a relational database and it exploits semantic functionalities by using redundancy mechanisms. In particular, it stores the full JSON-LD as an entity together with the _context_ and the _id_ of the entity. Clearly, this mechanism is not necessary in the case of a semantic repository, like the SEPA.

Given these conditions, in order to take advantage of all the query mechanisms already implemented in the Scorpio broker, we need to translate all the original SQL queries to SPARQL queries and add them to the SEPA adapter.

The final task can be hence summarized in this way: we receive the JSON-LD from the SCORPIO APIs, we translate the SQL queries defined for that API to SPARQL, we convert the JSON-LD into NQuads and we query the SEPA repository accordingly.

From the operational point of view, the first activity was to understand how to set up Scorpio in development mode, since no documentation for this need was provided. After a deep investigation, and a new issue on the official repo of Scorpio (link), we set up both a local environment for the development phase and a remote one for the demos on our servers. In both cases, we use docker containers for managing all the services required by Scorpio. The second activity was to study in detail the internal behavior of Scorpio in order to implement the SEPA adapter: a natural solution was to design a new DaoPattern for the SPARQL interactions with SEPA. In order to do so, we had to refactor a little bit the structure of the code, in particular by regrouping all the DAOPatterns under a _Factory_. We plan to make these modifications official to the core of Scorpio by making a Pull Request on the Scorpio&#39;s repository. The further step was to work on the translation of JSON-LD into N-Quads in order to pass the right data format to the new DaoPatter. For this task, we decided to use the _Titanium_ library, implementing a wrapper for N-Quads that converts JSON-LD to N-Quads and vice versa. This step required also to understand how to handle the context in order to enable the framing and deframing operations for the semantic data. In particular, we need to manage both the framing operations made by Scorpio and Titanium, hence correctly passing the right contexts in two different steps of the pipeline.

#### Testing

For the evaluation of the whole implementation, we decided to run two different kinds of tests, namely a set of [specific tests](https://github.com/vaimee/dasi-breaker/blob/main/reports/prototypes.md#prototypes) (available also for the demos) through the help of some auxiliary tools like _POSTMAN_ with the goal of assessing the proper working of all the operations required for this release, and some more structured tests, with the goal of verifying which NGSI-LD APIs we currently cover. In particular, for this last case, we used the official open-source testSuite provided by the [FIWARE](https://github.com/FIWARE/NGSI-LD_TestSuite) community. We detail in the following table the main results achieved and we highlight that we currently fully support the entities/attributes providing and consumption as well as the context source registration, we are currently working on the subscriptions and we need to make more investigations about the entity/attributes part.

| Resource Name 	| Test Success 	|
|---	|---	|
| Entity list 	| Yes 	|
| Entity by id 	| Yes 	|
| Entity Attribute List 	| No 	|
| Attribute by id 	| No 	|
| Subscriptions List 	| Yes 	|
| Subscription by Id 	| ? 	|
| Context source registration list 	| Yes 	|
|

For the details of the current status for the mapping of NGSI-LD APIs, please refer to [this table](https://docs.google.com/document/d/1_ZA-QoA0r5iM6AetykwMvs3iGmANZyZTThvH45QJFgc/edit?usp=sharing).


### **RESULTS**
- Mapping of the NGSI-LD APIs according to the [table](https://docs.google.com/document/d/1_ZA-QoA0r5iM6AetykwMvs3iGmANZyZTThvH45QJFgc/edit?usp=sharing) previously introduced
- Link: [https://ngsi.dasibreaker.vaimee.it](https://ngsi.dasibreaker.vaimee.it/)
- Starting discussions with the community of the [Scorpio broker](https://github.com/ScorpioBroker/ScorpioBroker/issues/230) and [FIWARE](https://github.com/FIWARE/NGSI-LD_TestSuite/issues/70)


## **DEPLOYMENT**

The entire system runs on a cluster of Linux machines grouped together via [Docker Swarm](https://docs.docker.com/engine/swarm/).

As you can see from the figure below and the [compose-file](https://todo.com/), there are 7 swarm services connected through the _default_ overlay network. This network allows the various services to communicate with each other securely; in fact, all traffic is encrypted by default using the AES algorithm in GCM mode. The only exposed services are those connected to the _traefik_ network that allows them to serve the related APIs on specific domains.

Some of these services need one or more volumes for saving their configurations and data. In the specific case of _blazegraph_ and _postgres_, since they are databases, we decided to bind them (at least temporarily) to a specific node, dedicated to saving data.

At this time the entire stack has been replicated two times to separate the development/testing environment from the production.

![Deployement](RackMultipart20210909-4-1giibgc_html_ec4380ea19d7b2b0.png)
## **FUTURE STEPS**

According to the project proposal, the next steps for the NGSI-LD part will be to keep mapping the NGSI-LD API to the right behavior on the SEPA component. In particular, we need to focus on the translation of Scorpio's SQL queries that include geospatial features into the proper SPARQL queries. Another important work will be to identify and use -according to the rest of the architecture- the security layer provided by Scorpio for granting the right access to the resources.

<!---## **CONCLUSIONS**

alla fine-->







