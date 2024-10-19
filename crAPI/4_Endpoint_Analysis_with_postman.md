# Endpoint analysis with postman.
The purpose of these exercise is to introduce the tool Postman. Postman is not a security tool, but at tool made
for testing API's. None the less, postman have several useful features when analyzing an API endpoint.
One of these features are the ability to create collections of request, which is connivent for reusing HTTP requests
sent to API endpoints, and thereby avoid having to recreate the requests through a browser or by hand. 

In these exercises you will interact with crAPI Using Postman (And at the same time,  do a bit of repetition with Burp suite)

## prerequisites
The Exercises [2 Exploiting BOLA and Excessive data exposure](2_Exploiting_BOLA_And_Excessive_Data_Exposure.md) and [3 Active reconnaissance](3_Active_reconnaissance.md) should
be completed prior to starting these exercises.

Furthermore postman should be installed. You can follow the guide [here](https://www.postman.com/downloads/)
You can also follow get and basic introduction to Postman [here](https://learning.postman.com/docs/getting-started/first-steps/sending-the-first-request/).

You should create a postman account. The Postman application only offers limited features for unauthenticated users.

The general documentation for creating requests and collections in postman, can be viewed [here](https://learning.postman.com/docs/collections/using-collections/)

**Be aware, postman does not automatically save, so when you have made some changes and there is a button that says save, you should click it.**

## 1 - Create new collection in postman
In this exercise, you will create a collection of HTTP requests for crAPI. This ensures that you don't have to manually recreate requests
when analyzing the _crAPI_ application. 

Perform the following actions to create your first collection in postman. Guidance for this task can be found [here](https://learning.postman.com/docs/collections/using-collections/#creating-collections).
1. In the left side of Postman, click the Collections tab.  
2. Click the `+` sign appearing to the right of the Collections tab And choose `Blank collection`
3. The default name will be _New collection_. Change the name to _crAPI_

## 2  - Capture the sign up request send to crAPI
In this exercise you will use burp suite to capture the sign up request sent to crAPI when creating a new user,
and then add the request to your crAPI collection.

Perform the following actions
1. In burp suite, capture the POST request used for creating a new user. To refresh how to a capture a request, review  your solutions for the exercises in [2 - Exploiting BOLA](2_Exploiting_BOLA_And_Excessive_Data_Exposure.md).
2. In Postman, create a new request inside the crAPI collection, and name it `signup`. Guidance can be found [here](https://learning.postman.com/docs/collections/using-collections/#creating-collections)
3. In the new request, Set the http method as a `POST` request. Guidance can be found [here](https://learning.postman.com/docs/sending-requests/requests/#selecting-request-methods)
4. Copy the url from the request created by burp suite in step 1 and paste it into the URL of the postman request. _In Burp suite proxy HTTP History, you can right click the URL and choose copy url from the context menu_
5. Copy the JSON content from the body of the request created by Burp suite in step one, and paste it into the body of the request in Postman. guidance can be found [here](https://learning.postman.com/docs/sending-requests/requests/#sending-body-data) The data should be raw, and in JSON format.
6. **Click save**
7. Click send to verify that the request works. _Must likely you will receive HTTP status code 403 in the response, saying that some of the request values are already used. Change them until you receive status code 200, and a new user have successfully been created_

## 3 - Bypassing frontend validation
If you try to create a user with a password that does not comply with the crAPI password policy from the crAPI website, you will
get an error in red text stating that the password needs to comply with a specific password  policy. 
But interestingly enough, Burp suite does not show request being sent to the backend API this validation. This could potentially mean that 
the application only validates the password compliance in the frontend(browser), and not in the backend API. You should find out, by experimenting.
  
With the POST _signup_ request, try to see if you can create a user, with a password length of 4, that only contains alphabetic characters, this might
give you some guidance on how the backend API validates the compliances of new passwords, by reading the returned message.

## 4 - Using postman as an authenticated user.
In this exercise you will obtain a bearer token using Burp suite, and use that bearer token to make
authorized request from postman.

Perform the following actions
1. From burp suite, capture the post request used for authentication (login).
2. From the response of the request, copy the value of the token value.(Only the value of token).
3. In Postman, Edit the `crAPI` collection by clicking the 3 dots to the right of the collection name.
4. In the edit menu, select `Authorization`.
5. In the drop down menu for _type_, select `Bearer Token`.
6. In the newly appeared text field for _Token_, paste in the value of the token.
7. Click the `Save` button.

In the next exercise you will test if the authorization works.

A general description of what a bearer token is, can be found [here](https://apidog.com/articles/what-is-bearer-token/)

## 5 - Create new post in community
In this exercise you will add a new request to the crAPI collection. The request is the one for posting new messages to the crAPI _community board_.
This will also test if the Authorization from the previous exercise works.

Perform the following actions
1. In Burp suite, capture the POST request for posting a new message to the community board.
2. Copy the POST request URL (Same procedure as exercise 2 )
3. In Postman, create a new POST request in the ´crAPI´ collection.
4. Paste the request URL into the URL field of the Postman request.
5. In Burp suite, copy the JSON content body of the POST request(Same procedure as exercise 2)
6. In the Postman request, add the JSON from the content body of the request.
7. Send the request from postman, and verify that the status code of the response is 200.
8. Click Save.
9. Enter the crAPI website, and verify that the post is now in the board.

## 6 Create collection using the browser
So far, you have manually created all of the requests. But all of these  creates can also be created, by allowing 
Postman to intercept all of the requests send from the browser to the API, by using Postman as a Proxy for the browser.

Postman have an built in proxy server on Port 5555 which allows postman to act as proxy server. There are many different ways 
to setup the browser to use a proxy. But in this exercise you will use postman Interceptor for convenience.

Perform the following actions
1. Install the _Postman interceptor_ extension to your browser following [this guide](https://learning.postman.com/docs/sending-requests/capturing-request-data/interceptor/)
2. Before you start intercepting, create a new  collection in Postman called `CrAPI-Automated`
3. Enter the crAPI website.
4. Use the browser extension to start capturing requests.
5. Create a new user.
6. Login as the new user.
7. Register the new user with the information from mailhoq (Port 8025)
7. Enter the crAPI dashboard and use the `Contact Mechanic` and the `Refresh location` feature(After you have added the new vehicle).
8. In the Shop, buy a seat, and use the `Past Orders` feature.
9. In the Community, create a `New Post`
10. Use the browser extension to stop capturing requests. (Verify that capturing have stopped in post man)
11. In postman, tick off all of the request, and click on the `+ save request` in the upper right corner.
12. Click the `CRapi-automated` as the collection you want to add to.
13. In the _Organize by request windows_, tick off both `Domain name` and `Endpoints` and click save.
14. Now you have all the requests send from the browser to the api, in a ready to use collection.

**All of the captured requests, have been captured with the bearer token included in the header. You could change the token by adding a token for the collection in the same way you did in exercise 5. Remember to remover the Authorization header from the requests**

## 7 Use Burp suite as a proxy for postman
Postman is not a tool for security testing. Burp suite on the other hand i. So the use of these two in combination i a very powerful tool, because 
Postman can hold an ready to use collection of API requests, and burp suite have numerous of security features.  In this exercise, you will setup
Postman to proxy through Burp suite, thereby using various analytic features of Burp suite with requests sent from Postman.

Perform the following actions
1. Open Postman settings from `File -> Settings`
2. Click the `proxy` tab
3. Click the on switch for `Use custom proxy configurations`
4. The server ip should be `127.0.0.1` and the port should be `8080`(Default proxy port Burp suite)
5. Click the `General` tab of the settings.
6. Click on the switch for `SSL certificate verification` to turn off verification of certificates.
7. Ensure that burp suite is running.
8. In the `proxy` tab of Burp suite, select HTTP History.
9. Send a couple of requests from Postman, and verify the These request appears in Burp suites HTTP history.
