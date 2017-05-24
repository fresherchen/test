# validic interface readme. version 0.1

** used interface **

function | description | interface 
---------------|--------------|-----------|-------------------------------------
getValidicUser | get the current validic user info from the table 'ValidicUser', including patient id, validic user id and validic user token properties | get /api/ValidicUsers/getUser 
provisionUser | used to create a user's profile and exchange the user id (patientId) for a validic id (validicUserId) on Validic | post /api/ValidicUsers/provisionUser  params: userId 
connectDevice | validate the user's token from table ValidicUser before adding devices, return null when it is correct, or return new token when it is invalid | get /api/ValidicUsers/connectDevice params: userToken, validicUserId 
getSyncedApp | used to synchronize all the devices which has authorized by the current user | get /getSyncedApp params: usertoken 
pollForUpdates | update the table 'ValidicUser' | post /pollForUpdates 


***

All validic objects -- 'biometrics','diabetes','fitness','nutrition','routine','sleep','user','weight' -- whose pre-provisioning interfaces are available because of loopback.

***


** not used interface **

interface  |  description
------------------------|-------------------------------------------------------
Updating User Records    | Push Notifications(individual user-level) or Latest Endpoint(bulk population-level)
Managing Change   |  Push Notification Service/Authenticating Push Notifications/Validating the Signature  How to prepare for change and manage your version
Working With Users  | Suspend/Updating Users/Delete Users/User Credentials
Validic Connect Partner API | Validic Connect allows developers of health apps and devices to push data seamlessly and safely through their platform API and eliminates the need to build and maintain a custom solution.
Mobile  |  Android Documentation/Cordova Documentation
