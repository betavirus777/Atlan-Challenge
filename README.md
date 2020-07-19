Atlan Problem (Backend Challenge)
Atlan logo

Authorization levels in the platform
Authorization is basically the process of who is enabled to do what.
Most of the times we implement role based authorization in our product with either defined roles object in user object or the groups array in user object.

Role Based Authentication
role based

We can use Role Based Auth. that determine what access a user should have within the application. This access could be to pages, components, functionality, or data within the application. A role-based approach to authorization usually aligns with an organization’s hierarchy. An example group of roles might be ‘Manager’, ‘User’, and ‘Admin’.

But since we have different task we can move forward with Claim based Auth

role based
I personally think that this claim based auth would be better for the implementation of this type of product as this approach to authorization provides more granular control over the authorized functionality for a user. This approach is considered a claims-based authorization. Here we’re claiming that a user can take actions like ‘canEditQuestions’ or ‘addNewQuestions’ . This claim provides a level of authorization that is tied to the user and is not specific to a role within the application or organization.

For Example as in Teams Panel we can implement this type of auth to give access to certain features of the Published Form to a user.
Claim Based Auth. is better as it would allow more granular controls and permission to our app.

DATA POINTS
I would like to go for a big-data structured view of data storage which is not centered around one technology instead build on multiple data stores which supports Storing, Ingestion, Processing and analyzing the data.

Data Storage Systems
At the bottom of the stack are technologies that stores masses of raw data, which comes from our APP.
- App stores the data of the users response on this system which can be later used for other purposes after refining or processing the raw data.
- Here I would like to use any Document based data base like MONGO DB.

Data Ingestion and Integration Layer
At this data store we will pipeline the data according to our requirement like we want stream lined form of data for real time analysis of data or we want to process data as batches so we can perform calculations over the data which is collected over a period of time.
We will use this layer to pipe line our data.
- Here I would like to use KAFKA as it can serve our purpose of (Batch or Stream) data and it is can be used in production at huge scale.
-I know KAFKA have more latency compared to that of NATS but the persistence of data is my concern as its vital information which i have to handle so i need persistence layer of KAFKA as i can give up on bit of latency.

Note:- One more consideration or assumption I have made is that we need both Stream and Batch Processing so I have gone for Kafka and setting up KAFKA to work as desired is also very complex.
Data Processing and Analytics Layer
Here aur raw material or data are in state which can easily be store and process so that to show the analytics to our client.
We can process and store data like our need for example like the triggers like how many forms or responses can be accumalated by which user.
We can also show various useful analysis done basis on location and region of the user using ths process layer of data store.
We can use Apache Spark or Postgres but I have not worked with Spark as much still it would be better choice as we can run parallel queries on Spark which is 100x faster than any other possible Map-Reduce tool available.

Technologies Considered
MONGO DB - Classified as a NoSQL database program, it is document based database which will store our raw data from the Collect App where we publish our forms from the Collect Dashboard. MongoDB uses JSON-like documents with optional schemas so the storage of the data will be very efficient and flexible.
Alternatively we can go for S3 to store data but Mongo Query system is very advanced and easy to implement which will a crucial factor as we will oftenly read the mongo database for the data ingestion and processing.

KAFKA - Apache Kafka is an open-source software platform developed by the Apache Software Foundation, it is used for streaming data feeds we here using KAFKA as our messaging queue as it is very reliable and mature which can handle this type of Application.
Alternatively we could have gone for NATS but NATS is very lightweight platform and very fast due to very less latency and is very efficient when it comes to requests per second.
One more reason is KAFKA Streams is available in KAFKA so we can easily process our data as Stream.

Map-Reduce Platform - It is the data store which can perform multiple queries and structure the required data according to our need,Also it filters and parcels out data according to our need.
I have gone for Apache Spark here as our Map-Reduce platform although I do not have as much of practical experience of working on Spark Platform.

Simplified Architecture Diagram
Note :- Here I have treated controllers as my layer which intreracts with all the microservices builded for the project.
Architecture Diagram 
I have considered two end components that is the Client which is publshing forms to collect useful data for its operations to run smoothly and analytically, and the other is the Collect App which will serve as our interface for the user to enter data to the app.

The data entered by user in Collect will make a request to controller and then data layer to store the data in MONGO DB (which is our document based store to store raw input data).
Which we will further process to show triggers like response limit and etc, so that we can show client the responses he has paid for and remove form from our Collect App.We can further categorize our data to show data in reports like region wise responses to a particular question or bar chart on the responses filled by the users. I have used KAFKA to easily Stream my raw data to a batch of data to be be processed and aggregated to store vital information and triggers which are needed by us.
I have handled the user authorization via claim based authorization and initial request will be passed through the auth Service to see if the user has privelages to edit the form or he/she can perform that operation on our dashboard.

Assumptions
I have made an assumption that using nginx and multiple server instances we can bifurcate the requests of user to multiple main severs using consistent hashing or any other method so that the load on one sever is not more than that of any specific server.
I have also made assumption that the Mongo DB have to Replica Servers at least so that they can backup the lost data if there is any case of that so the consistency of the data can be maintained like shown in the following figure.
Architecture Diagram 

Potential caveats in your solution
I have not considered multi threading operations on the Main Server so that it can handle over million requests per second.
I have also not used any caching platform or service which would have made this system more efficient.
I have also not considered the complexity data stores work in right manner would be very high as it would be very hard to implement.

Potencial Feature
Potencial feature for this application would be having responses collected from collect app over a ceratin defined geographical region defined by the Client which can be implemnted in the Collect App to collect the data from that specific region so that it would be very impactful for the Client as it may have running a small business and wanted to know the mindset over the set of product or service he wants to provide.

We can also store previous custom questions asked by the user and give him suggestions to add them in his next form he creates.
