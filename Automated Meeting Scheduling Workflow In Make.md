## Objective

To create an automated workflow using Make.com that processes meeting confirmations received via email and converts them into Google Calendar events with specific details.

## General Task Flow

![image](https://github.com/user-attachments/assets/0fc308ba-bd4c-4b84-a131-d4d0c6616daa)

## Overview
The task involves creating an automated workflow in Make.com to process meeting confirmation emails, extract relevant details using a text parser, and create Google Calendar events. The workflow begins with receiving and filtering emails based on specific criteria, parsing the email content to extract fields like date, time, duration, and contact details using test parser, and then using the Google Calendar module to schedule events with a structured name, description, and attendees. The document includes the configured Make.com scenario, and documentation of the approach.

**Step1:** 
### Making An Account On <https://www.make.com/> 
using the new email and credentials from above.

**Step2:** 
### Creating A Make Scenario 
that gets triggered when an email is sent

![image](https://github.com/user-attachments/assets/3cd68c09-cccd-4ca3-abb8-638d94943964)


**Step 3:** Setting Up The Email Module

Move to the scenario tab and select the email module.

![image](https://github.com/user-attachments/assets/95f33e52-5c4e-4079-b287-15ec0c2d24de)

![image](https://github.com/user-attachments/assets/6378e8c7-1c44-40d0-a0fd-a91f4f0eff90)

Next set the information as follows:

![image](https://github.com/user-attachments/assets/8e85c2c4-1e95-4f77-9913-268d2a6f24f3)

To trigger event, the receiver of email must be the one specified in query.

Gmail query used: to:----@gmail.com "Your appointment has been scheduled" ("Contact Name:" OR "Date and Time:")

The emails will not be received until we create the connection between make and gmail. This is done by using gmail API. To enable the API follow the following steps:

Visit the google cloud console and create a new project

![image](https://github.com/user-attachments/assets/14c94f00-e39a-4159-9cfe-09009d3daefa)

![image](https://github.com/user-attachments/assets/de696db2-6d8a-4475-ae6f-647745cd1f64)

Once created move to APIs & Services/API Library. Search for gmail API and then enable it

![image](https://github.com/user-attachments/assets/08cbcce3-7576-4dbf-9199-5898a3150586)

Creating an authorization screen which will let us access gmail/project from make

![image](https://github.com/user-attachments/assets/a0de9ece-5cd6-4848-b396-e0b78f71b197)

Only allowed users will be able to access the email.

![image](https://github.com/user-attachments/assets/01e58edd-df43-43fa-ab92-72ed59fc5db3)

Since sometimes the previous name of make.com is used so, we will add both domain names

![image](https://github.com/user-attachments/assets/f7c5c6f4-c988-450b-a67a-16e3e0fe2036)

The scope will tell what the integration will be allowed to do with the gmail API which is limiting the results to gmail API

![image](https://github.com/user-attachments/assets/694eaa97-56a8-4d98-a848-9573d580a2e0)

![image](https://github.com/user-attachments/assets/5288e7b4-860f-4bb6-9dad-cadb6481b5cb)

These are some scopes needed for gmail integration with make. Select these and click update

Next is adding the test users. We can use any gmail as test user.

![image](https://github.com/user-attachments/assets/53f37769-f1fa-430a-9752-da13b7cc1355)

Next step is to create credentials to restrict unauthorized access.

![image](https://github.com/user-attachments/assets/12e8fd01-1b9f-488f-b7f3-e3c1bc90115f)

![image](https://github.com/user-attachments/assets/f8c2ea0d-3232-4070-a1e5-41baf28cbd4f)

Click create and it will generate the client ID and client secret. Copy and store them is a safe place.

![image](https://github.com/user-attachments/assets/f891cf71-b92c-4627-b98e-a7576a478f67)

Now connection with gmail can be established in make.

![image](https://github.com/user-attachments/assets/f9f12a4a-b0f7-4c05-bfa8-a8ac1305a8c7)

Then click sign in with google and use the same email account used for google cloud console.

**Step 4**: 
### Extracting Information From Email

Create regex using websites <https://regex-generator.olafneumann.org/> and <https://regex101.com/>

The test parser module will be used to parse key:value pairs from the email body.

![image](https://github.com/user-attachments/assets/b828da34-d8f4-43c7-a433-275e24141977)

Once parsed they will be passed on to an array aggregator which will combine all the output bundles into a single one and therefore can be easily used later.

![image](https://github.com/user-attachments/assets/64eefa4b-764b-4a14-b3a9-7145f38a95cc)

**Step 5**: 
### Extracting Links From Email Body

The test parser module will parse the links from the html content

![image](https://github.com/user-attachments/assets/841fb6ce-8d6f-4a30-975d-1ad254b3bf78)

Once parsed we will again use the aggregator to combine our bundles.

![image](https://github.com/user-attachments/assets/2d0e6f15-cd66-4e17-8e2b-f2652f8f2634)

**Step 6**: 
### Storing All Information Into Variables

We will use the multiple variable tool to store all the extracted data.

![image](https://github.com/user-attachments/assets/010a02f2-cd9b-44d6-afa4-8dda6754c1e6)

Since date is in string we parse it to convert it to date format that will be further used in sheets and calender.

**Step 7:** 
### Obtaining The Owner ID

We again parse the meeting link to get the owner id. This ID will be used to look up the consultants information from the google sheets.

![image](https://github.com/user-attachments/assets/fd64397f-2f06-4843-8d18-28b0dd3a8ff5)

**Step 8:** 
### Add Extracted Data Into Google Sheets

Since we have already created the following sheet “Assessment” having consultants data, we created a new sheet named as extractedData and here we are going to add extracted values

Sheet 1: consultants

![image](https://github.com/user-attachments/assets/08fb76ba-f542-49c5-af80-c2cb853a24e5)

Sheet 2: extractedData

![image](https://github.com/user-attachments/assets/e7770f53-c323-4880-9f32-ad4a6bab9ec7)

First connect the google sheet API with the same process done with gmail integration

![image](https://github.com/user-attachments/assets/333b8a10-e43c-4cb4-b596-129dcc1ee0a0)

The values that need to be added into the sheets are as follows:

![image](https://github.com/user-attachments/assets/019a6038-a03e-4365-91d0-c25175f14444)

Also add a break to handle any errors that arise due to google sheets.

![image](https://github.com/user-attachments/assets/691fd4f8-387d-46d8-a1e2-2809a3bb4741)

**Step 9:** 
### Looking Up Consultant ID

A google sheet search module helps in finding the data in a sheet. The consultant ID is used to get further information about consultant from the available data.

![image](https://github.com/user-attachments/assets/c9e5a286-e9a6-4cb5-a45a-032f33425b07)

![image](https://github.com/user-attachments/assets/95c8f6e4-8948-41af-8c16-7c7de4a6635e)

**Step 10:** 
### Generating Event In The Google Calender

Connect the google calender API with the same process done above for gmail and sheets.

Once linked add the information in the module.

![image](https://github.com/user-attachments/assets/5d6b4ec8-8b08-444f-b535-0dc1e463a30f)

![image](https://github.com/user-attachments/assets/b62575a4-9eb2-4d27-920f-250ea8144893)

![image](https://github.com/user-attachments/assets/2e8f44f1-6e5f-4cf7-908c-77e8f32dc077)

Showing calender as busy

![image](https://github.com/user-attachments/assets/9162cd1e-9ced-4617-acea-062550a6ae6a)

**Step 11:**
### Sending An Email Invite To Consultant And Customer

![image](https://github.com/user-attachments/assets/d6ab120a-3a2b-45be-a1ee-773114aaccf1)

![image](https://github.com/user-attachments/assets/287be0ff-a4ab-4558-a17c-6d22ccee9619)

## Complete Scenario

![image](https://github.com/user-attachments/assets/9ba6ea20-000b-489c-82f3-1890279b41c9)

