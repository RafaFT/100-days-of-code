# #100DaysOfCode Log - Round 1 - Rafael

The log of my #100DaysOfCode challenge. Started on May 13, Monday, 2019.

## Log

### R1D1 - 2019/05/13
Started writing unittests for the _FinancialIndicesApi_ class, and finished the tests for the _create_api_url() method.

### R1D2 - 2019/05/14
Kept working on the same unittest script. Finished the tests for the method _get_json_results.

### R1D3 - 2019/05/15
Kept working on the script 'test_bcb_api.py', which is a unittest based file for the class FinancialIndicesApi.

### R1D4 - 2019/05/17
Kept working on 'test_bcb_api.py' and made some adjustments to the original bcb_api.py file.

### R1D5 - 2019/05/18
Almost finishing the unittest file for the FinancialIndicesApi class, two methods left.

### R1D6 - 2019/05/19
Kept working on my unittest file for the FinancialIndicesApi. I'm struggling a lot on design decisions.

### R1D7 - 2019/05/21
Still working on my tests for the FinancialIndicesApi class. Made a lot of changes to the behaviour of the _create_api_url() method.
I'm also re-thinking about the design decisions made for the _remove_wrong_records() method.

### R1D8 - 2019/05/22
Worked mostly on changes to the bcb_api script, to make it pass the new unittests.

### R1D9 - 2019/05/23
Technically finished the unittest classes for the bcb_api script.

### R1D10 - 2019/05/25
Decided to chill a little bit (tired of tests) and just did a couple of codewars and hackerrank exercises.

### R1D11 - 2019/05/26
Just did a few exercises from both hackerrank and codewars.

### R1D12 - 2019/05/27
Today I did a lot. Did a couple of codewars exercises, finished updating the bcb_api.py, to make the class
FinancialIndicesApi pass all unittests. Started making the unittest script for the IndicesExpander class.

### R1D13 - 2019/05/28
Did a couple of [HackerRank](https://www.hackerrank.com/rafael_ftrad) exercises and started making unittest script for
the IndicesExpander class. Basically finished tests for the field _workdays and private method _daily_workdays_indices_expander().

### R1D14 - 2019/05/29
* Did one codewars exercise.
* Wrote a few more test cases for the class TestDailyWorkdayIndicesExpander.
* Created and wrote tests for the TestDailyThreeFieldIndicesExpander class.

### R1D15 - 2019/05/30
* Added support from year 2001 to 2011 to the workdays.csv file from [financial_indices project](https://github.com/RafaFT/financial_indices).
* New file means broken classes and tests. Also updated the unittest test cases there were broken by the extra workdays.

### R1D16 - 2019/06/03
* Studied logging a lot, both for work and personal project.
* Today I did a somewhat model class of a QTreewidget with PyQt, and is just occurred me that it might be fun to create and share some PyQt recipes.
* Did a few more tests for the financial indices project, and started coding the IndicesExpander class.

### R1D17 - 2019/06/04
* Spent a lot of time trying to understand how a specific financial indices dates (cod 226) actually works..
understanding it is necessary as I'll need to create my own dates, following their rules, and also because I needed to
write unittests examples.
* Wrote the unittests based on the knowledge of the dates rules (method _get_next_days()).
* Continued to write the IndicesExpander class.

 ### R1D18 - 2019/06/05
* Fixed and created a couple of more tests for the classes TestGetNextDays and TestDailyThreeFieldIndicesExpander.
* Actually wrote the methods of those tests, the _get_next_days() and _daily_three_field_indices_expander().

### R1D19 - 2019/06/06
* Worked a lot on the new method _ipca_from_15_indices_expander of the IndicesExpander class. It was a little harder than
I anticipated, and I'm not entirely satisfied with my solution... I might come back to this method again later.

### R1D20 - 2019/06/07
* Did not write code. I was stuck studying and comparing the json and configparser libraries. One or maybe even both might
end up being used in the [financial_indices project](https://github.com/RafaFT/financial_indices).

### R1D21 - 2019/06/09
* Did not write code. Instead, I spent most of the time reflecting on how to organize the [financial_indices project](https://github.com/RafaFT/financial_indices)
as a class UML. I actually created a prototype that I intend to add to github if possible (as a pdf maybe). The software
I used to create the UML was [Ludichart](https://www.lucidchart.com/pages/landing?utm_source=google&utm_medium=cpc&utm_campaign=en_bucket_desktop_branded_x_bmm_lucidchart&km_CPC_CampaignId=1484560204&km_CPC_AdGroupID=60168105831&km_CPC_Keyword=%2Blucidchart&km_CPC_MatchType=b&km_CPC_ExtensionID=&km_CPC_Network=g&km_CPC_TargetID=aud-536921399221:kwd-308734946772&km_CPC_Country=1001773&km_CPC_Device=c&mkwid=s&slid=&pgrid=60168105831&ptaid=aud-536921399221:kwd-308734946772&gclid=Cj0KCQjwov3nBRDFARIsANgsdoEBkYbj3gKFN5_D21s3HJgq5VQAby_IJmcuEcUhyGO58fsrJ9h7AP8aAo5KEALw_wcB),
which seems awesome!

### R1D22 - 2019/06/10
* Wrote tests for the class IndicesWorkbook.
* Started creating the class IndicesWorkbook on a new script.

### R1D23 - 2019/06/11
* Continued to work on how to organize my [financial_indices project](https://github.com/RafaFT/financial_indices) as a
class UML diagram, and it looks awesome! I definitely don't feel ashamed of adding it to my github, I just I'm not sure how
to properly do it yet.
* Based on my UML, I actually wrote the skeleton of a few more classes (WorksheetWriter classes).
* Finished most of the IndicesWorkbook class.

### R1D24 - 2019/06/12
* Started writing the content of the Worksheet writer classes. Created the metaclass WorksheetWriter, and it's children,
CdiWriter, SelicWriter, IpcaWriter and TrWriter.

### R1D25 - 2019/06/13
* Actually finished all of the worksheet writer classes. The program is almost finished.

### R1D26 - 2019/06/15
* Added a new class, MetadataWriter, which is going to add support to updating an existing workbook.


### R1D27 - 2019/06/16
* Add most of the functionality to the MetadataWriter class.
* I can see three things missing on the program so far, besides the main function.
1. Logging support.
2. Worksheet protection by password.
3. Maybe an initial prompt (probably using pyqt), for the user to decide which initial date to consider for each indices.

### R1D28 - 2019/06/17
* Created a board on Trello to help manage my [financial indices project](https://github.com/RafaFT/financial_indices).
* Installed and played a little with the magic the gathering sdk in Python. I'm consider creating a site or web application
around my favourite and long lasting card game.
* Continued the course [web programming with python and js](https://www.edx.org/course/cs50s-web-programming-with-python-and-javascript)
and created a simple web page. trying to become more familiar with both html and css.

### R1D29 - 2019/06/18
* Solved a few exercises from [HackerRank](https://www.hackerrank.com/rafael_ftrad).

### R1D30 - 2019/06/20
* Added protection functionality to all worksheets from my
[financial indices project](https://github.com/RafaFT/financial_indices).

### R1D31 - 2019/06/21
* Did a little of everything; Solved a couple of [HackerRank](https://www.hackerrank.com/rafael_ftrad) exercises,
(informally) fixed a bug on [financial indices project](https://github.com/RafaFT/financial_indices), which was extending
ipca values incorrectly and played a little with the MTG API.

### R1D32 - 2019/06/22
* Did a couple of exercises and flirted with magic a little more.

### R1D33 - 2019/06/23
* Worked a lot on the MTG idea. Got to understand a little more how all the data is structered, which helps me decide how
I'll actually organize a relational database for it.
* Worked on [financial indices project](https://github.com/RafaFT/financial_indices). I've started thinking about logging
and how I'll organize it (should i actually write on file?)
* I'm seriously considering adding some minor user interface to the project, just so he user can pick the start date for
each indices (for the first time the program is executed), and whick indices will be tracked.

### R1D34 - 2019/06/24
* Created decorator to log the time it takes for a funtion to execute.
* Added loggings to both bcb_api and indices_expander files.
* Created a couple of new helper functions on indices_expander.py.

### R1D35 - 2019/06/25
* Created tests for the new methods from IndicesExpander.
* Added a bunch of logging to all scripts.
* Realized that the program works, but it's consuming around 4 gb of RAM!!!!! Trying to understand why, and how I can
change that.

### R1D36 - 2019/06/26
* Studied URL's, HTTP requests and responses from udacity's course [Intro to Backend](https://classroom.udacity.com/courses/ud171).
* Studied flask from [this tutorial](https://www.youtube.com/watch?v=MwZwr5Tvyxo).

### R1D37 - 2019/07/13
* YES, I did code everyday, but completely forgot to log it in... =(
* Today I studied the concept of CSS grid layout
* Made a small mock up for my personal website

### R1D38 - 2019/07/14
* Read an [article](https://hackernoon.com/how-css-grid-beats-bootstrap-85d5881cf163?source=bookmarks---------2-----------------------) that basically sold me the benefits of css grid layout and linked me to an interesting course on the subject
* Took the small but very interesting course on css grid layout [here](https://scrimba.com/g/gR8PTE)
* Wrote a HTML version of my personal website. Will post it to [github](https://github.com/RafaFT/RafaFT.github.io) once I finish the css

### R1D39 - 2019/07/15
* Wrote the CSS file for my index.html page
* Add/experimented with media query
* The simple site is already up [here](https://rafaft.github.io)

### R1D40 - 2019/07/16
* Updated my [personal web site](https://trello.com/b/1yXGT3dd/personal-website-1) and [financial indices](https://trello.com/b/xtCrULsg/financialindices) Trello's cards
* Identified and fixed an issue in the financial_indices project, where by running the program from an executable file, an uncaught exception would break the program without logging, making it very hard to debug. The solution I found is to override the function sys.excepthook, which is called right before exiting the program when an error uncaught is raised.

### R1D41 - 2019/07/20
* Again, I did code everyday, but unfortunately I did not logged it in
* Today I continued my study of HTML CSS and JS from mozilla's tutorial, I stopped [here](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/JavaScript_basics)
* For now, I added my [calculator website](https://rafaft.github.io/financial/) on [github](https://github.com/RafaFT/financial), in order to see it on my phone easily, by using github pages
* Today I played a little bit with JS, and was able to implement dynamic behavior to two columns, where one disappears and the other extends when necessary

### R1D42 - 2019/07/21
* Solved one exercise from [HackerRank](https://www.hackerrank.com/rafael_ftrad?hr_r=1)
* Fixed a bug from my mockup calculator [webpage](https://rafaft.github.io/financial/), where the js was failing to toggle on a column when the reset button was pressed
* Kept studying MDN's tutorial on web development

### R1D43 - 2019/07/22
* Continued to study MDN's tutorial
* Worked a lot on my [website](https://rafaft.github.io/), fixing some [issues](https://trello.com/b/1yXGT3dd/personal-website-1) and mostly simplifying underline code

### R1D44 - 2019/07/23
* Completed the module's 2 (CSS 3) exercise from coursera's course on this [github pages](https://rafaft.github.io/html-css-and-javascript-for-web-developers/)

### R1D45 - 2019/07/24
* Studied a little bit of forms from MDN's tutoril
* Graded the module's 2 exercise from coursera's course, and found a bug on my media query by testing other people's code

### R1D46 - 2019/07/25
* Continued my study on [MDN's tutorial](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout)
* Updated my [website](https://rafaft.github.io/) so that when the header nav bar doesn't fit in just one column, instead of breaking into another row, a horizontal scroll bar appears

### R1D47 - 2019/07/27
* Took some time to actually read and learn a little bit about VSCode's configuration system. Learned how to set up some manual settings for specific languages... settings.json is awesome and the default setting's json file helps a lot too!
* Studied [pyinstaller's documentation](https://pyinstaller.readthedocs.io/en/stable/usage.html). I learned how to separately create a spec file and how to add data files to the executable. Also figured it out how to make the 'app' correctly aware of it's location during runtime (python interpreter could simply use __file__).

### R1d48 - 2019/07/28
* Worked on my [financial indices project](https://github.com/RafaFT/financial_indices) and uploaded it to github. The project is functional and ready to use. But it doesn't have any relevant docs on github yet, and I'm not sure if and how an executable should be uplodad as well. I'll get into that.
* Started writing some ideas for my new project, 'calculadoras.app' on Trello. Also thought about a new feature for the financial indices project, which should be fairly easy to implement.

### R1D49 - 2019/07/29
* Moved on a little bit from HTML and CSS and started learning some real JavaScript! The language is not as bad (so far) as many people over the internet imply it to be, but it is a little weird. The difference (existence actually) between the two equality operators, regular (==) and strict (===) is simply bizarre! The lack of an error when calling a funtion without it's argument is also awkward.
* Did a couple of [HackerRank](https://www.hackerrank.com/dashboard) Javascript exercises.

### R1D50 - 2019/07/30
* Did a couple of exercises from codewars (only python =()
* Minor fix to my website.
* Minor fixes to the unittest files from the [financial-indices project](https://github.com/RafaFT/financial_indices).

### R1D51 - 2019/07/31

* Today I took a break on studying JS and played a little with GCP
* I used both GC Console and the shell that comes with the SDK to create and delete some projects
* Tomorrow I intend on finishing the tutorial 'Building an app' from the [Python 3 Standard Environment](https://cloud.google.com/appengine/docs/standard/python3/) docs.

### R1D52 - 2019/08/05

* Did a couple of JavaScript exercises from codewars
* Resumed my studies of relational databases from [udacity's course](https://www.udacity.com/course/intro-to-relational-databases--ud197)

### R1D53 - 2019/08/06

* Finally answer a question on [Stack Overflow](https://stackoverflow.com/questions/57384648/what-is-printnot-in-python/57385554#57385554)
* Continued the Udacity's course on [Relational Databases](https://www.udacity.com/course/intro-to-relational-databases--ud197)

### R1D54 - 2019/08/08

* Studied more SQL from both [Udacity's course](https://www.udacity.com/course/intro-to-relational-databases--ud197) and [Corey Scafer's youtube tutorial](https://www.youtube.com/watch?v=pd-0G0MigUA)
* Worked a little bit on the calculator project and actually wrote a few tests using sqlite3 lib. A database with all 13 indices from since the beginning of each up to today, has 48060 records.

### R1D55 - 2019/08/09

* Continued my studies of Udacity's course and it's almost done
* Started writing a script for the calculators project

### R1D56 - 2019/08/10

* Worked on the calculator project and got a few tables already running, with PK, FK and adequate reference integrity
* The most important table, IndicesRecord, which holds all of the indices records values and dates is already up and running

### R1D57 - 2019/08/11

* Worked on the calculator project and finished the init_db.py script, which is responsible for creating and populating the database for the first time

### R1D58 - 2019/08/12

* Today I studied a lot! Listened [Harvard's SQL](https://www.youtube.com/watch?v=Eda-NmcE5mQ) talk from the [Web programming with Python and Javascript course](https://www.edx.org/course/cs50s-web-programming-with-python-and-javascript)
* Continued to work on the calculators project. Today I changed a json configuration file for a singleton dict like class
* Created utils.py and also update_db.py, which is responsible for updating the tables created by the init_db.py file

### R1D59 - 2019/08/13

* Kept working on the calculators project. Decoupled the creation and initial population of the database tables. Now, the _init_db.py_ is responsible for only the creation of the tables, and the _update_db.py_ can be used to both populate the initial values of the tables, as well as update the values of an existing one.

### R1D60 - 2019/08/14

* Took a little bit more of the [Udacity's course](https://www.udacity.com/course/intro-to-relational-databases--ud197) and I'am almost done
* Continued working on the Calculator's project. Added definition to create the new table 'IndicesValues' and finished the function to update the table 'IndicesRecords', by using the class 'BcbApi', which is the renamed class 'FinancialIndicesApi' from the script _bcb_api.py_ of my [Financial-Indices project](https://github.com/RafaFT/financial_indices)

### R1D61 - 2019/08/16

* Created the table Workdays, IndicesValues and IndicesValuesMeta on the Calculator's project

### R1D62 - 2019/08/17

* Worked a lot on the functions necessaries to update/add values for the new tables, IndicesValues and IndicesValuesMeta
* Created decorator 'connection_handler', which handles the creation of the connection and cursor of sqlite3, as well as the logic for when to commit, rollback and close

### R1D63 - 2019/08/18

* Created adapters and converters to the sqlite3 for both date and decimal types. The adapters are encapsulated inside the new decorator for SQL connections. The catch is that for the decorated function to have access to the cursor (or connection), it must be passed as an argument, which makes all decorated functions to have a extra parameter
* Finished the overall logic of the update functions for the tables IndicesRecords, IndicesValues and IndicesValuesMeta
* Started writing the class responsible for expanding records values from IndicesRecords for the IndicesValues table

### R1D64 - 2019/08/19

* Fixed minor bugs from update_db.py
* Refactor indices_expander.py and IndicesExpander to indices_values.py and IndicesValuesHelper, since the idea of this class and script is now for converting original indices records from the api, into values appropriated for calculation
* Added more functionality to the new class IndicesValuesHelper

### R1D65 - 2019/08/20

* Finished the [Intro to Relational Databases](https://classroom.udacity.com/courses/ud197) course
* Basically finished the first feature of my calculators app, which is the capability of storing and updating financial indices information, which is necessary for performing some kinds of calculations. I'll have to visit this topic again, once I start to prepare for deployment, and have to change the RDBMS from sqlite3 to MySQL, which is what GCP supports
* The second feature of my app is an API that actually performs the calculations mentioned in the item above (update a value by the CDI, for example). Since this API will most likely respond to an HTTP request, I decided to learn more about the topic and started taking another [course on Udacity](https://classroom.udacity.com/courses/ud897) and reading about it on [MDN's website](https://developer.mozilla.org/en-US/docs/Web/HTTP)

### R1D66 - 2019/08/21

* Continued my study of HTTP from [MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP) and [Udacity's](https://classroom.udacity.com/courses/ud897) course
* Updated the [Trello](https://trello.com/b/xtCrULsg/financialindices) for the [financial-indices](https://github.com/RafaFT/financial_indices) project
* Removed the use the extra 1300 columns from the CDI worksheet
* Replaced float values with decimal.Decimal values

### R1D67 - 2019/08/22

* Started taking [GCP Essentials Quest](https://google.qwiklabs.com/quests/23), which is the most basic one available. I intend to complete one QwikLabs a day
* Did a few [codewars exercises](https://www.codewars.com/users/RafaFT)
* Studied a little bit more of [HTTP and client-server communication](https://www.udacity.com/course/client-server-communication--ud897)
* Add a tag to my financial-indicators project (the name 'indices' was incorret) and a [release on github](https://github.com/RafaFT/financial-indicators/releases), with the executable

### R1D68 - 2019/08/23

* Finished two more courses from [GCP Essentials Quest](https://google.qwiklabs.com/quests/23), [Creating a Virtual Machine](https://www.qwiklabs.com/focuses/3563?parent=catalog), and [Getting Started with Cloud Shell](https://www.qwiklabs.com/focuses/563?parent=catalog)

### R1D69 - 2019/08/24

* Did a major refactor on the financial-indicators (old financial-indices) project, because, it turned out the translation of 'indicador' is not 'indices', but 'indicators'

### R1D70 - 2019/08/25

* Completed the [GCP Essentials quest](https://www.qwiklabs.com/quests/23) and earned my badge! It's already on LinkedIn xD
* Completed the first course (on storage) from my new GCP quest, [Baseline: Infrastructure](https://www.qwiklabs.com/quests/33), which seems awesome!

### R1D71 - 2019/08/26

* Studied a lot about [firestore](https://cloud.google.com/firestore/docs/) and the differences between Relational Databases and NoSQL ones
* Learned some basic concepts of firestore, like collections, documents, fields and some concepts from Python's client Library

### R1D72 - 2019/08/27

* Continued to study firestore concepts and how to interact with it. Found an amazing [youtube channel](https://www.youtube.com/playlist?list=PLl-K7zZEsYLluG5MCVEzXAQ7ACZBCuZgZ) from google that gives great explanation
* Started creating the same create/update functionality of financial indices database from the _R1D63_, but this time using the firestore database

### R1D73 - 2019/08/31

* Worked a LOT on the calculators project, creating a cron job for updating the firestore database with IndicatorRecord's, fixing .gcloudignore and requirements.txt, and a very basic main.py file.

### R1D74 - 2019/09/02

* Did a major refactor and organization on the script responsible for updating the firestore database. Split the main function into smaller procedures and I'm basically preparing the script for handling the indicator values that will actually be used and query for calculation

### R1D75 - 2019/09/03

* Still working on the calculators project... Today I reorganized yet again the functions from the update_indicators script.
* Started writing the function that will update the CalculatorIndicator collection from Firebase. There will be some complicated steps in the future, but for now, the retrieving of documents necessary for the process to begin is almost finished

### R1D76 - 2019/09/04

* Wrote a few docs for the calculators project
* Started writing the new adjust_calc_docs() function, which is responsible for preparing the documents to be inserted into the CalculationIndicator DB, filling all fields and adjusting the dates from monthly indicators

### R1D77 - 2019/09/05

* Created the Workdays collection DB on Firestore
* Re-imagined how to solve the organization and structure of the documents on the CalculationIndicator collection

### R1D78 - 2019/09/09

* I actually did code everyday.. Just forgot to log it in
* Today I started connecting the dots of the croon job to update the collections BcbIndicator and CalculationIndicator

### R1D79 - 2019/09/11

* Finished the logic to update the firestore collections BcbIndicator and CalculationIndicator, both on a cron job

### R1D80 - 2019/09/16

* Started studying JavaScript again, from the [HTML, CSS and JavaScript for Web Developers](https://www.coursera.org/learn/html-css-javascript-for-web-developers/home/welcome) course

### R1D81 - 2019/09/17

* Continued my study of JavaScript. Considering starting another [course from Udacity](https://www.udacity.com/course/intro-to-javascript--ud803) which are usually more project driven
* Took another look at my calculators page mock up. Started fixing some issues and will use it for a actual mock up for the 'official' calculators app

### R1D82 - 2019/09/18

* I wanted to add some logic in the main.py file of the [calculators project](http://calculators-app.appspot.com/) that would log in every time it was executed in a firestore collection. But that backfired because of a `ServiceUnavailable: 503 Deadline Exceeded` error from the GCP that I could not resolve in 40 minutes...
* Started a new [JavaScript course](https://www.udacity.com/course/intro-to-javascript--ud803) that will hopefully keep me interested
* I intend to keep studying **javascript for front end** to help me get some things done, and **HTTP for the back end**, to understand more how things work and prepare myself for API's

### R1D83 - 2019/09/19

* Took the Python questions from the Revelo website, apparently I passed
* Did a couple of [HackerRank exercises](https://www.hackerrank.com/rafael_ftrad)
* Finished the _Data Types & Variables_ section from the [JavaScript course](https://classroom.udacity.com/courses/ud803) from Udacity
* Actually learned a couple new things of JS, such as how to do string formatting like in Python, to avoid doing string concatenations all the time (using `` instead of quotes, and the ${variable_name} syntax)

### R1D84 - 2019/09/20

* Did a couple of exercises from [HackerRank](https://www.hackerrank.com/rafael_ftrad)
* Continued the [Intro to JavaScript course](https://classroom.udacity.com/courses/ud803)

### R1D85 - 2019/09/21

* Did a couple of HackerRank and LeetCode exercises
* Studied a bit of JavaScript conditionals (going to finish a module a day from the course, during the week)

### R1D86 - 2019/09/22

* Today I visited my grandfather in the hospital, so I did not have access to my laptop, but I read the first two chapters from the [Grokking Algorithms](https://www.amazon.com.br/s?k=grokking+algorithms). Re-implemented a binary search algorithm and learned the concepts (and differences) of array and linked list

### R1D87 - 2019/09/23

* Tested a few things on GCP. I discovered that I don't actually need to access a service account credentials key once the code is being executed on the app engine. This basically means I only have to use the credentials key when running the backend locally. I'll probably change my code accordantly and just grab the firestore client in the main script for production, and for local tests I will set the path to the key on the environment variable GOOGLE_APPLICATION_CREDENTIALS
* Did a couple of exercises

### R1D88 - 2019/09/24

* Finished the _Conditionals_ section from the [JavaScript course](https://classroom.udacity.com/courses/ud803) from Udacity
* Learned about JavaScript's _Ternary Operator_ (Python's if much better =)) and the _Switch Statement_
* Did a couple of [HackerRank](https://www.hackerrank.com/rafael_ftrad)

### R1D89 - 2019/09/25

* Did a couple of exercises from codewars and hackerrank on JS, for practice
* Almost finished the _Loops_ section from the [JavaScript course](https://classroom.udacity.com/courses/ud803)

### R1D90 - 2019/09/26

* Finished the _Loops_ section from the [JavaScript course](https://classroom.udacity.com/courses/ud803) from Udacity and started taking the _Functions_ module

### R1D91 - 2019/09/28

* Finished the _Functions_ section from the [JavaScript course](https://classroom.udacity.com/courses/ud803) from Udacity
* it was very interesting and useful to learn about the concept of _hoisting_, which is a feature in JS where all functions and variables definition are executed before the actual JS script is executed. This means that if a function is defined after it has already been called, it would be valid code, since JS engine executes the definitions before

### R1D92 - 2019/10/02

* Since gcp services (such as app engine) don't need credentials to access other resources (such as firestore), once the application as been deployed, I removed and refactored all code that was trying to access the firestore client by using a credentials file
* Started refactoring the whole project as microservices

### R1D93 - 2019/10/03

* Refactored the whole project to be microservice compliant, very similar to the structure described by [google docs](https://cloud.google.com/appengine/docs/standard/python/microservices-on-app-engine)
* Finished the api for the Workdays firestore collection, with support for all workdays, or for all workdays given a year and month

### R1D94 - 2019/10/08

* Started Working on the [luiza labs challenge](https://trello.com/b/tysBalCL/luizalabs-desafio)
* Followed a LOT of GCP instructions and was able to actually deploy an App Engine service with access to a Cloud SQL MySQL instance

### R1D95 - 2019/10/10

* Finished the DB modeling and created the schema for tables 'Client' and 'FavoriteProduct'
* Kept learning and exploring MySQL on cloud SQL
* Struggling to use SqlAlchemy purely for the ORM features, and not for defining and creating tables (doc implies these are two different features)

### R1D96 - 2019/10/27

* Back from vacation
* Did a few [LeetCode](https://leetcode.com/user9407y/) exercises

### R1D97 - 2019/10/28

* Started transitioning from URL parameters to Query Parameters on the workdays api (for filtering by year and/or month)
* Add new indexes to the Workdays firestore DB

### R1D98 - 2019/10/29

* Created the methods necessary for any querying combinations for the workdays database (year, month, both and with order)
* The (future) new bcb-api-service now access the workdays api by an environment varible defined in the app.yaml file, instead of a global variable defined directly on the file

### R1D99 - 2019/10/31

* Removed the "extra" functions necessary to query the workdays collection (by year, month, or both), and now use only one function for the job. The function create and updates the query by the query parameters given. This is "cleaner" than using 3 or 4 differente functions.
* Removed the order parameter, since it's overhead and overkill... I don't need this option
* Request to insert of a new workday now must originte from the same google cloud project (id) as the workdays api

### R1D99.1 - 2019/11/1

* Did not code today. I did however, work on the worksheet formulas from my investment portfolio. Finally managed to have my CDI and Selic functions use the new workbook (generated by the executable from the [financial-indicators](https://github.com/RafaFT/financial-indicators)).

### R1D99.2 - 2019/11/2

* Did not code today (again), but I spent a couple of hours just fixing a bunch of other functions and references on my investment portfolio workbook.

### R1D100 - 2019/11/3

* Worked on the bcb-indicators api. The view function for GET now dynamically creates the firestore query (with support query parameters for both start and end dates, as well as ordering).
* Fixed and change the behaviour of some functions from util.py.
