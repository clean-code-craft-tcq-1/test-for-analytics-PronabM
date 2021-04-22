# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

Fill the parts marked '_enter' in the **Tasks** section below.

## Analysis-functionality to be tested

This section lists the Analysis for which _tests_ must be written.

Battery Tele-metrics are collected and stored on a server.
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

1. Read Access to the Server containing the tele-metrics in a csv file.
1. Third party Data visualization library to include plots/charts in the report. *[OPTIONAL]*
1. Third party PDF converter library for PDF generation.
1. Write Access to the Server to store the PDF report.
1. Notification medium is not specified, but assuming it would be by mail access to SMTP server is required. 

### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | We do not test the accuracy of data.
Computation of maximum      | Yes           | This is part of the software being developed.
Off-the-shelf PDF converter | No | Since off-the-shelf it is expected that the converter has already passed it's unit-tests. 
Counting the breaches       | Yes | This is part of the software being developed.
Detecting trends            | Yes | This is part of the software being developed.
Notification utility        | No | Since off-the-shelf it is expected that the converter has already passed it's unit-tests. 

### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests:

1. Write "Data Source Inaccessible" and mention http response code to the PDF when csv file is not reachable (unauthorized, not found and so on).
1. Write "Data Source Empty" when csv contains no data.
1. Write "Invalid input" to the PDF when the csv doesn't contain expected data.
1. Write all occurrences of minimum and maximum to the PDF from a csv containing positive and negative readings.
1. Write the number of breaches to the PDF from the csv containing positive and negative readings.
1. Write "No Breach recorded" to the PDF from the csv containing positive and negative readings when no breach is recorded.
1. Write/Visualize all occurrences of Trends (reading was continuously increasing for 30 minutes) to the PDF from the csv containing positive and negative readings.
1. Write "No Trend Recorded" to the PDF from the csv containing positive and negative readings when no occurrence is recorded.
1. Write PDF to the designated place on the server from the raw data after all computations are complete.
1. Notify relevant users the URL of the new PDF when PDF is successfully written on the server.
1. Notify relevant users with the http response code and additionally the raw data when server is inaccessible. 

### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality            | Input        | Output                      | Faked/mocked part
|--------------------------|--------------|-----------------------------|---
Read input from server     | csv file     | internal data-structure     | Fake the server store
Validate input             | csv data     | valid / invalid             | None - it's a pure function
Notify report availability | Report path (null if unavailable) | invoke notification utility if path is not null  | Fake the notification utility
Report inaccessible server | Server Path | HTTP Response Code eg. 200,401,404            | Fake the HTTP client
Find minimum and maximum   | csv data | indices of minimum and maximum               | None - it's a pure function
Detect trend               | csv data | indices where trend conditions were met               | None - it's a pure function
Write to PDF               | Raw data | PDF File               | Fake the PDF Converter