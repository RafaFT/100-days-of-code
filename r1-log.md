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

