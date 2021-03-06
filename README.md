# Cloud Project

## Technologies Used
* AWS Lambda (For logic)
* AWS RDS ( For database)
* Sails (Nodejs framework, for UI development)
* AWS Cognito (For user management)

## Description
* The project involves 3 companies which interact with each other. 
* Blossom is a company which supplies flowers to FlowerBasket company. 
* WickerBasket is a company which supplies baskets to FlowerBasket company.
* FlowerBasket company is for user, where they can place order for combos of Flower and Basket. They can customize the flower and basket if they want and place order. To place order user must login.
* FlowerBasket company uses AWS Cognito for user management. All the users registered at flower basket credentials are kept in AWS cognito. Whenever new user registers user is added to the user pool. User cannot login until they verify their email.

## Two Phase Commit Protocol
The two phase commit protocol is implemented for flower basket combos orders on the Flower Basket company.
### Pre-conditions
* Before starting the two phase protocol, it checks if the quantity of ordered flower and basket is available to successfully process the order. If the quantity is not available it will not start two phase commit protocoal and return response with message "Not able to process order as we are running out of stock"
### Phase 1
* Flower Basket generates the unique order id which will be assigned to XA transaction, blossom and wicker basket APIs are called to place the order.
* Blossom company receives the order and starts the XA transaction. It update the quantity of available flowers and add entry to order history. After running the sql queries, XA transaction is put into IDLE state. The response of SQL queries is returned Flower Basket company.
* Wicker Basket company receives the order and starts the XA transaction. It updates the available quantity of baskets by subtracting the quantity ordered, and add entry to order history. After running the sql queries, XA transaction is put into IDLE state. If there is error in running the SQL queries, result is returned with flag false else true.

### Phase 2
* If both the companies return true, flower and wicker basket apis to complete the order are called with commit flag true, and XID which is order id generated in phase one.
* If any company returns false, complete order api of both companies are called with commit flag, indicating to rollback the transaction and order id.
* Flower and Wicker Basket companies complete the order on the basis of commit flag. The XA transaction is moved from IDLE state to prepare statement and terminated using ROLLBACK or COMMIT.

## Lambda functions
The lambda functions are created and linked to the API Gateway which provides the url to access the functions inside the application.
## Architecture and functionalities of Blossom Company
* The application is developed on Salis framework and all the logic is written in AWS lambda functions. 
* When user hits the url, the request is passed to the method in Sailsjs linked to the request. It internally calls the lambda function corresponding to that function.
* Flower can be easily searched, as partial search is supported by the application.
 ![Alt Text](https://github.com/ruminder-hub/cloud_project/blob/master/screenshots/blossom_home_page.png)
 <p align="center">Figure 1: Home page of blossom company.</p>
 
## Architecture and functionalities of Wicker Basket Company
* Similar to Blossom company, the application is developed on Salis framework and all the logic is written in AWS lambda functions. 
* When user hits the url, the method corresponding the request is called, which calls the AWS Lambda function and returns the response on the basis of result returned by lambda function.
* New basket can be added to the database.
* Details of basket can be updated and baskets which are no longer in demand can be deleted.
 ![Alt Text](https://github.com/ruminder-hub/cloud_project/blob/master/screenshots/wicker_basket_home_page.png)
 <p align="center">Figure 2: Home page of wicker basket company.</p>
 
## Architecture and functionalities of Flower Basket Company
* The application is completely written in Sails framework and uses Waterline ORM for any operations in database.
* It uses the APIs provided by blossom and wicker basket company.
* The flower and basket combos which are hugely popular are created by the company and displayed at the home page.
* User can create their own flower basket combos, application fetch the flower and basket list from blossom and wicker company respectively.
* To order any combos, user must have login to the application

 ![Alt Text](https://github.com/ruminder-hub/cloud_project/blob/master/screenshots/flower_basket_home_page.png)
 <p align="center">Figure 3: Home page of flower basket company.</p>
 
 * User management is done by AWS Cognito.
 * Whenever new user registers at the application, their details are stored in the User Cognito.
 * User cannot login to the application, without verifying the mail provided.
 
  ![Alt Text](https://github.com/ruminder-hub/cloud_project/blob/master/screenshots/signup_page.png)
   <p align="center">Figure 4: User signup page of flower basket company.</p>
   
   ![Alt Text](https://github.com/ruminder-hub/cloud_project/blob/master/screenshots/aws_cognito.png)
   <p align="center">Figure 5: User pool created in AWS cognito, showing some of the user accounts created.</p>
   
### Future Scope
* The access token generated after login can be used to secure the lambda function access.
  
  
 
