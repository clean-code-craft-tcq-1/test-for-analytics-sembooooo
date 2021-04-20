# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

Fill the parts marked '_enter' in the **Tasks** section below.

## Analysis-functionality to be tested

This section lists the Analysis for which _tests_ must be written.

Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

Analysis must be done on this csv file to find the following:
- minimum
- maximum
- count of breaches (how many times did it cross the threshold in a month?)
- record trends (date & time when the reading was continuously increasing for 30 minutes)

A PDF report of the analysis must be stored every week.
Notification must be sent when a new report is available.

## Tasks

### List Dependencies

List the dependencies of the Analysis-functionality.

1. __Access to the Server containing the telemetrics in a csv file__  
1. __Accessing Notification medium.__    
    _Remark: As the way of notifying is not mentioned : let us assume it to be a mail client._  
           _Accessing a mail client(SMTP server)_
1. __Accessing the Server to store the pdf file.__    
    _Remark: Before it was reading a file from server now it is writing a file into the server._
   _As they are two different aspects of accessing i wanted to mention them explicitly._
1. __PDF creation.__    
    _Remark: Mostly we use third party tools to create PDFs. Hence listed under dependency._
1. __Recording Trends in a beautiful fashion.__    
    _Remark: Not really sure how trend recording is going to be depicted._  
    _If third party libraries are used to generate pictures or graphs for easy visualisation_    
    _in the PDFs then this could be a dependency_
1. __Accessing Battery temparature values.__  
    _Remark: Assuming our telemetrics is about the Battery parameter __Temparature__ ._  
    _Temparature values are read using ADC. Normally one unit reads the temp sensor ADC signals_  
    _and gives out the processed results to other units. So one or the other unit should supply_  
    _data of battery temparature to our analysis function._  
    _Hence accessing Battery temparature is also a dependency for the Analysis function_  
 

### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | We do not test the accuracy of data
Computation of maximum      | Yes           | This is part of the software being developed
Off-the-shelf PDF converter | _enter Yes/No | _enter reasoning
Counting the breaches       | _enter Yes/No | _enter reasoning
Detecting trends            | _enter Yes/No | _enter reasoning
Notification utility        | _enter Yes/No | _enter reasoning

### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests:

1. Write minimum and maximum to the PDF from a csv containing positive and negative readings
1. Write "Invalid input" to the PDF when the csv doesn't contain expected data
1. _enter a test
1. _enter a test

(add more)

### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality            | Input        | Output                      | Faked/mocked part
|--------------------------|--------------|-----------------------------|---
Read input from server     | csv file     | internal data-structure     | Fake the server store
Validate input             | csv data     | valid / invalid             | None - it's a pure function
Notify report availability | _enter input | _enter output               | _enter fake or mock
Report inaccessible server | _enter input | _enter output               | _enter fake or mock
Find minimum and maximum   | _enter input | _enter output               | _enter fake or mock
Detect trend               | _enter input | _enter output               | _enter fake or mock
Write to PDF               | _enter input | _enter output               | _enter fake or mock
