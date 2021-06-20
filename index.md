## Sections
[Self-Assessment](https://arrioc.github.io/arrioc.github.oi/) | [Introduction](https://arrioc.github.io/Introduction/) | [Software Design & Engineering](https://arrioc.github.io/Software-Design/) | [Algorithms & Data Structures](https://arrioc.github.io/Algorithms-DataStructures/) | [Database](https://arrioc.github.io/Portfolio-Database/) | [Old Source Code](https://github.com/Arrioc/CS340_client-server) | [New Source Code](https://github.com/Arrioc/Enhanced-Artifact)

# NoSQL Database MongoDB
My plan for database enhancement is to utilize more advanced administrative methods to investigate and apply performance tuning on the database, with the end-goal of improving database efficiency by speeding up its reads and writes.

* ## Knowledge Acquisition
  * I learned quite a bit about how NoSQL distributed databases work. They are very efficient because they only use “rows” (only the concept of a “row” is replaced with a more flexible model which is a called “document”) whereas SQL databases use columns and rows that make up tables (Chodorow, 2013, p. 3). The ability to use only rows allows for indexing (Chodorow, 2013, p. 81). When a search is performed on an indexed field, only that field is looked at (by using a pointer) (Chodorow, 2013, pp. 81). This allows for higher performance and efficiency as the database grows (Chodorow, 2013, pp. 3-4). 

  * One of the first skills I learned was how to create a MongoDB database using the import tool to load a JSON file containing the company’s database documents and verify its creation. I then learned how to carry out manipulations within a Mongo shell using MongoDB statements that create, read, update, and delete documents (known as CRUD). I learned how to carry out queries and aggregations within the shell, both with and without projections. Projections help to organize and display data in an orderly way for the user. I also learned about superseded aggregation commands like “distinct,” which can find all distinct values for a given key (Chodorow, 2013, p. 146). Some of these commands may be outdated, but some, like “distinct” or “*.regex*.,” still carry out useful tasks. 

  * I then gained knowledge on how to implement indexes and how to use them efficiently. Indexing allows one to speed up reads, but it can also slow down reads and writes if used incorrectly (Chodorow, 2013, p. 160). Indexing requires forethought and is not for all situations; it can slow down queries when one must scan over 30% of their collection (Chodorow, 2013, pp. 102-103). I learned how to use unique indexes, single-field indexes, and multi-field indexes called compound indexes. A compound index has the power to limit the search of an entire database by more than one field and is useful for a key with many distinct values (called “high cardinality”) (Chodorow, 2013, pp. 84, 98). What I learned inside the shell would later translate to coding for modules outside the shell. These modules would carry out the same abilities as the shell commands through Pymongo, a language combining Python with Mongo.

* ## Developing Solutions
  * I met the planned objectives of enhancing the artifact’s database by applying more advanced administrative methods to improve the efficiency of read and write speeds. All the database’s queries and aggregations have been investigated by using intensive profiling on the database. Profiling is an admin command that one can turn on to log more information about operations carried out (Chodorow, 2013, p. 339). The higher the level of profiling, the more information that is recorded. I set the profile level to its highest and I set the profiler to catch any query taking longer than ten milliseconds (ms). I also wanted to know which index each query/aggregation was using (if any). I performed profile queries to gather information. I discovered that my compound index was not being used by the query it was intended for; it used a single-field index instead. I rewrote the compound index and tested it using profiling and discovered it was no faster than my single-field index at 0ms. I decided to remove the compound index, and thereby improved write speed. 

  * Profile Level:
  * ![Profile level](https://user-images.githubusercontent.com/73560858/122131646-7b150480-ce07-11eb-87b9-b970dd7c3e0f.png)
  
  * Discovery of Unused Index. Note "PlanSummary":
  * ![Studio 3T Discovery 4](https://user-images.githubusercontent.com/73560858/122134800-5459cc80-ce0d-11eb-8c50-eeae05d023ba.png)
  
  * Single Index Comparison. Note the "cursor" and "millis" (milliseconds):
  * ![stocks_read2, industry index, explain](https://user-images.githubusercontent.com/73560858/122131865-d515ca00-ce07-11eb-844c-3c5b731deac3.png)

  * Compound Index Comparison. Note the "cursor" and "millis" (milliseconds):
  * ![stocks_read2 compound index, explain](https://user-images.githubusercontent.com/73560858/122131779-b1528400-ce07-11eb-844c-12eba93ef711.png)
  
  * Index Removal, Before and After:
  * ![before and after index removal2](https://user-images.githubusercontent.com/73560858/122132372-b6fc9980-ce08-11eb-8fa2-d3ea4b57502d.jpg)

  * After finishing with query inspection and improvement, I attempted to see if I could add a database authentication feature to meet outcome coverage. I added an “admin” account to the admin database, and then added a user with and without write access to the “market” database. In an attempt to verify that users could only carry out their assigned roles, I found that they had more roles than they should. I believe this is because all accounts are on a local directory, and thus, were all superusers. I started to have disconnection problems in Codio when logging back into “admin” after testing the users accounts. Codio crashed and although I got it back up by clearing cookies and cache later on, the problem persisted until I deleted all the accounts. I decided not to go that route for now, and instead fix a flaw which I mentioned in my code review.

* ## Reflecting on the Process
  * What I did seems simple, but it was not so simple for me because this was my first time profiling. I had to overcome many obstacles; first to implement profiling, then to figure out if queries were using the indexes that were designed for them, and then to figure out how to get “millisecond” feedback for all queries and aggregations. Query information is found through Mongo’s “explain” feature, but I tried many suggested command styles and that feature doesn’t work with aggregations for me. The version of MongoDB that I am using is likely the cause. The latest version is 4.4 and I am using version 2.6.12. This is what led me to profiling and I could not get profiling to work for me in MongoDB’s shell on the school’s Codio platform.

  * I learned about a tool I could use to help me profile called IntelliShell by Studio 3T. Installing and correctly emulating my database was a bit challenging and a time-risk. I downloaded a trial of Studio 3T and used tutorials and guides to set it up. Assuring that I had mongo 2.6.12 working in Studio 3T, I configured the local connection and path to my database directory. I also needed to emulate the real database by applying my indexes. 

  * ![Figure 1](https://user-images.githubusercontent.com/73560858/122121707-f1126f00-cdf9-11eb-97f9-baca29e8e0f1.png)
  * ###### Figure 1: emulating my database through its version and my indexes using Studio 3T.
  
  * Using a Studio 3T guide, I learned how to use profiling by applying profile queries, but first I needed to execute my module’s queries. I wrote all my queries and aggregations in MongoDB shell-style commands, first testing them in my original project for accuracy and then running them in IntelliShell. I did this because once the profiler is on, one can confuse oneself with bad entries that were logged. The profiler logged my entries, and then I ran profile-based queries given by the guide to investigate each query and aggregation. I honestly did not change as much as I wanted to in fine-tuning reads and writes because almost all my indexes proved to be speeding up reads and there were no needed indexes that I missed (Factor, 2020).

  * One feature of IntelliShell that I think is exceptional is that one can see the time an aggregation takes. I conducted research in many places, and most claimed that one cannot accurately time aggregations through a Mongo shell in version 2.6.12 (Stack Exchange Inc, 2012). I felt the milliseconds given were very consistent with real-time results when comparing queries.
 
  * ![Figure 2](https://user-images.githubusercontent.com/73560858/122122006-451d5380-cdfa-11eb-9022-69884cfae35d.png)
  * ###### Figure 2: An example of a profiling command showing 16ms on the aggregation “APIindustryReport” using Studio 3T.

&nbsp;
&nbsp;
&nbsp;

  * **References**
   * ###### Chodorow, K. (2013). _Mongo DB: The definitive guide_ (2nd ed.). O’Reilly Media, Inc. https://www.oreilly.com/library/view/mongodb-the-definitive/9781449344795/

   * ###### Sriparasa, S., & D’mello, B. (2018). _JavaScript and JSON essentials : Build light weight, scalable, and faster web applications with the power of JSON_ (2nd ed.). Packt Publishing. https://search-ebscohost-com.ezproxy.snhu.edu/login.aspx?direct=true&db=nlebk&AN=1801025&site=eds-live&scope=site

   * ###### Factor, P. (2020, October 6). _How to use the MongoDB profiler and explain() to find slow queries. Studio3T._ Retrieved June 3, 2021, from https://studio3t.com/knowledge-base/articles/mongodb-query-performance/

   * ###### Stack Exchange Inc. (2012, December 12). _How can I see the execution time for the aggregate command? StackOverflow._ Retrieved June 3, 2021, from https://stackoverflow.com/questions/14021605/mongodb-how-can-i-see-the-execution-time-for-the-aggregate-command


