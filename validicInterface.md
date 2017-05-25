# validic interface readme. version 0.1

** used interface **

function | description | interface | validic interface
---------------|--------------|-----------|----------------
getValidicUser | get the current validic user info from the table 'ValidicUser', including patient id, validic user id and validic user token properties | get /api/ValidicUsers/getUser | https://api.validic.com/v1/organizations/:organization_id/users/:user_id/refresh_token.json https://api.validic.com/v1/organizations/:organization_id/users.json
provisionUser | used to create a user's profile and exchange the user id (patientId) for a validic id (validicUserId) on Validic | post /api/ValidicUsers/provisionUser | https://api.validic.com/v1/organizations/:organization_id/users/:user_id/refresh_token.json https://api.validic.com/v1/organizations/:organization_id/users.json
connectDevice | validate the user's token from table ValidicUser before adding devices, return null when it is correct, or return new token when it is invalid | get /api/ValidicUsers/connectDevice params: userToken, validicUserId | https://api.validic.com/v1/organizations/:organization_id/users/:user_id/refresh_token.json
getSyncedApp | used to synchronize all the devices which has authorized by the current user | get /getSyncedApp params: usertoken | https://api.validic.com/v1/organizations/{ORGANIZATION_ID}/apps.json?authentication_token={USER_ACCESS_TOKEN}&access_token={ORGANIZATION_ACCESS_TOKEN}
pollForUpdates | update the table 'ValidicUser' | post /pollForUpdates | https://api.validic.com/v1/organizations/{ORGANIZATION_ID}/users.json?access_token={ORGANIZATION_ACCESS_TOKEN}&status=all  https://api.validic.com/v1/organizations/:organization_id/users/:user_id/refresh_token.json https://api.validic.com/v1/organizations/:organization_id/users/:user_id/fitness.json https://api.validic.com/v1/organizations/:organization_id/users/:user_id/routine.json https://api.validic.com/v1/organizations/:organization_id/users/:user_id/nutrition.json https://api.validic.com/v1/organizations/:organization_id/users/:user_id/sleep.json https://api.validic.com/v1/organizations/:organization_id/users/:user_id/weight.json https://api.validic.com/v1/organizations/:organization_id/users/:user_id/diabetes.json https://api.validic.com/v1/organizations/:organization_id/users/:user_id/biometrics.json


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


** resource & contact information **
Validic API documentation: https://docs.validic.com/v1.2016-03-01/docs,
ValidicNodeJS repository: https://github.com/Validic/ValidicNodeJS.

Any question about this documentation you can contact me by email jack.chen@claifyhealth.com
