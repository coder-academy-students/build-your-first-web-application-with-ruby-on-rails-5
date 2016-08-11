# Build your first web application with Ruby on Rails 5

Brought to you by [Coder Factory Academy].

## Introduction
Ruby on Rails is a powerful web application framework specially designed to optimise developer productivity and happiness. This tutorial aims to teach you some Rails basics while creating a Diary app that you can use yourself!

## Outcomes
- Create a basic Ruby on Rails web app
- Learn the basics of Ruby on Rails

## Requirements
- Google Chrome or Firefox browser
- Internet connection

## Prerequisite knowledge and skills
- None

---

## Tutorial

### Step 1: Create an app with Cloud 9
1. Create an account with C9

Cloud 9 is a cloud-based development environment. We will use it to minimise the setup time for this tutorial.

2. Create a new workspace with the starter app

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/newworkspace-compressor.png "New C9 workspace")

3. Interface Tour

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/c9layout-compressor.png "C9 workspace tour")

4. Show the final product

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/finalproduct2-compressor.png "Final product")

> If you want to follow along with the theme we use for C9 follow the steps in this screenshot:

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/c9preferences-compressor.png "Theme preferences")

### Step 2: Create a homepage with Controllers and Views
1. Briefly explain controllers/MVC

Ruby on Rails uses MVC architecture for its web applications. MVC stands for Model, View, Controller. The Ruby on Rails guides are an excellent resource and brief definitions of each of these from the guides are below:

> Model - the layer of the system responsible for representing business data and logic.

> View - displays information in a human readable format.

> Controller - receives specific requests for the application. This is different to routing which decides which controller receives which requests.

2. Generate a home controller

In the terminal (box at the bottom with the 'bash' label) run:

```
bundle
```

You will need to run `bundle` in the terminal to update the app's dependencies (open source code we've borrowed to make our app). The output should look similar to this:

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/updatedependencies-compressor.png "Bundle")


```
rails g controller home index
```

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/newcontrollercmd-compressor.png "Generate controller")

The main files to note from this are `controllers > home_controller.rb`, `views > home > index.html.erb`

> Note: .erb is an extension to html which allows us to write Ruby code embedded in our HTML.

3. Start the server

To be able to view our web app we need to start a server. We need a server to serve us the pages we request. For this example, we can request to see the home#index page we just created through the url https://`<your-app-name>`/home/index.

> Note: Here I refer to our page as home#index where 'home' identifies the controller name and 'index' identifies the controller action or page. You can have many actions within a controller.

To start the server press 'Run Project' in the navbar.

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/runproject-compressor.png "Run project")

The terminal will outline a few lines of code and provide you with the URL to see your app at. If it isn't working, double check that the server has finished starting up.

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/server-compressor.png "Server output")

4. Edit view file to show finished product

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/filehomeindex-compressor.png "Home")

Navigate to `app > views > home > index.html.erb` and customise the generated code to whatever you want e.g.

```html
<h1>Welcome to my diary</h1>
<p>I use this to write about my thoughts.</p>
```

Go back to your page preview and refresh the page to see the changes. Now this isn't a homepage yet because we still have to go to /home/index in our URL. In the next section we will go over routing and how to manipulate our URLs.

### Step 3. Routing
1. What are routes?

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/fileroutes-compressor.png "Routes")

Routes define which pages (or controller actions) URLs match to. If you navigate to `config > routes.rb`, you'll notice that there will be a route for our home#index page. It should look like this:

```
get 'home/index'
```

2. Create a root route

There is one more step that we have to do to make it our 'root route' or our homepage. We can delete the generated line for our home#index route and replace it with:

``` ruby
root to: 'home#index'
```

> Optional: When we want to keep a line of code in our apps or write notes that a computer will skip we can use 'comments'. To 'comment out' code means to make it invisible to the program. A shortcut to make code commented out is: on mac 'CMD + /' or on linux/windows 'CTRL + /'. Give it a try!

### Step 4. Basic Authentication with Devise
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

3. Apply authentication for all pages

At the moment a non-signed in user can still see every page. Let's go to `app > controllers > application_controller.rb` and add on the second line:

```ruby
before_action :authenticate_user!
```

Now before a user hits any of our pages, devise will check if they are logged in or not.

### Step 5. Generating a Scaffold

We now want to create our actual diary entries. To do this we need a place to store our diary entries in the database, a model to put our logic, a controller to retreive the entries from the database and views to visualise the information.

1. What is a scaffold?

Generating a Ruby on Rails scaffold gives you a basic template for a new object. It creates a database object for you with certain attributes, a model, controller and view files. While it is not normally used at more advanced levels because it can give you a lot of unneeded code, its a great tool for beginners.

2. Plan our data

Before we tell our database schema (like a blueprint) what a Diary Entry looks like we have to plan out what information we want to store.

`DiaryEntry title:string content:text user:references`

> Help: The word after the colon ':' defines the data type of that attribute. If we don't define the data type it becomes a string by default.

> string: any character up to 256 in length (eg. for username, password etc)

> text: any character up to 4294967296 in length (eg. for long-form text)

> references: this references the id of the model before the colon. In this case its adding a reference to which user is related to the DiaryEntry.

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/DataTypesimg-compressor.png "Data Types")

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

Once we run a scaffold command, a migration file is created. The purpose of this file is to propose changes to our database schema. This means that it wants to change what our database looks like. The below code is what our scaffold will generate to create the Diary Entry database table.

```ruby
1  class CreateDiaryEntries < ActiveRecord::Migration[5.0]
2    def change
3      create_table :diary_entries do |t|
4        t.string :title
5        t.text :content
6        t.references :user, foreign_key: true
7
8        t.timestamps
9      end
10   end
11 end
```

5. Remove scaffolds.scss

A scaffolds.scss is is generated with each scaffold. It gives some basic styles but also conflicts with many css frameworks. We will delete it for this app. You can find it in `app > assets > stylesheets > scaffolds.scss`.

6. Migrate your changes

To apply the changes to our database, in our terminal run:

```
rails db:migrate
```

### Step 6. Creating a Layout

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/finalproduct2layout-compressor.png "Final Product Layout")

To get our sidebar, navbar and main content area together, we will need to set up the layout.

1. Navbar

Most websites have a navbar across the top of their pages to aid the user's navigation. Since our navbar will have a lot of code in it, let's create a separate file for it. We will use code from Bootstrap, a popular css framework. Basically Bootstrap gives us pre-designed components that we can use and customise for our designs.

Right click the folder `app > views > layouts` and select 'New File'.
Save this file as `_navbar.html.erb`.

Copy & paste this code into your new file:

```
<nav class="navbar navbar-default navbar-fixed-top">
  <div class="container-fluid">
    <div class="navbar-header">
      <a class="navbar-brand" href="/">My Diary</a>
    </div>

    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav navbar-right">
        <% if user_signed_in? %>
          <li><%= link_to '<i class="fa fa-plus fa-2x" aria-hidden="true"></i>'.html_safe, new_diary_entry_path %></li>
          <li><%= link_to '<i class="fa fa-sign-out fa-2x" aria-hidden="true"></i>'.html_safe, destroy_user_session_path, method: :delete %></li>
        <% end %>
      </ul>
    </div>
  </div>
</nav>
```

Since this code is in a separate file, let's render it onto our main template.

Navigate to `app > views > layouts > application.html.erb`

Copy & paste `<%= render 'layouts/navbar' %>` directly under `<body>`.

> Info: This file is the base template that all views get rendered into. Here we can add things to the `<head></head>` of our website such as metatags for better SEO or Google Analytics tracking codes. The code in our other view files will get rendered where `<%= yield %>` is within the `<body></body>`.

2. Sidebar

Our sidebar is going to keep track of all of our past diary entries. Let's again navigate to `app > views > layouts`, right click the folder name and select 'New File'. Save this file as `_entry_list.html.erb`.

In our new file let's paste this code:

```
<% if @diary_entries.present? %>
    <% @diary_entries.each do |diary_entry| %>
        <a href="<%= diary_entry_path(diary_entry) %>">
            <div class="row">
                <div class="col-sm-12 entry_listing">
                    <div class="row">
                        <div class="col-sm-3 text-center">
                            <h1><%= diary_entry.updated_at.strftime("%d") %></h1>
                            <h4><%= diary_entry.updated_at.strftime("%b %Y") %></h4>
                        </div>
                        <div class="col-sm-9">
                            <h4><%= truncate(diary_entry.title, length: 30, separator: ' ') %></h4>
                            <p class="grey-text"><%= truncate(diary_entry.content, length: 100, separator: ' ') %></p>
                        </div>
                    </div>
                </div>
            </div>
        </a>
    <% end %>
<% end %>
```

Again, let's render this file on our main layout in `app > views > layouts > application.html.erb`.

Copy & paste this code underneath our `<%= render 'layouts/navbar' %>`

```ruby
<div class="entry_list xs-hidden col-sm-3 col-md-4">
    <%= render 'layouts/entry_list' %>
</div>
```

3. Yield

Lastly, we want to make sure our other pages fit properly into our `application.html.erb` template.

Let's wrap `<%= yield %>` with a `<div>` like this:

```ruby
<div class="main-section col-sm-offset-3 col-sm-9 col-md-offset-4 col-md-8">
    <%= yield %>
</div>
```

> A yield is where all our other pages will slot into e.g. when we open the new diary entry page, the code within `app > views > diary_entries > new.html.erb` will replace `<%= yield %>`. When we wrap our div with classes around content we are giving them custom css I have pre-prepared. We will get more into css in further courses.

### Step 7. More Routing

1. Change root route to new diary entry

Change your `config > routes.rb` file line 7 to this:

![alt text](https://github.com/coder-factory-academy/build-your-first-web-application-with-ruby-on-rails-5/raw/master/app/assets/images/tutorial-images/rootroutes-compressor.png "Final Product Layout")

2. Explain what resources actually gives us and compare to home#index route

In our routes file you will notice
```ruby
resources :diary_entries
```
This sets up the default Rails routes for our 'index', 'new', 'create', 'update', 'edit' and 'destroy' actions.

### Step 8. CRUD pages

CRUD refers to Create, Read, Update and Destroy. These are the actions that our diary entry scaffold set up for us by default. You will notice that the routes from the previous section match these actions.

Now that we have our layout set up, we need to make sure the pages that get rendered in our yield are correct.

1. Diary Entry Form

Similar to our navbar and entry_list sidebar, our form is in a separate file to the main template.

Navigate to the diary entry form at `app > views > diary_entries > _form.html.erb`

What we currently have is the default generated form, let's replace it with this code to make it a bit nicer.

```ruby
<%= simple_form_for(@diary_entry) do |f| %>
  <%= f.error_notification %>

  <div class="form-inputs">
    <%= f.input :title %>
    <%= f.input :content, as: :text, input_html: {rows: 15} %>
  </div>

  <div class="form-actions">
    <%= f.button :submit, class: "btn btn-green" %>
  </div>
<% end %>
```

The other thing you may notice is that we removed the line of code to choose which user is assigned to a post. We want only the user who creates the post to be assigned to it so let's assign that relationship automatically.

First, navigate to `app > models > diary_entry.rb`

We want to create a relationship in the data between our users and diary entries with `belongs_to :user`

Change your `diary_entry.rb` file to look like this:

```ruby
class DiaryEntry < ApplicationRecord
  belongs_to :user
end
```

Now that our database is set up to know that diary entries can be related to a user, let's make that association be created whenever a user creates a diary entry.

Navigate to `app > controllers > diary_entries_controller.rb`

Scroll down to the `def create` block. This action or method defines what happens when we create a diary entry. We want to assign the user to our newly created diary entry with this line of code `@diary_entry.user_id = current_user.id`. It basically grabs the id of the user currently logged in and then assigns that value into our user_id attribute of the diary entry.

Your `create` method should now look like this:

```ruby
def create
    @diary_entry = DiaryEntry.new(diary_entry_params)
    @diary_entry.user_id = current_user.id

    respond_to do |format|
      if @diary_entry.save
        format.html { redirect_to @diary_entry, notice: 'Diary entry was successfully created.' }
        format.json { render :show, status: :created, location: @diary_entry }
      else
        format.html { render :new }
        format.json { render json: @diary_entry.errors, status: :unprocessable_entity }
      end
    end
end
```

You can create diary entries now and they will only belong to the user who is logged in.

Click on the `+` in the navbar and create your first diary entry.

Let's spice up the page that actually shows the diary entry once it has been completed. This file is fitting called the 'show' page.

Navigate to `app > views > diary_entries > show.html.erb`

Replace the default code with this:

```
<em><%= @diary_entry.updated_at.strftime("%A, %d %b %Y %l:%M %p") %></em>
<h1><%= @diary_entry.title %></h1>

<p>
  <%= @diary_entry.content %>
</p>

<%= link_to 'Edit', edit_diary_entry_path(@diary_entry), class: 'btn btn-green btn-sm' %>
```

Here we are simplifying it a little to just the main attributes we want to show from a diary entry. We've also added a few nice features such as `.strftime("%A, %d %b %Y %l:%M %p")` to format our time nicely. Try deleting that method to see what the default time stamp looks like.

> Hint: If the time isn't showing up correctly it is probably in the wrong timezone. We will look at how to change that in later courses.



   [Coder Factory Academy]: <https://www.coderfactoryacademy.edu.au>
   [Ruby on Rails guides]: <http://guides.rubyonrails.org/>
