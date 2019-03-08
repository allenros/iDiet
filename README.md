# iDiet
## Overview
iDiet is a mobile web application that prescribes users customizable meal plans based on health goals, dietary preferences/restrictions, and weekly budget for food.

## Development
To set up iDiet for local development:
1. Clone this repository via:  
`git clone https://github.com/joshlopez97/iDiet.git`

2. From the root directory, install the dependencies by running:  
`npm install`

3. Run the application via:  
`node server.js`  
and then go to http://localhost:5000 to view the application.

## MySQL Database Schema
iDiet Accounts:
- Email: User's email used to sign in
- UserPassword: Chosen password used to sign in (will be replaced with PasswordHash)
- FirstName: User's first name
- Height: User's height stored in inches
- Weight: User's weight stored in pounds
- Age: User's age (will be replaced with date of birth)
- Allergies: String containing comma separated list of allergies to avoid in diet plan
- GoalWeight: int containing the weight that user wishes to be at
- WeeklyBudget: int containing amount of money user wishes to spend on food
- DailyCalories: int containing amount of calories user should consume
- FitBitConnected: bit indicating if FitBit is connected
```
CREATE TABLE Account (
  Email varchar(255) UNIQUE NOT NULL,
  UserPassword varchar(255) NOT NULL,
  FirstName varchar(255) NOT NULL,
  Height int NOT NULL,
  Weight int NOT NULL,
  Age int NOT NULL,
  Allergies varchar(255),
  GoalWeight int NOT NULL,
  WeeklyBudget int NOT NULL,
  DailyCalories int NOT NULL,
  FitBitConnected bit NOT NULL
);
```
Cached Meal Metadata from Spoonacular Nutrition API:
- mid: int containing unique Recipe ID of meal
- title: description of food
- type: breakfast, lunch, or dinner
- price: estimated price based on ingredients
- imagelink: link to thumbnail image
- calories: calories in meal
- protein: protein in meal
- carbs: total carbohydrates in meal
- fats: total fats in meal
- link: original link to recipe
- slink: Spoonacular link to recipe
- vegetarian: bit indicating if meal is vegetarian
- vegan: bit indicating if meal is vegan
- glutenfree: bit indicating if meal is gluten-free
- dairyfree: bit indicating if meal is dairy-free
- ketogenic: bit indicating if meal is ketogenic
```
CREATE TABLE MealEntry (
  mid int UNIQUE NOT NULL,
  title varchar(255),
  type varchar(32) NOT NULL,
  price varchar(10),
  imagelink varchar(255),
  calories int,
  protein int,
  carbs int,
  fats int,
  link varchar(255),
  slink varchar(255),
  vegetarian bit,
  vegan bit,
  glutenfree bit,
  dairyfree bit,
  ketogenic bit
);
```
Assigned Weekly Meals:
- email: email of account being assigned
- mid: recipe id
- expire: string containing UTC date that meal should be replaced (Midnight of day that meal was assigned)
- mindex: index of day that meal is assigned to (Sunday=0, Monday=1, etc.)
```
CREATE TABLE UserMeal (
  email varchar(255) NOT NULL,
  mid int NOT NULL,
  expire varchar(255) NOT NULL,
  mindex int NOT NULL
);
```
FitBit Data:
- email: email of iDiet account
- accessKey: access_key needed to connect to FitBit Web API
- caloriesBurned: number of calories burned in FitBit Account
- steps: number of steps taken in FitBit Account
- distance: distance (in feet) travelled in FitBit Account
```
CREATE TABLE FitBit (
  email varchar(255) UNIQUE NOT NULL,
  accessKey varchar(255) NOT NULL,
  caloriesBurned int,
  steps int,
  distance int
);
```

Liked and Disliked Meals:
- email: email of iDiet account
- mid: recipe ID of meal that was liked/disliked
```
CREATE TABLE Likes (
  email varchar(255) NOT NULL,
  mid int NOT NULL
);
CREATE TABLE Dislikes (
  email varchar(255) NOT NULL,
  mid int NOT NULL
);
```

