# Regex Tutorial

<br/>

## **Link** to video walkthrough to demonstrate app functionality: https://drive.google.com/file/d/1IIZau0uRP8h1FsaSFpihcZrEHgtrjyFB/view

## **Link** to project repository: https://github.com/alinz07/mod13-ecommerce-backend

<br/>

## **Motivation**

E-commerce is the largest secotr of the electronics industry and develops should understand the fundamental archiecture of e-commerce sites.

<br/>

## **Table of Contents**

[How and Why?](#what-problem-does-this-solve-and-how-was-a-solution-accomplished) <br/>
[Things I learned](#things-i-learned) <br/>
[What makes this project stand out?](#what-makes-this-project-stand-out) <br/>
[Challenge Criteria](#challenge-criteria)<br/>
[Screenshot of Web Application](#screenshot-of-web-application)<br/>
[How to Contribute](#how-to-contribute)<br/>
[Credits](#credits)<br/>

<br/>

## **What Problem does this solve and how was a solution accomplished?**

I wanted to build the back end for an e-commerce site by using a working express.js API and configuring it to use Sequelize to interact with a MySQL database.
<br/>

## **Things I learned**

-   How to write API routes to perform RESTful CRUD operations
-   How to use Sequelize to write Models and create data associations between Models.
    <br/>

## **What makes this project stand out?**

This project stands out thanks to its consistent data validations in the API routes. If a user queries to find or delete a product, tag, or category using an id that doesn't exist, they will receive a json response that "there is no x with that id."

<br/>

## **Challenge Criteria**

AS A manager at an internet retail company
I WANT a back end for my e-commerce website that uses the latest technologies
SO THAT my company can compete with other e-commerce companies

GIVEN a functional Express.js API<br/>

-   WHEN I add my database name, MySQL username, and MySQL password to an environment variable file<br/>
    THEN I am able to connect to a database using Sequelize

-   WHEN I enter schema and seed commands<br/>
    THEN a development database is created and is seeded with test data

-   WHEN I enter the command to invoke the application<br/>
    THEN my server is started and the Sequelize models are synced to the MySQL database

-   WHEN I open API GET routes in Insomnia for categories, products, or tags<br/>
    THEN the data for each of these routes is displayed in a formatted JSON

-   WHEN I click on an existing note in the list in the left-hand column<br/>
    THEN that note appears in the right-hand column

-   WHEN I test API POST, PUT, and DELETE routes in Insomnia<br/>
    THEN I am able to successfully create, update, and delete data in my database
    <br/>

## **Screenshot of Web Application**

This applicatin has no front end. Please see walkthrough video link up top.
<br/>

## **How to Contribute**

Please feel free to review, refactor and submit a pull request for additional features on my github page: <br/>
https://github.com/alinz07

### **Credits**

-   Source code provided by University of Wisconsin-Milwaukee Extended Campus Full-Stack Coding Bootcamp
