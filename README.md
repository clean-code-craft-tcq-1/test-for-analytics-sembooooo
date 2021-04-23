# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

Fill the parts marked '_ enter' in the **Tasks** section below.

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


### doubt in the above requirements
1. CSV file is generated and stored every month but PDF is stored every week.  
Moreover PDF is generated every week after analysing CSV file that is only generated over a month.  
Is that a typo over there ? or is that voluntarily written like that ? or Am i missing something ?  
1. In which system the Analysis function is present ? The requirements sounds to me that analysis function is   present not in the bms system/ server but some where else as we are reading the CSV stored in the server.  
So is this a system of systems ?  
or is it present in the BMS system itself ?   
I am assuming it to be in a third system so it has to access server to get the CSV file (which is expensive though).  
(assumption based on the list of dependencies first point given by examiner)  
1. Where should the PDF be stored ? Hence i am assuming that it is stored in the server where CSV is stored.  

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
Off-the-shelf PDF converter | No            | Third party libraries or APIs. We shall test number of calls to it , inputs and outputs.
Counting the breaches       | Yes           | This is part of the software being developed
Detecting trends            | Yes           | This is part of the software being developed
Notification utility        | No            | Third party libraries or APIs. We shall test number of calls to it , inputs and outputs.

### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests:

1. Write minimum and maximum to the PDF from a csv containing positive and negative readings.
1. Write "Invalid input" to the PDF when the csv doesn't contain expected data.
   _Remark "Contain expected data" :By this i am assuming this to a requirment which was given to us during the assignments._    
   _If data crosses beyond certain limit then it is not trustable._  
1. Write/Draw a TREND (increasing of values for 30mins contiously) to the PDF from a csv containing positive and negative readings.
1. Write "No increasing trend in values " to the PDF when the csv doesnt contain any trends.
1. Write the number of breaches to the PDF from a csv containing positive and negative readings.
1. Write "No breaches found" to the PDF when the csv doesnt contain any breaches.
1. Verify if PDF is stored on the server location after analysis.
1. Verify if the appropriate user is notified with appropriate message if the server to store CSV/PDF is not accessible for reading.
1. Verify if the appropriate user is notified with appropriate message if the server to store CSV/PDF is not accessible for writing.
1. Verify if the notification is sent after every new PDF generation.
1. Verify if the user is notified with appropriate message if the Notification medium is not being sent.  
1. Verify if the data is written to PDF in the "defined" Format.  
    _Remark: My intention here is not to verify the third party converters but the way we are using the third party PDF_    
    _generators_
1. Verify if only one month telemetrics is stored in CSV.
1. Verify if battery telemetrics is stored in proper format in CSV.
   By format i mean, every sample of the data should be having date and time stamp for analysis function.  
   As the CSV is now an interface, the data should be depicted in a predefined (agreed) format. 
1. Verify if new PDF is stored every week.  
 _(still the requirement is ambigious to me. Please find the remark in ```doubt in the above requirements``` section)_

### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

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

Output: Once the return value of the API is read then it is upto diagnostic function, another function which handles  
all the exceptions ,to decide how the output should be conveyed to the user (whether it has to print on console as an error or glow an led on the pannel)   
or display an error code in LCD display). Hence mentioned ```return value from the API``` as output.

__Write to PDF:__ As we have a fake server store created already i would really prefer to have a fake pdf file generated on the server.
