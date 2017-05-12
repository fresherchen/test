# validic interface readme. version 0.1

** used interface **

function | description | interface | implementation logic
---------------|--------------|-----------|-------------------------------------
getValidicUser | get validic user info from validicUser table, including patient id, validic user id and validic user token | get /api/ValidicUsers/getUser | get validic user by the current user's person_id from validicUser table if it exists, or provision it on validic, save and return one if it doesn't exists
provisionUser | used to create a user's profile and exchange our User ID (patientId) for a Validic ID (validicUserId) on Validic | post /api/ValidicUsers/provisionUser  params: userId | create validic user's profile by the parameter 'userId', convert the userId to the patientId, then save the patientId as an object to the table ValidicUser and return this object. Observe the property of the validic user id before saving, find and save in db if it exists in validic, or provisioning the user object if it doesn't exist in validic.
connectDevice | validate the user's token from table ValidicUser before adding devices, return null when it is correct, or return new token when it is invalid | get /api/ValidicUsers/connectDevice params: userToken, validicUserId | validate the token by requesting the url of the validic marketplace with the parameter useToken, observe the response status code, return null when the code is 200, or refresh, save and return the token with the parameter 'validicUserId' when the code is small than 200 or big than 299
getSyncedApp | used to synchronize all the devices which has authorized by the current user | get /getSyncedApp params: usertoken | get all the authorized apps with the parameter 'usertoken'
pollForUpdates | update the table 'ValidicUser' | post /pollForUpdates | update the validic user by invoking the function in file 'server/validic/updateUserInfo'


***

All validic objects -- 'biometrics','diabetes','fitness','nutrition','routine','sleep','user','weight' -- whose pre-provisioning interfaces are available because of loopback.

***


** not used interface ** 

interface  |  introduce
------------------------|-------------------------------------------------------
Updating User Records    | Push Notifications(individual user-level) or Latest Endpoint(bulk population-level)
Managing Change   |  Push Notification Service/Authenticating Push Notifications/Validating the Signature  How to prepare for change and manage your version
Working With Users  | Suspend/Updating Users/Delete Users/User Credentials
Validic Connect Partner API | Validic Connect allows developers of health apps and devices to push data seamlessly and safely through our platform API and eliminates the need to build and maintain a custom solution.
Mobile  |  Android Documentation/Cordova Documentation
