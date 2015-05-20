JOOXTER API v1.0
===================

----------
#Overview

The Jooxter API gives developers the opportunity to interact with their Jooxter data (bookings, resources, etc.) and build third-party applications to extend the Jooxter experience.

##Getting started

The Jooxter API consists of url formatted functions, which return JSON Object, easy handled in a third-party application.

###URL
All functions call should be made to https://app.jooxter.com/

Example (For "Lookup Resource" function) :

    https://app.jooxter.com/searchresource?

> Please note that this may change with future releases of the API.

###Authorization

Jooxter ensures the safety of data access, i.e every request must contain an authorization token.
Each user has a authorization token, attached to his Jooxter profile.

To get his authorization token, a user must authentificate (See [User > Authentificate](#authentificate)).

###Response Structure

All Jooxter API functions are structured as JSON Object.

Example
```json
     { 
         "STATUS" : "SUCCESS",
         "LABEL" : "INFO",
         "ID_RES" : "1",
         "RES_NAME" : "DEMO",
         "RES_IS_BOOKABLE" : "true",
         "RES_DESC" : "DEMO Description",
         "RES_CAPACITY" : "10",
         "RES_OWNER_NAME" : "John Doe",
         "RES_BOOKABLE_FROM" : "",
         "RES_BOOKABLE_TO" : "",
         "ON_MAP" : "FALSE",
         "RESOURCE_STATUS" : "FREE",
         "HAS_NEXT_MEETING" : "FALSE",
         "GRANTED_TO_CHECK" : "FALSE",
         "PEOPLE" : [] 
         }
```
This document presents the available functions, the input parameters and the response structures.

#User

##Authentificate

###Function 

The function name, to call, is ***connect***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **api** | |true| YES |
| **email** | The User's email |(none)|YES|
| **password** | The User's password | (none) | YES |

###Output

Example
```json
    {
    	"STATUS" : "SUCCESS", 
    	"HASH" : "4d80c957f44972b8ee5a30854cef61c5d00cfe30",
    	"USER_ID" : "1", 
    	"FIRSTNAME" : "John", 
    	"LASTNAME" : "DOE", 
    	"ACCOUNT_NAME" : "DEMO ACCOUNT", 
    	"ACCOUNT_CODE" : "1", 
    	"CORPORATION_NAME" : "DEMO CORP", 
    	"CORPORATION_ID" : "1", 
    	"LOCATION_NAME" : "DEMO LOCATION", 
    	"LOCATION_ID" : "1", 
    	"TIMEZONE_DELTA" : "2", 
    	"TIMEZONE_NAME" : "Europe/Paris", 
    	"HAS_INDOOR_MAP" : "true", 
    	"PHOTO" : "", 
    	"PROFILE" : "Web Designer", 
    	"STATUS_USER" : "FREE"
    }
```
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Technical Response Status | "SUCCESS" \| "ERROR" |
| **HASH** | User Authentification Token | (String)|
| **USER_ID** | Technical User ID | (String) |
| **FIRSTNAME** | User Firstname | (String)|
| **LASTNAME** | User Lastname | (String)|
| **ACCOUNT_NAME** | The Account name which the User is linked to | (String)|
| **ACCOUNT_CODE** | Technical Account ID | (String) |
| **CORPORATION_NAME** | The Corporation name which the User is linked to | (String)|
| **CORPORATION_ID** | Technical Corporation ID | (String)|
| **LOCATION_NAME** | The Location name which the User is linked to | (String)|
| **LOCATION_ID** | Technical Location ID | (String)|
| **TIMEZONE_NAME** | The User Timezone | (String) |    	
| **TIMEZONE_DELTA** | The delta between User Timezone and the JOOXTER Application Server Timezone | (String)|      	
| **HAS_INDOOR_MAP** | Indicates if the user is geolocalizable | "true"\|"false"|
| **PHOTO** | The User Picture | Picture encoded in base64 (String) |   	
| **PROFILE** | The User Profile | (String) |     	 
| **STATUS_USER** | The User current Status |(See [Rule below](#rule))|   

###Rule

 - **BUSYM** : if the User sets in its settings that he's busy, otherwise,
 - **BUSYA** : if the User is participating to a Booking, otherwise,
 - **FREE** : if the User isn't participating to a Booking,
 - **UNKN** : in case of technical problem.

##Get Info

###Function 

The function name, to call, is ***getuser***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |
| **uid** | Technical ID of the Requested User | (none) | YES |

###Output

Example
```json
    {
	    "STATUS":"SUCCESS",
		"ID_USER":1,
		"TITLE":"Mr",
		"LASTNAME":"DOE",
		"FIRSTNAME":"John",
		"USER_TYPE":3,
		"TELEPHONE":"0000000000",
		"EMAIL":"john.doe@demo.jooxter",
		"LANG":"fr",
		"DEFAULT_LOC_NAME":"DEMO LOCATION",
		"DEFAULT_LOC_ID":1,
		"DEPARTMENT":"Design Office",
		"PHOTO":"",
		"PROFILE":"",
		"STATUS_USER":"FREE",
		"LAST_POSITION :{
			"ID_LOCATION":1,
			"ID_RESOURCE":55,
			"NAME":"Demo Office",
			"DATE":"19/05/2015 12:48"
			}
	}
```

Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Technical Response Status | "SUCCESS" \| "ERROR" |
| **HASH** | User Authentification Token | (String)|
| **USER_ID** | Technical User ID | (String) |
| **TITLE** | User Title | "Mr"\|"Miss"\|"Madam"\|"Mrs"|
| **FIRSTNAME** | User Firstname | (String)|
| **LASTNAME** | User Lastname | (String)|
| **USER_TYPE** | User Administration Profile | "1"\|"2"\|"3"|
| **TELEPHONE** | User phone number | (String) |
| **EMAIL** | User email address | (String) |
| **LANG** | User Language Code | "fr"\|"en"|
| **DEFAULT_LOC_NAME** | The Location name which the User is linked to | (String) |
| **DEFAULT_LOC_ID** | Technical Location ID | (String) |
| **DEPARTMENT** | The User Department | (String) |
| **PHOTO** | The User Picture | Picture encoded in base64 (String) |   	
| **PROFILE** | The User Profile | (String) |     	 
| **STATUS_USER** | The User current Status |(See [User Status Rule](#rule))|   
| **LAST_POSITION** | The User Last known Position | (Last Position Object) |

Description of Last Position Object

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **ID_LOCATION** | Technical Location ID | (String)|
| **ID_RESOURCE** | Technical Resource ID | (String)|
| **NAME** | Resource Name | (String) |
| **DATE** | Last Position Date | (DD/MM/YYYY HH:mm)|

##Lookup

###Function 

The function name, to call, is ***searchusers***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |
| **api** | |true| YES |
| **uniqueCriteria** | Search criteria | (none)| YES |

###Output

Example
```json
    {
	    "STATUS":"SUCCESS",
	    "USERS" : [{
			"ID_USER":1,
			"TITLE":"Mr",
			"LASTNAME":"DOE",
			"FIRSTNAME":"John",
			"USER_TYPE":3,
			"TELEPHONE":"0000000000",
			"EMAIL":"john.doe@demo.jooxter",
			"LANG":"fr",
			"DEFAULT_LOC_NAME":"DEMO LOCATION",
			"DEFAULT_LOC_ID":1,
			"DEPARTMENT":"Design Office",
			"PHOTO":"",
			"PROFILE":"",
			"STATUS_USER":"FREE",
			"LAST_POSITION :{
				"ID_LOCATION":1,
				"ID_RESOURCE":55,
				"NAME":"Demo Office",
				"DATE":"19/05/2015 12:48"
				}
			}]
	}
```
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Technical Response Status | "SUCCESS" \| "ERROR" |
| **USERS** | Array of User | See [Get User Identity Card > Output](#output-1)

##Get Status

###Function 

The function name, to call, is ***getstatususer***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |

###Output

Example
```json
    {
	    "STATUS":"SUCCESS",
	    "HASH":"",
	    "STATUS_USER":"FREE"
	}
```
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Technical Response Status | "ERROR"\|"SUCCESS"|
| **HASH** | Authentification Token of the User | (String)|
| **STATUS_USER** | Current User Status | (See [User Status Rule](#rule))

#Resource

##Get Info

###Function 

The function name, to call, is ***getresource***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |
| **rid** | Technical ID of the Requested resource | (none) | YES |
| **api** |  | "true" | YES |

###Output


##Lookup

###Function 

The function name, to call, is ***searchresource***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |
| **api** | |"true"| YES |
| **d** | Technical Parameter | "false" | YES |
| **uniqueCriteria** | Search criteria | (none)| YES |

###Output

Example
```json
    {
	    "STATUS":"SUCCESS",
	    "RESOURCES" : [{
			"ID_RES" : "96",
			"RESOURCE_NAME" : "Bureau Fabien",
			"RESOURCE_CODE" : "01pnjdou",
			"PHOTO" : "",
			"STATUS_RES" : "FREE"
			}]
	}
```
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Technical Response Status | "SUCCESS" \| "ERROR" |
| **RESOURCES** | Array of Resources which match criteria | See Below |

Description of Matched Resource

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **ID_RES** | Technical Resource ID | (String) |
| **RESOURCE_NAME** | Resource Name | (String) |
| **RESOURCE_CODE** | Resource Code | (String) |
| **PHOTO** | The Resource Picture | Picture encoded in base64 (String) | 
| **STATUS_RES** | The Current Resource Status | (See [Resource Status Rule](#rule-1))|

##Get Status

###Function 

The function name, to call, is ***statusRT***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **hash** | Authentification Token of the Requester |(none)| YES |
| **r** | Technical Resource ID |(none)| YES |

###Output

Example
```json
    {
	    "STATUS":"FREE"
	}
```
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Current Resource Status | (See [Rule below](#rule-1)) |

### Rule
- **ERROR** : if ***hash*** or ***r*** parameter is not setted, or ***r*** not corresponding to a known Resource in JOOXTER, otherwise,
- **AREA** : if ***r*** is a non-bookable Resource, otherwise,
- **BUSY** : if it exists a Booking, planned in Resource ***r***, which begins before current time and ends after current time, otherwise,
- **FREE** : There is no Booking, planned in Resource ***r***, running at current time.

#Booking

##Create

###Function 

The function name, to call, is ***savebooking***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |
| **api** |  |"true"| YES |
| **resourceId** | Technical Resource ID |(none)| YES |
| **userId** | Technical User ID |(none)| YES |
| **title** | Booking title | (none) | YES |       
| **dateFrom** | Begin Date (DD/MM/YYYY) |(none)| YES |
| **dateTo** | End Date (DD/MM/YYYY) |(none)| YES |    
| **timeFrom** | Begin Time (HH:MM) |(none)| YES |
| **timeTo** | End Time (HH:MM) |(none)| YES | 
| **description** | Booking description | (none) | NO |  
| **isDetailPrivate** | Booking privacy | "false" | NO |
| **invitedPeople** | List of invited people (emails separated with ",")| (none) | NO |

###Output

Example
```json
    {
	    "STATUS":"SUCCESS",
	    "LABEL":"BOOKING CREATED"
	}
```
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Create Booking Status | "ERROR"\|"SUCCESS" |
| **LABEL** | Explanation label | (See [Rule below](#rule-2)) |
| **ID** | Created Booking ID | (String) |

###Rule

- if the ***userHash*** is not a valid token then STATUS="ERROR", LABEL = "USER NOT FOUND",
- otherwise, if the Booking period overlaps an existing Booking linked to the Resource ***resourceId*** then STATUS="ERROR", LABEL = "OVERLAP",
- otherwise, STATUS="SUCCESS", LABEL = "BOOKING CREATED".

##Get Info

###Function 

The function name, to call, is ***getbooking***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |
| **api** | | "true" | YES |
| **bookingId** | Booking ID | (none) | YES |

###Output

Example
```json
    {  
       "STATUS":"SUCCESS",
       "booking":{  
          "idBooking":"1",
          "idResource":"1",
          "resourceName":"Demo",
          "idUser":"1",
          "bookingOwner":"John Doe",
          "title":"Booked by John Doe",
          "description":"Booked by John Doe",
          "confirmed":"true",
          "timestampFrom":"1431000000000",
          "timestampTo":"1431021600000",
          "recurrency":"false",
          "location":"Demo Location",
          "updateFromMobile":"false",
          "creationFromMobile":"false",
          "dateFrom":"01/01/1970 14:00",
          "dateTo":"01/01/1970 20:00",
          "checkedIn":"false",
          "timestampCheckedIn":"",
          "checkedOut":"false",
          "timestampCheckedOut":"",
          "isInternal":"false",
          "code":"anydkowx",
          "isDetailPrivate":"false",
          "status":"Approved",
          "idCreatedBy":"2",
          "createdBy":"John Smith",
          "timestampCreatedBy":"1970-01-01 08:00:00.0",
          "idModifiedBy":"2",
          "modifiedBy":"John Smith",
          "timestampModifiedBy":"1970-01-01 09:00:00.0",
          "color":"black",
          "cancellationPolicy":"120"
       }
    }
```
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Technical Status | "ERROR"\|"SUCCESS" |
| **booking** | Booking Having ***bookingId*** criteria ||
| **idBooking** | Booking ID | (String) |
| **idResource** | Technical Resource ID | (String)|
| **resourceName** | Resource Name | (String)| 
| **idUser** | Owner User ID | (String) |
| **title** | Booking Title | (String) |
| **description** | Booking Description | (String) |
| **confirmed** | Booking confirmation |"true"\|"false"|
| **timestampFrom** | Begin Timestamp |(number of milliseconds since midnight January 1, 1970)|
| **timestampTo** | End Timestamp |(number of milliseconds since midnight January 1, 1970)|
| **recurrency** | *Not to use yet* ||
| **location** | Location Name| (String)|
| **updateFromMobile** | Indicates if the booking was updated from Mobile device | "true"\|"false"|
| **creationFromMobile** | Indicates if the booking was created from Mobile device | "true"\|"false"|
| **dateFrom**| Booking Begin date | (DD/MM/YYYY HH:MM)|
| **dateTo**| Booking End date | (DD/MM/YYYY HH:MM)|
| **checkedIn** | Indicates if the Booking is checked in | "true"\|"false"|
| **timestampCheckedIn** | Checked in Timestamp |(YYYY-MM-DD HH:MM:SS)|
| **checkedOut** | Indicates if the Booking is checked out | "true"\|"false"|
| **timestampCheckedOut** | Checked out Timestamp |(YYYY-MM-DD HH:MM:SS)|
| **isInternal** | |"true"\|"false"|
| **code** | |(String)|
| **isDetailPrivate** | |"true"\|"false"|
| **status** | Booking Status | "Requested"\|"Approved"\|<br>"Rejected"\|"Cancelled"\|<br>"Finished"|
| **idCreatedBy** |  User ID who created the Booking | (String) |
| **createdBy** |  Username who created the Booking | (String) |
| **timestampCreatedBy** | Creation Timestamp | (YYYY-MM-DD HH:MM:SS)|
| **idModifiedBy** |  User ID who created the Booking | (String) |
| **modifiedBy** |  Username who created the Booking | (String) |
| **timestampModifiedBy** | Creation Timestamp | (YYYY-MM-DD HH:MM:SS)|
| **color** | *Not to use yet* | (String) |
| **cancellationPolicy** | Delay, before the beginning of the Booking, the owner can't cancel it. | (String) |


##Update

###Function 

The function name, to call, is ***updatebooking***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |
| **api** |  |"true"| YES |
| **resourceId** | Technical Resource ID |(none)| YES |
| **userId** | Technical User ID |(none)| YES |
| **bookingId** | Technical Booking ID |(none)| YES |
| **title** | Booking title | (none) | YES |       
| **dateFrom** | Begin Date (DD/MM/YYYY) |(none)| YES |
| **dateTo** | End Date (DD/MM/YYYY) |(none)| YES |    
| **timeFrom** | Begin Time (HH:MM) |(none)| YES |
| **timeTo** | End Time (HH:MM) |(none)| YES |  
| **description** | Booking description | (none) | NO | 
| **isDetailPrivate** | Booking privacy | "false" | NO |
| **invitedPeople** | List of invited people (emails separated with ",")| (none) | NO |

###Output

Example
```json
    {
	    "STATUS":"SUCCESS",
	    "LABEL":"BOOKING UPDATED"
	}
```	
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Update Booking Status | "ERROR"\|"SUCCESS" |
| **LABEL** | Explanation label | (See [Rule below](#rule-3)) |

###Rule

- if the ***userHash*** is not a valid token then STATUS="ERROR", LABEL = "USER NOT FOUND",
- otherwise, if the Booking period overlaps an existing Booking linked to the Resource ***resourceId*** then STATUS="ERROR", LABEL = "OVERLAP",
- otherwise, STATUS="SUCCESS", LABEL = "BOOKING UPDATED".

##Cancel

###Function 

The function name, to call, is ***cancelbooking***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |
| **api** |  |"true"| YES |
| **b** | Technical Booking ID |(none)| YES |

###Output

Example
```json
    {
	    "STATUS":"SUCCESS"
	}
```	
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Cancel Booking Status | "ERROR"\|"SUCCESS" |

##Lookup

###Function 

The function name, to call, is ***requestbooking***.

###Input
| Name     | Description | Default Value   | Mandatory |
| :------- | :--------------- | ------: | :---: |
| **userHash** | Authentification Token of the Requester |(none)| YES |
| **api** |  |"true"| YES |
| **title** | Booking Title | (none) | NO |
| **description** | Booking Description | (none) | NO |
| **resourceId** | Technical Resource ID linked to the Booking | (none) | NO |
| **resourceTypeId** | Technical Resource Type ID | (none) | NO |    
| **locationId** | Technical Location ID | (none) | NO |  
| **userId** | Technical User ID | (none) | NO |    
| **dateFrom** | Booking Begin Date (DD/MM/YYYY)| (none)| NO |
| **dateTo** | Booking Begin Date (DD/MM/YYYY)| (none)| NO |
| **timeFrom** | Booking Begin Time (HH:MM)|(none)| NO |
| **timeTo** | Booking End Time (HH:MM)|(none)| NO |
| **yours** | Retrieve restriction | "false" | NO|   

###Output

Example
```json
    {
    	"STATUS":"SUCCESS",
    	"BOOKINGS":[
          {
             "id":"1",
             "corporation":"Demo Corporation",
             "bookingOwner":"John Doe",
             "bookingOwnerId":"1",
             "createdBy":"John Doe",
             "dateCreatedBy":"20/05/2015 13:14",
             "resourceOwner":"John Smith",
             "resourceId":"2",
             "cancellationPolicy":"",
             "status":"Approved",
             "timezone":"Europe/Paris",
             "resourceCode":"4opdk3",
             "resource":"Demo Resource",
             "bookingTitle":"Title",
             "code":"uvtsnmkw",
             "dateFrom":"01/01/1970 14:00",
             "dateTo":"01/01/1970 18:00",
             "start":"1970-01-01T14:00:00",
             "end":"1970-01-01T18:00:00",
             "guests":[
                {
                   "STATUS_INVITATION":"true",
                   "FIRSTNAME":"John",
                   "STATUS_USER":"BUSYM",
                   "EMAIL":"john.smith@jooxter.com",
                   "DEFAULT_LOC_ID": 1,
                   "USER_TYPE":3,
                   "TELEPHONE":"",
                   "DEFAULT_LOC_NAME":"Demo Location",
                   "LANG":"fr",
                   "TITLE":"Mr",
                   "ID_USER":2,
                   "LASTNAME":"Smith"
                }
             ]
          }
       ]
    }
```	
Description

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS** | Technical Status | "ERROR"\|"SUCCESS" |
| **BOOKINGS** | List of Booking, matching with criteria | Array of Booking (See description below)|
| **id** | Booking ID | (String) |
| **corporation** | Corporation name | (String)|
| **bookingOwnerId** | Owner User ID | (String) |
| **bookingOwner** | Owner User Name | (String) |
| **bookingTitle** | Booking Title | (String) |
| **createdBy** | Name of the User who created the Booking | (String) |
| **dateCreatedBy** | Date of Creation (DD/MM/YYYY HH:MM) | (String) |
| **resourceOwner** | Manager name of the Resource | (String) |
| **resourceId** | Technical Resource ID | (String)|  
| **resource** | Resource Name | (String)|          
| **cancellationPolicy** | Delay, before the beginning of the Booking, the owner can't cancel it. | (String) |
| **status** | Booking Status | "Requested"\|"Approved"\|<br>"Rejected"\|"Cancelled"\|<br>"Finished"|
| **timezone**| Timezone of the Booking | (String)|
| **dateFrom**| Booking Begin date | (DD/MM/YYYY HH:MM)|
| **dateTo**| Booking End date | (DD/MM/YYYY HH:MM)|
| **start**| Booking Begin date | (YYYY-MM-DDTHH:MM:SS)|
| **end**| Booking End date | (YYYY-MM-DDTHH:MM:SS)|             
| **guests**| List of invited people | (See Description below) |

 Description of invited people

| Name     | Description | Value  |
| :------- | :--------------- | ------: |
| **STATUS_INVITATION** | Invitation Status | "wait"\|"true"\|"false" |
| **TITLE** |  See ([User > Get Info > Output](#output-1))||
| **ID_USER** | See ([User > Get Info > Output](#output-1))||
| **FIRSTNAME** | See ([User > Get Info > Output](#output-1))||
| **LASTNAME** | See ([User > Get Info > Output](#output-1))||
| **USER_TYPE** | See ([User > Get Info > Output](#output-1))||
| **TELEPHONE** | See ([User > Get Info > Output](#output-1))||
| **EMAIL** | See ([User > Get Info > Output](#output-1))||
| **LANG** | See ([User > Get Info > Output](#output-1))||                       
| **DEFAULT_LOC_ID** | See ([User > Get Info > Output](#output-1))||
| **DEFAULT_LOC_NAME** | See ([User > Get Info > Output](#output-1))||
| **STATUS_USER** | See ([User > Get Info > Output](#output-1))||           




