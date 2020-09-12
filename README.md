# Cloud Project

## Technologies Used
* AWS Lambda 
* AWS RDS
* Sails (Nodejs framework)
* AWS Cognito for user management

## Description
* The project involves 3 companies which interact with each other. 
* Blossom is a company which supplies flowers to FlowerBasket company. 
* WickerBasket is a company which supplies baskets to FlowerBasket company.
* FlowerBasket company is for user, where they can place order for combos of Flower and Basket. They can customize the flower and basket if they want and place order. To place order user must login.
* FlowerBasket company uses AWS Cognito for user management. All the users registered at flower basket credentials are kept in AWS cognito. Whenever new user registers user is added to the user pool. User cannot login until they verify their email.

## Architecture of Blossom Company
* The application is developed on Salis framework and all the logic is written in AWS lambda functions. 
* When user hits the url, the request is passed to the method in Sailsjs linked to the request. It internally calls the lambda function corresponding to that function.
