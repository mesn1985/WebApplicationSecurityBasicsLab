# Endpoint analysis with postman.
The purpose of this exercise is to introduce the OpenApi specifications, and to have repetition of the skills learned
in previous exercises.

## prerequisites
Prior to starting these exercises, you should have completed the [postman exercise with crAPI](../crAPI/4_Endpoint_Analysis_with_postman.md)

## 1 -  Using the Swagger documentation of Juiceshop
In the previous Juiceshop exercises found in [3 Active reconnaissance](3_Active_reconnaissance.md) exercise 4. Gobuster and the wordlist _Swagger.txt_ uncovered
that there is an endpoint called `/api-docs/swagger.json` in the juiceshop API. [Swagger](https://swagger.io/docs/specification/about/) is a set of tools, used
to create API documentation around the [OpenApi specifications](https://swagger.io/docs/specification/about/). This indicates that documentation for the use of
the Juiceshop API can be found here. In this exercise, you will explorer the documentation found in the Swagger endpoint.

1. Try experimenting with what you can find in the documentation at the endpoint `/api-docs/swagger.json`, does quantity have validation in the backend API?

## 2 - Build a juiceshop collection
The approach in this exercise, is similar to the approach you used with with crAPI in exercise [4 Endpoint Analysis with postman](../crAPI/4_Endpoint_Analysis_with_postman.md).
Build a request collections for Juiceshop. You could build it manually, or by proxy'ing the browser through Postman. How you do it, is up to you.

