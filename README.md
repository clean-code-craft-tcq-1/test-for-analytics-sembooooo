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
1. __Accessing Date and Time from the system__
    _Remark: As there is a requirement to record trends (date & time when the reading was continuously increasing for 30 minutes)_  
    _For time markers we might need to access date and time from the system_

 

### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|-----------------------------------------
Battery Data-accuracy       | No            | We do not test the accuracy of data
Computation of maximum      | Yes           | This is part of the software being developed
Off-the-shelf PDF converter | No            | Third party libraries or APIs
Counting the breaches       | Yes           | This is part of the software being developed
Detecting trends            | Yes           | This is part of the software being developed
Notification utility        | No            | Third party libraries or APIs

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

#### Few points on the below table :Srikar Sana
_On the first glance seeing your comments i understood that test under discussion is not unit test._  
_But some times , according to my little experience, few things can be verified at unit test level too._  
_So I would like to give my answer with an explanation in two piece format, one for UT and one more for higher level test._  
_I am not really sure if this is even allowed. lol. I feel like i am changing question paper._  


__Test under discussion : HIGHER LEVEL TEST__

| Functionality            | Input                                      | Output                                        | Faked/mocked part
|--------------------------|--------------------------------------------|-----------------------------------------------|---
Read input from server     | csv file                                   | internal data-structure                       | Fake the server store
Validate input             | csv data                                   | valid / invalid                               | None - it's a pure function
Notify report availability | PDF file availability                      | Console(printf)/an email to dummy client      | fake is sufficient                        
Report inaccessible server | server address-port number/link to file    | return value from the API                     | fake - please refer explanation below
Find minimum and maximum   | csv data                                   | internal data-structure                       | None - it's a pure function
Detect trend               | csv data                                   | internal data-structure                       | None - it's a pure function
Write to PDF               | internal data-structure(s)                 | pdf file in fake server store/console output  | fake is sufficient   


__Notify report availability:__ A fake that sends an email to some testing email address.  
 So the testing reciever frame work can respond back to us telling that email recieved upon succesful recieving.

__Report inaccessible server:__ I could have sticked to it as mock as the output needs to be manipulated. But my second thought  
upon reading the statement made by you "Fake the server store" in the first row last column of the table.  
If a fake server store is already created in the infrastructure then we need to create a situation so it responds as inaccessible server.
But if we say that the fake server store has nothing to do with report inaccessible server api test framework then mock could have been my answer.

Output: Once the return value of the API is read then it is upto analysis function to decide how the output  
should be conveyed to the user (whether it has to print on console as an error or glow an led on the pannel  
or display an error code in LCD display). Hence mentioned ```return value from the API``` as output.

__Write to PDF:__ As we have a fake server store created already i would really prefer to have a fake pdf file generated on the server.



__Test under discussion : UNIT TEST__

If the test under discussion is Unit Test , I would say everything as a mock as we need to know the following during Unit Test
1. whether a particular API is called or not                     
1. parameters given passed to it
1. manipulation of return values
