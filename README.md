# **_GMIT-Software Development-Timetabling-System-Using-Neo4j_**
## **_Garret  Tonra G00341908_**
## **_Project Overview_**
You are required to design and prototype a Neo4j database for use
in a timetabling system for a third level institute like GMIT. The database
should store information about student groups, classrooms, lecturers, and
work hours – just like the currently used timetabling system at GMIT.

The project was guided by the following excerpt from the project instructions:
The document should clearly state what data needs to be stored by a
timetabling system, and how that data is to be represented in the database.
This means that you must clearly specify the design choices you have made
as to what data are represented as nodes, relationships, labels, relationship
types or properties. It should also specify, and give examples of, Cypher
queries for creating, retrieving, updating and deleting data.
The prototype database must contain data from GMIT’s own timetabling
system, in the structure set out in your design document. The data can be
copied or scraped from GMIT’s timetabling website, or otherwise obtained.
You should include, as a separate section in your design document, how you
obtained the data in your prototype database.

## **_Research & Implementation Tools Used_**

  - Neo4j IDE (discription below)
  - Cypher Query Languge
  - NotePad++
  - MicroSoft Excel CSV Spreadsheets
  - Stack Overflow (Great for Problem Solving)
  - Internet

# **_Neo4j_**
[![N|Solid](https://cldup.com/69EJDUYNho.png)](https://neo4j.com/nsolid)

  - Neo4j is a graph DataBase 
  - It is Acid, all data modifications is downloaded within a transaction. Transactional processing ensures data reliability.
  - It is implemented in Java( works on any platform).
  - Comes with Enterprises features this means it can scaleup as well as scale out.
  - It is comfortable with Billions of Entities. 
  - It has a restful APi & is Schemaless.
  - 1.Nodes. a Node Represents an entity in the database,Represented as a circle or Node visually.
  - 2.Edges. An Edge Shows the relationship between nodes, must be directional and are defined as lines when shown visually.
  - 3.Labels. Are used to show what type of entity a node is representing or what type of relationship an edge is representing
  - 4.Properties.Are used to store information about a node eg node of person has the properties gender,age,height etc.
  

## **_Cypher_**

  - link to the [Cypher Documentation](http://neo4j.com/docs/stable/cypher-query-lang.html). 
  - Cypher is a declarative graph query language that allows for expressive and efficient querying and updating of the graph store.
  - Cypher is a relatively simple but still very powerful language.
  - Very complicated database queries can easily be expressed through Cypher.
  - This allows you to focus on your domain instead of getting lost in database access.
  

## **_The Timtable Database_**
When i started this Project I had Found it hard to grasp the concept of what was required i first thought that i had to redisgn Gmits whole database to remake it as such. 
But once the lecturer went into a bit more detail of what was required and along with the project spec I had a good idea of what was needed  so I went and started to play around with neo4j to see how the system operates checked out the syntax and the query language cypher which is built into the neo4j database program 

I started to identify what i thought where the main things i needed 
eg a course, module  and so on after a lot of scrapped paper and redoing out a ways to design this database i came to the final finished product by doing the Following Below    

## **_Data Sourcing_**
To get my data I went mostly on the GMIT website and into the time table section there i got most of my data.
Once i had completed this task,I then imported each file into Notepad++ . 

I found this extremely useful as i was able to use notepad++'s versatility to remove html tags,artifacts & any unawanted whitespaces.
I was able to accomplish this with simple Find and replace all functions in Notpad++ also i could insert comma seperators where necessary using another useful tool built in using notepad++ which was Using
<br><b>Alt + Mouse dragging</b> or <b>Alt + Shift + Arrow</b> keys to switch to column mode<br>
I have referenced how to use this column mode via link in my references section

At first I tried to merge all the data gathered into one big cypher file that would create nodes and the relationships but after creating and recreating this file numerous time I found this method was just was not working and time being a limiting factor.</br>
I opted to stick to the traditional way of loading the nodes and relationships into neo4j via :
</br><b>LOAD CSV</b> & <b>LOAD PERIODIC COMMIT</b>
Which worked alot better and created all my relationships and nodes on the fly although it being a bit slower than just one cypher file 
but i worked better for me personally</br>
</br>
For Anyone who would Like to avail of this Database, they can just copy and paste the scripts in supports folder and run in the Neo4j Browser.

## **_Deciding my Nodes and relationships_**
So deciding what nodes and relationships also what properties and what to name at I was a few days putting together a rough graph on paper as Well as that i scrapped a few that i had half working but seen that the design was no proper for what i needed.
What was catching me out was the time i didnt know to make a node itself or have it as a property. i opted to make a Timeslot instead as this had a Day and a time so i could cut down the amount of Edges and Nodes consdierably

**The Database Nodes**
Here is a Table to Represent my Nodes and what description they have as well as Labels and Propertys 
MyDatabase consists of 5 different types of nodes which are outlined in the table below along with their
properties and labels.

Node | Description | Label(s) | Property(s)
------------ | ------------- | ------------- | -------------
Lecturer Node | Lecturers in Gmit Database  | Lecturer | Name
Module Node | modules in the Database  | Module | module,semester
Group Node| Group nodes split into 3 groups | Group | group 
Timeslot Node | Node representing the time slot time and days the Database has  | Timeslot | slot
Room Node | Which room the Database has  | Room | description,room 

**The Database Relationships**

Next we have the relationships that connect these nodes together, the project consists of four different types of relationships, these relationships are outlined in the
table below.

Relationship | Description | Connected Nodes | Label(s) | Property(s)
------------ | ------------- | ------------- | ------------- | -------------
Attends_Class_At Relationship | Represents what group attends class at what Timeslot | Group, timeslot | Attends_Class_At | id,type
In_Use Relationship | Represents which room is in use at what Timeslot  |Room, Timeslot | In_Use | id,type
Lecturer_Has_Module Relationship | Represents which Lecturer has a module   |Lecturer,Module | Lecturer_Has_Module | id,type
Lecturing Relationship | Represents which Lecture is Lecturing at what Timeslot  | Lecture, TImeslot | Lecturing | id,type
Taught_at Relationship | represents the Module Taught_At What Timeslot | TimeSlot, Module | Taught_At |id,type

## **_Queries_**
In the Introduction section of this documentation i mentioned that i needed to come up with interesting queries for the database i have created. I spent some time trying 
to think of some interesting ones and i think the three here do have some interesting data. I also wanted to make use of different keywords and functions and show my understanding
of them. So the three interesting queries for my database are as follows:

#### **_Match all lecturers all groups and all modules taught on wednesday_**

```
MATCH p=(l:Lecturer)-[:Lecturer_Has_Module]->(m:Module)-[r:Taught_At]->(t:Timeslot)<-[:In_Use]-(a:Room) where t.slot contains "Wednesday" RETURN p
UNION
MATCH p=(g:Group)-[:Attends_Class_At]->(tt:Timeslot) where t.slot contains "Wednesday" RETURN p
```

#### **_match the lecturer that teaches Graph Theory to group b on wednesday at 12 _**

```
MATCH p=(l:Lecturer)-[:Lecturer_Has_Module]->(m:Module)-[r:Taught_At]->(t:Timeslot)<-[:In_Use]-(a:Room) where m.module="GRAPH THEORY" and  t.slot contains "Tuesday" RETURN p
UNION
MATCH p=(g:Group)-[:Attends_Class_At]->(tt:Timeslot)where g.group="B" AND tt.slot contains "Tuesday at 12" 
RETURN p
```

#### **_Match the modules that Group A have at each timeslot_**

``` 
MATCH p=(m:Module)-[:Taught_At]->(t:Timeslot)<-[:Attends_Class_At]-(g:Group) Where g.group CONTAINS "A" return p
```

#### **_Find lecturer & room that software testing is in on a monday_**
``` 
MATCH p=(l:Lecturer)-[:Lecturer_Has_Module]->(m:Module)-[r:Taught_At]->(t:Timeslot)<-[:In_Use]-(a:Room) where m.module="Software Testing" and a.room="G0481" and  t.slot contains "Monday" RETURN p
```


## **_Conclusion_**
Although My knowledge of Neo4j and cypher were minimal I feel that the language and database are very user friendly and a great 
Way to represent a database visually to Customers and Clients using potentially billion of nodes for example face 

I would also like to point out although I did not use a Deptarment,Course or Campus nodes for this representation they would not be to hard to implemnet afterwards as its creating a node and relationship and link them with the existing nodes already in the Database. which means my design is easily scalable with not to much retweek of the existing database already there.My vision for this database is to be simplistic to a potetial client and that it would be very easy to explain as a visual representation hence why they say a picture speaks a thousand words   
 
## **_References_**

1. [Neo4J website](http://neo4j.com/), the website of the Neo4j database.
2. [Notepad++ Tricks](http://a4apphack.com/featured/tricks-with-notepad). Found this very useful for parsing plain text and removing unwanted
3. [Neo4J IN Keyword](http://neo4j.com/docs/stable/cypher-introduction.html) contains information in the introduction section of the Cypher documentation showing how to use the IN keyword to reference multiple properties at once. 
4. gmit timeTable
5. Questions I asked on stack overflow Stackoverflow http://stackoverflow.com/questions/43435905/neo4j-multiple-relationship-combine
6. http://neo4j.com/docs/stable/cypher-query-lang.html assisted creating the database
7. Ian McLoughlin's problem sheets 
