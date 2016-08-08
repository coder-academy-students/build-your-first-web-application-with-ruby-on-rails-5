# Build your first web application with Ruby on Rails 5
---

Brought to you by [Coder Factory Academy].

## Introduction
Ruby on Rails is a powerful web application framework specially designed to optimise developer productivity and happiness. This tutorial aims to teach you some Rails basics while creating a Diary app that you can use yourself!

## Outcomes
- Create a basic Ruby on Rails web app
- Learn the basics of Ruby on Rails

## Requirements
- Google Chrome or Firefox browser

## Prerequisite knowledge and skills
- None

---

## Tutorial

### Step 1: Create an app with Cloud 9
1. Create an account with C9

Cloud 9 is a cloud-based development environment. We will use it to minimise the setup time for this tutorial.

2. Create a new workspace with the starter app
3. Guide through files and folders

### Step 2: Create a homepage with Controllers and Views
1. Show the finished product
2. Briefly explain controllers/MVC

Ruby on Rails uses MVC architecture for its web applications. MVC stands for Model, View, Controller. The Ruby on Rails guides are an excellent resource and brief definitions of each of these from the guides are below:

> Model - the layer of the system responsible for representing business data and logic.

> View - displays information in a human readable format.

> Controller - receives specific requests for the application. This is different to routing which decides which controller receives which requests.

3. Generate a home controller

In the terminal (box at the bottom with the 'bash' label) run:

```
rails g controller home index
```

The main files to note from this are [controllers > home_controller.rb], [views > home > index.html.erb]

> Note: .erb is an extension to html which allows us to write Ruby code embedded in our HTML.

4. Start the server

To be able to view our web app we need to start a server. We need a server to serve us the pages we request. For this example, we can request to see the home#index page we just created through the url http://localhost:3000/home/index.

> Note: Here I refer to our page as home#index where 'home' identifies the controller name and 'index' identifies the controller action or page. You can have many actions within a controller.

To start the server, in the terminal run:

```
rails server
```

> Tip: You can also use 'rails s' for short.

The terminal will outline a few lines of code and provide you with the URL to see your app at. If it isn't working, double check that the server has finished starting up.

5. Edit view file to show finished product

Navigate to [app > views > home > index.html.erb] and replace the default code with:

``` ruby
PASTE CODE HERE
```

Go back to your page preview and refresh the page to see the changes. Now this isn't a homepage yet because we still have to go to /home/index in our URL. In the next section we will go over routing and how to manipulate our URLs.

### Step 3. Routing
1. What are routes?

Routes define which pages (or controller actions) URLs match to. If you navigate to [config > routes.rb], you'll notice that there will be a route for our home#index page. It should look like this:

```
get 'home/index'
```

2. Create a root route

There is one more step that we have to do to make it our 'root route' or our homepage. We can delete the generated line for our home#index route and replace it with:

``` ruby
root to: 'home#index'
```

> Optional: When we want to keep a line of code in our apps or write notes that a computer will skip we can use 'comments'. To 'comment out' code means to make it invisible to the program. A shortcut to make code commented out is: on mac 'CMD + /' or on linux/windows 'CTRL + /'. Give it a try!

### Step 4. Generating a Scaffold

We now want to create our actual diary entries. To do this we need a place to store our diary entries in the database, a model to put our logic, a controller to retreive the entries from the database and views to visualise the information.

1. What is a scaffold?

Generating a Ruby on Rails scaffold gives you a basic template for a new object. It creates a database object for you with certain attributes, a model, controller and view files. While it is not normally used at more advanced levels because it can give you a lot of unneeded code, its a great tool for beginners.

2. Plan our data

Before we tell our database schema (like a blueprint) what a Diary Entry looks like we have to plan out what information we want to store.

DiaryEntry
title:string
content:text
user:references

> Help: The word after the colon ':' defines the data type of that attribute. If we don't define the data type it becomes a string by default.

> string: any character up to 256 in length (eg. for username, password etc)

> text: any character up to 4294967296 in length (eg. for long-form text)

> references: this references the id of the model before the colon. In this case its adding a reference to which user is related to the DiaryEntry.

> See them all here: http://stackoverflow.com/questions/17918117/rails-4-list-of-available-datatypes (StackOverflow is a great resource for coding Q&A)

3. About the command

Our command to generate our scaffold is:

```
rails g scaffold DiaryEntry title:string content:text user:references
```

Let's dissect this...

rails: Begins most Ruby on Rails CLI (Command Line Interface) commands

g/generate: Signifies that we are generating some code. You can use either 'g' or 'generate' here.

scaffold: Signifies which type of generator to run. A scaffold generates a model, controller, view files, routes and tests.

DiaryEntry: What ever is placed after 'scaffold' becomes the model name. This must be singular and by convension is written in Camel Case with no spaces and capitals at the beginning of each word.

title/content/user: 'Title', 'content' and 'user' refer to attributes you are giving our model. Attributes are used to describe the model. If you were to create attributes for a human you may want to create some for height, age, nationality etc.

string/text/references: These define the type of attribute that they are attached to. There are several more types than we use here but a string is a sequence of characters up to 256 in amount, text acts the same except that you can use many more than 256 characters, references is a Rails way to set up a reference to another model. With user:references we can see which user is related to our Diary Entry.

4. Show the migration file/What is a migration?

Once we run a scaffold command, a migration file is created. The purpose of this file is to propose changes to our database schema. This means that it wants to change what our database looks like.

5. Remove scaffolds.scss

A scaffolds.scss is is generated with each scaffold. It gives some basic styles but also conflicts with many css frameworks. We will delete it for this app. You can find it in [app > assets > stylesheets > scaffolds.scss].

6. Migrate your changes

To apply the changes to our database, in our terminal run:

```
rails db:migrate
```

### Step 5. More Routing
1. Change root route to new diary entry

2. Explain what resources actually gives us and compare to home#index route

In our routes file you will notice
```ruby
resources :diary_entry
```
This sets up the default Rails routes for our 'index', 'new', 'create', 'update', 'edit' and 'destroy' actions.

### Step 6. Basic Authentication with Devise
1. What is authentication?

Authentication let's users log in to see content only they should see. Think about logging in to see your social media or email account. With our Diary app, we want to authenticate users so that they only see their diary entries and no one elses.

2. How does devise work?

Devise is a Ruby Gem that gives us all the code for authentication out of the box. Authentication is difficult functionality to get right by yourself. Open source projects like Devise are a great resource to save developers time and for security. Devise has been peer reviewed by thousands of developers, it isn't perfect but its a very good solution. To install select the terminal and run:

```
rails g devise:install
rails g devise User
rails db:migrate
```

This will install Devise and add a user model to our database so we can record who has an account with our app.

3. Apply auth for all pages

At the moment a non-signed in user can still see every page. Let's go to [app > controllers > application_controller.rb] and add:

```ruby
before_action :authenticate_user!
```

Now before a user hits any of our pages, devise will check if they are logged in or not.

### Step 7. Finishing touches
1. Get rid of user ref in DiaryEntry form
2. Remove :registerable?
3. Time localisation


   [Coder Factory Academy]: <https://www.coderfactoryacademy.edu.au>
   [Ruby on Rails guides]: <http://guides.rubyonrails.org/>

