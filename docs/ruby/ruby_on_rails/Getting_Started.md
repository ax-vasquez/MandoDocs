# [Getting Started with Rails](http://guides.rubyonrails.org/getting_started.html)
Rails is a web application framework running on the Ruby programming language. _Please note that the guide used in writing these notes uses some older modules, so syntax may differ_.

### What this Guide Covers
- _How to install Rails, create a new Rails application, and connect your application to a database_
- _The general layout of a Rails application_
- _The basic principles of MVC (Model, View, Controller) and RESTful design_
- _How to quickly generate the starting pieces of a Rails application_

### Guide Assumptions
- This guide is designed for beginners who want to get started with a Rails application from started
  - _No prior experience with Rails is required for this_
  - You WILL need to be familiar with the concepts in Ruby

## What is Rails?
- Rails is a web application development framework written in the Ruby programming language
  - Designed to make programming web applications
    - It does so by _making assumptions about what every developer needs to get started_
  - Allows you to write less code
  - Experienced developers claim that it also makes web application development more fun too
- Rails is an _**opinionated software**_
  - It makes the assumption that _there is a "best" way to do things_
    - Rails designed to encourage doing things the "best" way
    - In some cases, it also discourages alternatives to the best way
  - **If you learn "The Rails Way", you will discover a tremendous increase in productivity**
    - NEVER BRING OLD HABITS FROM OTHER LANGUAGES INTO RAILS

### The Rails Philosophy
There are two major guiding principles in the Rails Philosophy:
1. **Don't Repeat Yourself** (DRY)
    - "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system"
    - By not writing the same code over and over, your code is much more maintainable and less buggy
2. **Convention over Configuration**
    - Rails defaults to use the conventions it configures to be the "best" way to do things
    - This removes any ambiguity about how to structure any given Ruby on Rails application

## Creating a New Rails Project
The best way to read this guide is step-by-step. _All steps are essential to run this example application - **no additional code or steps are needed**

This guide teaches you how to create a Rails project called `blog`, a (very) simple weblog.

### 0 - Install Rails
The first thing you need to do is install Ruby and Ruby on Rails on your system, if you haven't already. Additionally, you will need to have SQLite3 installed for this guide.

**Verify Ruby is Installed**
Note that your output may differ
```bash
ruby -v
ruby 2.4.0p0 (2016-12-24 revision 57164) [x86_64-darwin16]
```
**You should have AT LEAST version `2.2.2` of Ruby installed on your system to use Rails**

### 1 - Install SQLite3
Most popular UNIX-like OSes ship with acceptable versions of SQLite3 installed. Verify that it is installed correctly and in your PATH:
```bash
sqlite3 --version
```
- If installed, you should see the version number output

### 2 - Install Rails
To install Rails, use the `gem install` provided by RubyGems:
```bash
gem install rails
```
_This can take a couple of minutes to complete_

**Verify that Rails is Installed**
```bash
rails --version
```
If you see something like "Rails 5.1.1", then you are good to go

### 3 - Create the Blog Application
Rails comes with a number of scripts called generators that are designed to make your development life easier by creating everything that's necessary to start working on a particular task.
- One of these is the new application generator
  - Provides you with the foundation of a fresh Rails application so you don't have to write it yourself

**Using the New Application Generator**
Be sure to navigate to the parent directory you want to create the new project in before running this command.
```bash
rails new blog
```
- This will create a new directory called `blog` _within the current working directory_

Navigate to the newly-created directory:
```bash
cd blog
```
- This directory contains a number of auto-generated files and folders
  - These make up the structure of a _proper_ Rails application
  - Most of the work in this tutorial occur within the `app` directory

**Structure of a Rails Application**
| File / Folder | Purpose | 
| ------------- | ------- |
| `app/` | Contains the controllers, models, views, helpers, mailers, channels, jobs and assets for your application |
| `bin/` | Contains the rail script that starts your app and can contain other scripts you use to setup, update, deploy or run your application |
| `config/` | Configure your application's routes, database, and more. This is covered in more detail in [Configuring Rails Applications](http://guides.rubyonrails.org/configuring.html) |
| `config.ru` | Rack configuration for Rack based servers used to start the application. For more information, see the [Rack website](https://rack.github.io/) |
| `db/` | Contains your current database schema, as well as the database migrations |
| `Gemfile`, `Gemfile.lock` | These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem. For more information about Bundler, see the [Bundler website](https://bundler.io/) |
| `lib/` | Extended modules for your application |
| `log/` | Application log files |
| `package.json` | This file allows you to specify what npm dependencies are needed for your Rails application. This file is used by Yarn. For more information about Yarn, see the [Yarn website](https://yarnpkg.com/lang/en/) |
| `public/` | The only folder seen by the world as-is. Contains static files and compiled assets |
| `Rakefile` | This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of Rails. _Rather than changing `Rakefile`, you should add your own tasks by adding files to the `lib/tasks` directory of your application._ |
| `README.md` | This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on. |
| `test/` | Unit tests, fixtures, and other test apparatus. These are covered in [Testing Rails Applications](http://guides.rubyonrails.org/testing.html) |
| `tmp/` | Temporary files (like cache and pid files) |
| `vendor/` | A place for all third-party code. In a typical Rails application this includes vendored gems |
| `.gitignore` | This file tells git which files (or patterns) it should ignore. [See Github - Ignoring Files](https://help.github.com/articles/ignoring-files) for more information |
| `ruby-version` | This file contains the default Ruby version |

### 4 - Hello, Rails!
To start, just get some text on the screen quickly. To do this, you first need to get your Rails application server running

#### Start a Webserver on your Machine
Be sure you are in the `blog` directory, then run the following:
```bash
bin/rails server
```
Once the Puma server is up and running, navigate to http://localhost:3000/
- If you see the "Yay! You're on Rails!" page, then it worked!
  - _This page serves as a smoke test for new Rails applications_
  - It makes sure you have your software configured correctly enough to serve a page

#### Say "Hello, Rails"
In order to say "Hello", you'll need to create at a minimum a _controller_ and a _view_.
- **Controller**
  - A controller's purpose is to receive specific requests for the application
  - _Routing_ decides which controller receives which requests
    - Often, there is more than one route to each controller
  - Different routes can be served by different _actions_
    - Each action's purpose is to collect information to provide it to a view
- **View**
  - A view's purpose is to display this information in a human readable format
  - _**An important distinction to make is that the CONTROLLER, not the view, is where information is collected**_
    - The view simply displays the information contained in the controller in a readable way
  - By default, view templates are written in a language called eRuby (for "Embedded Ruby")
    - This is processed by the request cycle in Rails before being sent to the user

**Creating a New Controller**
Rails provides a "controller" generator - for this tutorial, you'll tell it to create a controller called "Welcome" with an action called index, like so:
```bash
bin/rails generate controller Welcome index
```
- Rails will create several files and a route for you
  - Most importantly for this tutorial:
    - `app/controllers/welcome_controller.rb` (the controller)
    - `app/views/welcome/index.html.erb` (the view)

Open `index.html.erb` in your text editor of choice, then delete all of the code in it. Replace it with the following code:
```html
<h1>Hello, Rails!</h1>
```

#### Setting the Application Home Page
Now, you will tell Rails where your actual home page is located. To do this, open the application's _routing_ file, `config/routes.rb`. By default, it should have the following content:
```ruby
Rails.application.routes.draw do
  get 'welcome/index'
 
  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
end
```
- Notes on the `routes.rb` file:
  - The routing file holds entries in a special [DSL (Domain-Specific Language)](https://en.wikipedia.org/wiki/Domain-specific_language) that tells Rails how to connect incoming messages to controllers and actions

Add the line, `root 'welcome#index'`, to `routes.rb`:
```ruby
Rails.application.routes.draw do
  get 'welcome/index'
 
  root 'welcome#index'
end
```
- `root 'welcome#index'`
  - Tells Rails to map requests to the root of the application to the welcome controller's index action
- `get 'welcome/index'`
  - Tells Rails to map requests to http://localhost:3000/welcome/index to the welcome controller's index action

Now, start the server again using `bin/rails server` while in the application directory, then navigate to http://localhost:3000. You should now see the "Hello, Rails!" page.

### 5 - Getting Up and Running
This section will guide you on creating something with a bit more substance.

You will now create a new _resource_ in your blog application. A resource is the term used for a collection of similar objects, such as articles, people or animals. You can Create, Read, Update and Destroy items for a resource - these operations are referred to as CRUD operations.
- Rails provides a `resources` method
  - Can be used to declare a standard REST resource

For this tutorial, add the _article resource_ to `config/routes.rb` so that it looks like the following:
```ruby
Rails.application.routes.draw do
  get 'welcome/index'
 
  resources :articles
 
  root 'welcome#index'
end
```
- Verify this worked by running `bin/rails routes`
  - This lists the defined routes for all the standard RESTful actions
  - The meaning of the prefix column (and other columns) will be discussed later
  - For now, notice that _Rails has inferred the singular form `article` and makes meaningful use of the distinction_

#### Laying Down the Groundwork
First, you need a place within the application to create a new application. 
- Since the route for this location is already defined, you should use `/articles/new`
  - With the route already defined, requests can now be made `/articles/new` in the application

**Generating a New Controller**
Now, start the server again and navigate to https://localhost:3000/articles/new; you should get an error, `uninitialized constant ArticlesController`
- This error occurs because _the route needs to have a controller defined in order to serve the request_
- The solution to this is simple:
  - Create a controller called `ArticlesController`
  - You can do this by running this command:
```bash
bin/rails generate controller Articles
```
- This will generate a file, `articles_controller.rb`
  - This file is where you will define methods that will become actions for this controller
  - These actions will perform CRUD operations on the articles within our system
    - Only public methods can be actions for controllers

Refresh the page at http://localhost:3000/articles/new; you should now see a different error, `The action 'new' could not be found for ArticlesController`
- This error indicates that Rails cannot find the new action inside the `ArticlesController` that you just generated
  - This is because controllers are empty by default when generated by Rails
    - Unless you tell it your desired actions in the generation process

**Defining an Action**
To define an action inside a controller, all that's needed is to define a new method inside the controller. Modify the code of `articles_controller.rb` so that it looks like the following:
```ruby
class ArticlesController < ApplicationController
  def new
  end
end
```
- Now, refresh the page at http://localhost:3000/articles/new
- You should get an error something along the lines of `UnknownFormat in ArticlesController#new`
  - This error occurs because Rails expects plain actions like this one to have views associated with them to display their information
  - With no view available, Rails will raise an exception
- The simplest template that woudl work to fix this error would be located at `app/views/articles/new.html.erb`
  - _The extension of the file name is important!!!!_
    - The first part of the extension _format_ of the template
    - The second extension is the _handler_ that will be used to render the template
  - Rails will attempt to find a template to find a template called `articles/new` within `app/views` for the application
  - The format for this template can only be HTML
    - The default handler for HTML is `erb`
    - Rails is capable of using other handlers for other formats
      - `builder` handler is used to build XML
      - `coffee` handler uses CoffeeScript to build JavaScript templates

**Creating a New View**
As mentioned before, you need to create a file, `new.html.erb`, in the `app/views/articles/` directory. Provide the following as its contents:
```html
<h1>New Article</h1>
```

#### The First Form
To create a form within this template, you will use a _form builder_. The primary form builder for Rails is provided by a helper method called `form_with`. To use this method, add the following code into `new.html.erb`:
```html
<%= form_with scope: :article, local: true do |form| %>
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>
 
  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>
 
  <p>
    <%= form.submit %>
  </p>
<% end %>
```
- Refresh the page at http://localhost:3000/articles/new
- You should now see a title and body text field, as well as a "Save Article" button
  - Simple as that!

However, there is a problem with this form: the `action` attribute for the form is pointing at `articles/new`. This is bad since this is the same page you are currently on - and that route is reserved for creating new articles only!
- The form needs to use a different URL in order to go somewhere else
- This is done simply with the `:url` option of `form_with`
  - Typically in Rails, the action used for new form submissions like this is called "create"
    - So, the form should be pointed to that action

Edit the `form_with` line in `new.html.erb` so that it looks like this:
```html
<%= form_with scope: :article, url: articles_path, local: true do |form| %>
```
- Here, the `articles_path` helper is passed to the `:url` option
  - To see what Rails will do with this, run the `bin/rails routes` command
  - The `articles_path` helper tells Rails to point the form to the URI Pattern associated with the `articles` prefix
    - The form will send a POST request to that route by default
    - This is associated with the `create` action of the current controller (the `ArticlesController`)

Now, fill in the fields and click the "Save Article" button; you should get an error, `The action 'create' could not be found for ArticlesController`

#### Creating Articles
To make the "Unkown Action" error go away, define a `create` action within the `ArticlesController` class in `articles_controller.rb` under the `new` action, as show below:
```ruby
class ArticlesController < ApplicationController
  def new
  end
 
  def create
  end
end
```
- _This alone does not fix the aforementioned error_
  - If you refresh the page now, you will still get the error since this action doesn't do anything yet (e.g. "empty action")
  - You still need to specify how the response should be
    - In this case, the `create` action should save the new article to the database

**Form Submission and Field Parameters**
When a form is submitted, the fields of the form are sent to Rails as _parameters_. These parameters can be referenced inside the controller actions, typically to perform a particular task. Here is an example of what these parameters look like, done to the `create` method (do the same to your code):
```ruby
def create
  render plain: params[:article].inspect
end
```
- The `render` method is taking a very simple hash with a key of `:plain` and value of `params[:article].inspect`
  - The `params` method is the object which represents the parameters [fields] coming in from the form
    - This method returns an `ActionController::Parameters` object
      - This allows you to access the keys of the hash using either strings or symbols
    - In this guide, the parameters from the form are the only ones that matter

**_Now_**, the previous "Unknown Action" should go away; go back to http://localhost:3000/articles/new and fill out the title and text fields, then click "Save Article". You should now see some output similar to the following on the resulting page (it won't be pretty, though...):
```text
<ActionController::Parameters {"title"=>"Test", "text"=>"This is just a test, don't panic."} permitted: false>
```

#### Creating the Article Model
_Models in Rails use a singular name_. Their corresponding databases use a plural name. Rails provides a generator for creating models - _most Rails developers tend to use this when creating new models_. Create an article model using the following command:
```bash
bin/rails generate model Article title:string text:text
```
- This command tells Rails that you want an `Article` model
  - Has a _title_ attribute of type string
  - Has a _text_ attribute of type text
  - These attributes are automatically added to the `articles` table in the database
- Rails will create several files
  - For now, only worry about:
    - `app/models/article.rb` and 
    - `db/migrate/XXXXXXXXXXXXXX_create_articles.rb` (the actual name will have na timestamp in all numbers instead of 'X's)

#### Running a Migration
Migrations are Ruby classes that are designed to make it simple to create and modify database tables.
- Rails uses rake commands to run migrations
  - It is possible to undo a migration after it's been applied to your database
- Migration file names include a time stamp to ensure that they're processed in the order they were created

Open the file, `XXXXXXXXXXXXXX_create_articles.rb` (remember, the X's are a timestamp in all-number format); here is what you will find:
```ruby
class CreateArticles < ActiveRecord::Migration[5.0]
  def change
    create_table :articles do |t|
      t.string :title
      t.text :text
 
      t.timestamps
    end
  end
end
```
- This migration creates a method named `change`
  - This method will be called when you run this migration
  - The action defined in this method is also reversible
    - This means that Rails knows how to reverse the change made by this migration
- When you run this migration:
  - It will create an `articles` table with one string column and a text column
  - It also creates two timestamp fields to allow Rails to track article creation and update times
- For more information on migrations, see [Active Record Migrations](http://guides.rubyonrails.org/active_record_migrations.html)

Use the following command to run the migration:
```bash
bin/rails db:migrate
```

You should see output similar to the following:
```bash
== 20180509044214 CreateArticles: migrating ===================================
-- create_table(:articles)
   -> 0.0022s
== 20180509044214 CreateArticles: migrated (0.0023s) ==========================
```
- Since you are working in the development environment by default, this command applies to the database defined in the `development` section of the app's `config/database.yml` file
  - To execute migrations in other environments (for instance, production), explicitly-pass it when invoking the command:
    - `bin/rails db:migrate RAILS_ENV=production`

#### Saving Data in the Controller
To support article saving, edit the `ArticlesController`'s `create` action to use the new `Article` model to save the data in the database. Edit `create` action in `articles_controller.rb` so that it looks like the following:
```ruby
def create
  @article = Article.new(params[:article])
 
  @article.save
  redirect_to @article
end
```
- Breakdown
  - Every Rails model can be initialized with its respective attributes
    - This is done with the line `@article = Article.new(params[:article])`
  - `@article.save` is responsible for saving the model in the database
    - This returns a boolean to indicate whether the article was saved or not
  - Finally, the user is redirected to the `show` action, which will be discussed later
- _Why is the "A" capitalized in `Article.new(...)`?_
  - This is because you are referring to a class name (`Article`) - all class names start with a capital letter in ruby

**Whitelisting Controller Parameters**
If the server isn't already running, start it (using `bin/rails server` in the `root` of the blog project) and then go back to http://localhost:3000/articles/new. Try to submit an article. This time, you should get error, `ActiveModel::ForbiddenAttributesError`
- This error is due to the line `@article = Article.new(params[:article])`
  - This is a result of one of Rails several security features (this one is called [Strong Parameters](http://guides.rubyonrails.org/action_controller_overview.html#strong-parameters))
    - Strong Parameters require you to tell Rails exactly which parameters are allowed into your controller actions
- To get around this error, you must whitelist your controller parameters to prevent wrongful mass assignment (which can be unfortunate accidents, or intentional attacks)
  - In this case, you want to allow both the required `title` and `text` parameters for valid use of `create`

To whitelist these parameters, change one line in the `create` action of the `articles_controller.rb` file:
```ruby
@article = Article.new(params.require(:article).permit(:title, :text))
```
- Note that this is often factored out into its own method
  - Allows it to be reused by multiple actions in the same controller (for example `create` and `update`)
- Beyond the mass assignment issues, _**the method is often made private to prevent being called from outside its intended context**_

Here is the complete result:
```ruby
def create
  @article = Article.new(article_params)
 
  @article.save
  redirect_to @article
end
 
private
  def article_params
    params.require(:article).permit(:title, :text)
  end
```

#### Showing Articles
If you submit your form now, Rails will complain about not finding the `show` action. _Add the `show` action to prevent this_. As you have already seen in the output of `bin/rails routes`, the route for the `show` action is as follows:
```bash
article GET    /articles/:id(.:format)      articles#show
```
- The syntax `:id` tells Rails that this route expects an `:id` parameter
  - In this case, it will be the ID of the article

**Note on Standard Practice**
- It's standard practice to place the CRUD actions in _each controller_ in the _following order_:
  - `index`
  - `show`
  - `new`
  - `edit`
  - `create`
  - `update`
  - `destroy`
  - You can use any order you want, but keep in mind that these are public methods
    - They must be declared before all private methods

Add the action `show` in `articles_controller.rb` and its respective view:
```ruby
class ArticlesController < ApplicationController
  def show
    @article = Article.find(params[:id])
  end
 
  def new
  end
 
  # snippet for brevity
```
- Some things to notes:
  - You are using `Article.find` to find the article you are interested in
    - It passes in `params[:id]` to get the `:id` parameter from the request
  - You are also using an instance variable (prefixed with `@`) to hold a reference to the article object
    - This is done because Rails will pass all instance variables to the view

Create a new file at `app/views/articles/` called `show.html.erb` with the following content:
```html
<p>
  <strong>Title:</strong>
  <%= @article.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @article.text %>
</p>
```
- You should now be able to create new articles
  - Visit http://localhost:3000/articles/new to try it out

#### Listing All Articles
You still need a way to list all articles - recall that the route for this per the output of `bin/rails routes` is:
```bash
articles GET    /articles(.:format)          articles#index
```

**Update the `index` Method**
Add the following `index` action for that route inside `articles_controller.rb`; typically, the convention is to place it as the first method:
```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end
 
  def show
    @article = Article.find(params[:id])
  end
 
  def new
  end
 
  # snippet for brevity
```

You also need to create the corresponding view for this action, `index.html.erb` (in `app/views/articles/`):
```html

<h1>Listing articles</h1>
<table>
  <tr>
    <th>Title</th>
    <th>Text</th>
    <th></th>
  </tr>
 
  <% @articles.each do |article| %>
    <tr>
      <td><%= article.title %></td>
      <td><%= article.text %></td>
      <td><%= link_to 'Show', article_path(article) %></td>
    </tr>
  <% end %>
</table>
```

#### Adding Links
You can now create, show and list articles. Modify `app/views/welcome/index.html.erb` as follows:
```html
<h1>Hello, Rails!</h1>
<%= link_to 'My Blog', controller: 'articles' %>
```
- The `link_to` method is one of Rails` built-in view helpers
  - It creates a hyperlink based on text to display where to go (in this case, the path for the articles)

Now add another link to the `new.html.erb` to go back to the `index` action:
```html
<%= form_with scope: :article, url: articles_path, local: true do |form| %>
  ...
<% end %>
 
<%= link_to 'Back', articles_path %>
```

Finally, add a link to the `app/views/articles/show.html.erb` template to go back to the `index` action as well (this will allow people viewing a single article to go back to view the whole list again):
```html
<p>
  <strong>Title:</strong>
  <%= @article.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @article.text %>
</p>
 
<%= link_to 'Back', articles_path %>
```
- Notes
  - If you want to link to an action in the same controller, _you don't need to specify the `:controller` option_
    - Rails uses the current controller by default
  - In development mode (which is the default setting), _Rails reloads your application with every browser request_
    - This is why there is no need to stop and restart the web server when a change is made

#### Adding Some Validation
The current form of the `app/models/article.rb` is as simple as it can be:
```ruby
class Article < ApplicationRecord
end
```
- Note that the `Article` class inherits from `ApplicationRecord`
  - `ApplicationRecord` inherits from `ActiveRecord::Base`
    - This class provides a lot of Rails functionality with models out-of-the-box:
      - Basic database CRUD operations
      - Data Validation
      - Sophisticated search support
      - Ability to relate multiple models to one another

**Validating Data**
Rails includes methods to help you validate data you send to models. Edit `app/models/article.rb` so that it looks like the following:
```ruby
class Article < ApplicationRecord
  validates :title, presence: true,
                    length: { minimum: 5 }
end
```
- Ensures that the title is _at least_ 5 characters long
- Rails can validate a variety of conditions in a model, including:
  - The presence or uniqueness of columns
  - The format of columns
  - The existence of associated objects
  - Validations are covered in detail in [Active Record Validations](http://guides.rubyonrails.org/active_record_validations.html)

**Handling Failed Validations**
With the validation in place, _you can call`@article.save` on an invalid article_ (it will return `false`). Look back at `app/controllers/articles_controller.rb`; notice that you are currently not checking the result of `@article.save` inside the `create` action.
- If `@article.save` fails in this situation, you need to show the form back to the user
- To do this, change the `new` and `create` actions inside `app/controllers/articles_controller.rb` so that it looks like the following:
```ruby
def new
  @article = Article.new
end
 
def create
  @article = Article.new(article_params)
 
  if @article.save
    redirect_to @article
  else
    render 'new'
  end
end
 
private
  def article_params
    params.require(:article).permit(:title, :text)
  end
```
- The `new` action is now creating a new instance variable called `@article`
- Notice in `create` action you are using `render` _instead of_ `redirect_to` when `save` returns `false`
  - The `render` method is used so that the `@article` object is passed back to the new template when it is rendered
    - This rendering is done in the same request as the form submission (whereas `redirect_to` tells the browser to issue another request)

**Informing the User of Failed Validations**
If you attempt to submit an article without a title now, you will simply be brought back to the create article page; This is not very useful validation. You need to tell the user something went wrong - modify `app/views/articles/new.html.erb` to check for error messages:
```html
<%= form_with scope: :article, url: articles_path, local: true do |form| %>
 
  <% if @article.errors.any? %>
    <div id="error_explanation">
      <h2>
        <%= pluralize(@article.errors.count, "error") %> prohibited
        this article from being saved:
      </h2>
      <ul>
        <% @article.errors.full_messages.each do |msg| %>
          <li><%= msg %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
 
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>
 
  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>
 
  <p>
    <%= form.submit %>
  </p>
 
<% end %>
 
<%= link_to 'Back', articles_path %>
```
- Let's break this down a bit:
  - First new thing here is that you are now checking if any errors occurred with the code `@article.errors.any?`
    - In case of an error, you show a list of all errors with `@article.errors.full_messages`
  - `pluralize` is a Rails helper that takes a number and a string as its arguments
    - If the number is greater than one, the string is automatically pluralized
  - The reason for adding `@article = Article.new` in the `ArticlesController` is because `@article` would be `nil`
    - Calling `article.errors.any?` would throw an error

NOTE: Rails automatically wraps fields that contain an error with a div with the class `field_with errors`; You can define a CSS rule to make them stand out.

Now, attempt to submit another no-title article on http://localhost:3000/articles/new - it should handle the errors much more cleanly.

#### Updating Articles
The first step is to add an `edit` action to the `ArticlesController`, typically between the `new` and `create` actions:
```ruby
def new
  @article = Article.new
end
 
def edit
  @article = Article.find(params[:id])
end
 
def create
  @article = Article.new(article_params)
 
  if @article.save
    redirect_to @article
  else
    render 'new'
  end
end
```

The view will contain a form similar to the one you used when creating new articles. Create a new file, `app/views/articles/edit.html.erb` and give it the following contents:
```html
<h1>Edit article</h1>
<%= form_with(model: @article, local: true) do |form| %>
 
  <% if @article.errors.any? %>
    <div id="error_explanation">
      <h2>
        <%= pluralize(@article.errors.count, "error") %> prohibited
        this article from being saved:
      </h2>
      <ul>
        <% @article.errors.full_messages.each do |msg| %>
          <li><%= msg %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
 
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>
 
  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>
 
  <p>
    <%= form.submit %>
  </p>
 
<% end %>
 
<%= link_to 'Back', articles_path %>
```
- This form is pointed to the `update` action (which is not yet defined, but will be soon)
  - Passing the article object to this method will _automatically create the URL_ for submitting the edited article form
- The arguments to `form_with` could be model objects
  - For example, `model: @article` which would cause the helper to fill in the form with fields of the object
  - Passing in a symbol scope (`scope: article`) just creates the fields, but without anything filled into them
  - More details can be found in [`form_with` documentation](http://api.rubyonrails.org/v5.2.0/classes/ActionView/Helpers/FormHelper.html#method-i-form_with)

**Create the `update` Action**
Add the `update` action to `articles_controller.rb` between `create` and the `article_params` method, as shown:
```ruby
def create
  @article = Article.new(article_params)
 
  if @article.save
    redirect_to @article
  else
    render 'new'
  end
end
 
def update
  @article = Article.find(params[:id])
 
  if @article.update(article_params)
    redirect_to @article
  else
    render 'edit'
  end
end
 
private
  def article_params
    params.require(:article).permit(:title, :text)
  end
```
- `update` is used when you want to update a record that already exists
  - It accepts a hash containing the attributes that you want to update
  - This too handles errors as the `create` method does (by showing the user the form again, with the errors of the previous submission)

Now, you need to add a link to the `edit` action in the list of articles; do this in `app/views/articles/index.html.erb` (make it appear next to the "Show" link):
```html
<table>
  <tr>
    <th>Title</th>
    <th>Text</th>
    <th colspan="2"></th>
  </tr>
 
  <% @articles.each do |article| %>
    <tr>
      <td><%= article.title %></td>
      <td><%= article.text %></td>
      <td><%= link_to 'Show', article_path(article) %></td>
      <td><%= link_to 'Edit', edit_article_path(article) %></td>
    </tr>
  <% end %>
</table>
```

Additionally, add one to the `app/views/articles/show.html.erb` template:
```html
...
 
<%= link_to 'Edit', edit_article_path(@article) %> |
<%= link_to 'Back', articles_path %>
```

#### Using Partials to Clean Up Duplication in Views
In this demo application, the `edit` page looks very similar to the `new` page - they share the same code. In order to remove duplicate code, _use a **partial**_. By convention, partial files are prefixed with an underscore. (Learn more about partials in the [Layouts and Rendering in Rails](http://guides.rubyonrails.org/layouts_and_rendering.html) guide)

Create a new file at `app/views/articles/` and title it `_form.html.erb`:
```html
<%= form_with model: @article, local: true do |form| %>
 
  <% if @article.errors.any? %>
    <div id="error_explanation">
      <h2>
        <%= pluralize(@article.errors.count, "error") %> prohibited
        this article from being saved:
      </h2>
      <ul>
        <% @article.errors.full_messages.each do |msg| %>
          <li><%= msg %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
 
  <p>
    <%= form.label :title %><br>
    <%= form.text_field :title %>
  </p>
 
  <p>
    <%= form.label :text %><br>
    <%= form.text_area :text %>
  </p>
 
  <p>
    <%= form.submit %>
  </p>
 
<% end %>
```
- Everyting except for the `form_with` declaration remained the same
  - The reason you can use this simpler `form_with` declaration to stand in for either of the other forms is that `@article` is a _resource_ corresponding to a full set of RESTful routes
    - Rails is able to infer which URI and method to use
    - For more information about this use of `form_with`, see [Resource-oriented style](http://api.rubyonrails.org/v5.2.0/classes/ActionView/Helpers/FormHelper.html#method-i-form_with-label-Resource-oriented+style)

**Update `new` to Use the `_form.html.erb` Partial**
```html
<h1>New article</h1>
 
<%= render 'form' %>
 
<%= link_to 'Back', articles_path %>
```

And do the same for `edit.html.erb`:
```html
<h1>Edit article</h1>
 
<%= render 'form' %>
 
<%= link_to 'Back', articles_path %>
```

Much cleaner!

### Deleting Articles
Following the REST convention, the route for deleting articles as per output of `bin/rails routes` is:
```bash
DELETE /articles/:id(.:format)      articles#destroy
```
- The `delete` routing method should be used for routes that destroy resources
  - If this was left as a typical `get` route, it could be possible for people to craft malicious URLs like this:
```bash
<a href='http://example.com/articles/1/destroy'>look at this cat!</a>
```

The `delete` method is used for destroying resources - this route is mapped to the `destroy` action inside `app/controllers/articles_controller.rb` (which doesn't exist yet). The `destroy` method is generally the last CRUD action in the controller. _Like other public CRUD actions, **it must be placed before any private or protected methods**_. Add the following `delete` action to `articles_controller.rb`:
```ruby
def destroy
  @article = Article.find(params[:id])
  @article.destroy
 
  redirect_to articles_path
end
```
-You can call `destroy` on Active Record objects when you want to delete them from the database
  - _You do not need a view for this action - it just redirects to the `index` action_

Finally, add a "Destroy" link to you `index` action template:
```html
<h1>Listing Articles</h1>
<%= link_to 'New article', new_article_path %>
<table>
  <tr>
    <th>Title</th>
    <th>Text</th>
    <th colspan="3"></th>
  </tr>
 
  <% @articles.each do |article| %>
    <tr>
      <td><%= article.title %></td>
      <td><%= article.text %></td>
      <td><%= link_to 'Show', article_path(article) %></td>
      <td><%= link_to 'Edit', edit_article_path(article) %></td>
      <td><%= link_to 'Destroy', article_path(article),
              method: :delete,
              data: { confirm: 'Are you sure?' } %></td>
    </tr>
  <% end %>
</table>
```
- Here you are using `link_to` in a different way
  - You pass the named route as the second argument and the options as another argument
- The method `:delete` and `data: { confirm: 'Are you sure?' }` are used as the HTML5 attributes
  - Rails will first show a confirm dialog to the user
  - It then submits the link with the method `delete` (unless the user declines the confirmation)
    - This is done via the JavaScript file `rails-ujs`, which is automatically included in your application's layout when you generated the application
    - _Without this file, **the confirmation dialog won't appear**_

To learn more about Unobtrusive JavaScript in the [Working with JavaScript in Rails](http://guides.rubyonrails.org/working_with_javascript_in_rails.html) guide

**In general, Rails encourages using resource objects instead of declaring routes manually. For more information, see [Rails Routing from the Outside In](http://guides.rubyonrails.org/routing.html)**

### 6 - Adding a Second Model
The second model in this application will handle comments on articles.

#### Generating the Model
Use the same generator you used when creating the `Article` model. This time, create a Comment model to hold a reference to an article. To do this run the following command in your terminal (from within the `blog` directory):
```bash
bin/rails generate model Comment commenter:string body:text article:references
```
- This command will generate 4 files:
  - `db/migrate/XXXXXXXXXX_create_comments.rb` (Where the Xs are a time stamp)
    - Migration to create the comments table in your database
  - `app/models/comment.rb`
    - The actual Comment model
  - `test/models/comment_test.rb`
    - Testing harness for the comment model
  - `test/fixtures/comments.yml`
    - Sample comments for use in testing

**The `Comment` Model**
Look at the `comment.rb` file:
```ruby
class Comment < ApplicationRecord
  belongs_to :article
end
```
- This model is very similar to the `Article` model
  - The only difference is `belongs_to: article`
    - This sets up an Active Record _association_
    - It appears that this _is not_ added automatically in the `bin/rails generate` command
      - For now, I am adding this manually, but there may be something to look into here

The `(:references)`keyword used in generating this model is a special data type for models. _It creates a new column on your database table with the provided model name appended with an `_id` that can hold integer values_. 

**The Updated the Schema**
To get a better understanding of this process, analyze `db/schema.rb` after running the migration.

In addtion to the model, Rails has also made a migration to create the corresponding database table:
```ruby
class CreateComments < ActiveRecord::Migration[5.0]
  def change
    create_table :comments do |t|
      t.string :commenter
      t.text :body
      t.references :article, foreign_key: true
 
      t.timestamps
    end
  end
end
```
- The `t.references` line creates an integer column called `article_id`
  - Also creates an index for it and a foreign key constraint
    - FK constraint points to the ID column of the `articles` table

NOW, run the migration using the following command:
```bash
bin/rails db:migrate
```

_Rails is smart enough to only execute the migrations that have not already been run against the current database_. You should see something similar to the following:
```bash
==  CreateComments: migrating =================================================
-- create_table(:comments)
   -> 0.0115s
==  CreateComments: migrated (0.0119s) ========================================
```

#### Associating Models
Active Records associations let you easily declare the relationship between two models. This case of comments and articles, their relationships could be written as:
- _Each comment belongs to only one article_
- _One article can have many comments_

You have already seen some syntax similar to this in `comment.rb`, with the `belongs_to` line. Now you need to **edit `article.rb` to add the other side of the association**
```ruby
class Article < ApplicationRecord
  has_many :comments
  validates :title, presence: true,
                    length: { minimum: 5 }
end
```
- These two declarations enable quite a bit of automatic behavior
- For more information on Active Record associations, see the [Active Record Associations](http://guides.rubyonrails.org/association_basics.html) guide

#### Adding a Route for Comments
As with the `welcome` controller, we will need to add a route so that Rails knows where we would like to navigate to see comments. Open up `config/routes.rb` and edit it as follows:
```ruby
resources :articles do
  resources :comments
end
```
- This creates `comments` as a _nested resource_ within `articles`
  - This is another part of capturing the hierarchical relationship that exists between articles and comments

For more information on routing, see the [Rails Routing](http://guides.rubyonrails.org/routing.html) guide

#### Generating a Controller
Now that you have the model, it's time to create a matching controller. Use the same generator you have used before (make sure your syntax is correct - this is always important)
```bash
bin/rails generate controller Comments
```

This creates five files and one empty directory:
| File / Directory | Purpose |
| ---------------- | ------- |
| `app/controllers/comments_controller.rb` | The Comments controller |
| `app/views/comments/` | Views of the controller are stored here |
| `test/controllers/comments_controller_test.rb` | The test for the controller |
| `app/helpers/comments_helper.rb` | A view helper file |
| `app/assets/javascripts/comments.coffee` | CoffeeScript for the controller |
| `app/assets/stylesheets/comments.scss` | Cascading style sheet for the controller |

Here is how this sample application behavior should go down:
1. User creates comment directly after reading an article
2. Once the comment has been added, user is sent back to comment page with their comment now showing

To support this, your `CommentsController` is there to provide a method to create comments and delete spam comments when they arrive.

**Enable Commenting on the Article `show` Template**
Before you can even show the user the UI to add a comment, you need wire up the `show` template (`app/views/articles/show.html.erb`) to have this functionality:
```html
<p>
  <strong>Title:</strong>
  <%= @article.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @article.text %>
</p>
 
<h2>Add a comment:</h2>
<%= form_with(model: [ @article, @article.comments.build ], local: true) do |form| %>
  <p>
    <%= form.label :commenter %><br>
    <%= form.text_field :commenter %>
  </p>
  <p>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </p>
  <p>
    <%= form.submit %>
  </p>
<% end %>
 
<%= link_to 'Edit', edit_article_path(@article) %> |
<%= link_to 'Back', articles_path %>
```
- This adds a form on the Article page that creates a new comment by calling the `CommentsController create` action
- The `form_with` call uses an array 
  - This will build a nested route, such as `/articles/1/comments`

Edit the `comments_controller.rb` file to add the `create` action:
```ruby
class CommentsController < ApplicationController
  def create
    @article = Article.find(params[:article_id])
    @comment = @article.comments.create(comment_params)
    redirect_to article_path(@article)
  end
 
  private
    def comment_params
      params.require(:comment).permit(:commenter, :body)
    end
end
```
- This is a more complex controller than you have seen so far
  - It's a side effect of the nesting in place here
  - Each request for a comment _has to keep track of the article to which the comment is attached_
    - This is the reason for the initial call to find the Article in question
- In addition, the code takes advantage of some of the methods available for an association
  - Use the `create` method on `@article.comments` to create and save the comment
    - This will automatically link the comment so that it belongs to that particular article

Now, we need to make the comment show on the Article's show view; add the following code to `show.html.erb` like so:
```html
<p>
  <strong>Title:</strong>
  <%= @article.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @article.text %>
</p>
 
<h2>Comments</h2>
<% @article.comments.each do |comment| %>
  <p>
    <strong>Commenter:</strong>
    <%= comment.commenter %>
  </p>
 
  <p>
    <strong>Comment:</strong>
    <%= comment.body %>
  </p>
<% end %>
 
<h2>Add a comment:</h2>
<%= form_with(model: [ @article, @article.comments.build ], local: true) do |form| %>
  <p>
    <%= form.label :commenter %><br>
    <%= form.text_field :commenter %>
  </p>
  <p>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </p>
  <p>
    <%= form.submit %>
  </p>
<% end %>
 
<%= link_to 'Edit', edit_article_path(@article) %> |
<%= link_to 'Back', articles_path %>
```
- _If you didn't miss any steps_ this should all be working!

### 7 - Refactoring
Now, let's look back at `show.html.erb` - it's getting pretty long and awkward, so let's use partials to clean it up some.

#### Rendering Partial Collections
First, make a comment partial to extract showing all the comments for the article. _Create the first file, `app/views/comments/_comment.html.erb`, and put the following in it_:
```html
<p>
  <strong>Commenter:</strong>
  <%= comment.commenter %>
</p>
 
<p>
  <strong>Comment:</strong>
  <%= comment.body %>
</p>
```

After creating the above file, you can change `show.html.erb` to look like the following:
```html
<p>
  <strong>Title:</strong>
  <%= @article.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @article.text %>
</p>
 
<h2>Comments</h2>
<%= render @article.comments %>
 
<h2>Add a comment:</h2>
<%= form_with(model: [ @article, @article.comments.build ], local: true) do |form| %>
  <p>
    <%= form.label :commenter %><br>
    <%= form.text_field :commenter %>
  </p>
  <p>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </p>
  <p>
    <%= form.submit %>
  </p>
<% end %>
 
<%= link_to 'Edit', edit_article_path(@article) %> |
<%= link_to 'Back', articles_path %>
```
- This will _render the partial once for each comment that is in the `@article.comments` collection_
  - Basically, a db-linked for-loop that generates a partial for each detected item (and populates them accordingly)

#### Rendering a Partial Form
You can also render forms as partials - do that now for the comment form. Create a new file, `app/views/comments/_form.html` with the following contents:
```html
<%= form_with(model: [ @article, @article.comments.build ], local: true) do |form| %>
  <p>
    <%= form.label :commenter %><br>
    <%= form.text_field :commenter %>
  </p>
  <p>
    <%= form.label :body %><br>
    <%= form.text_area :body %>
  </p>
  <p>
    <%= form.submit %>
  </p>
<% end %>
```

And then edit the `show.html.erb` file to look like the following:
```html
<p>
  <strong>Title:</strong>
  <%= @article.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @article.text %>
</p>
 
<h2>Comments</h2>
<%= render @article.comments %>
 
<h2>Add a comment:</h2>
<%= render 'comments/form' %>
 
<%= link_to 'Edit', edit_article_path(@article) %> |
<%= link_to 'Back', articles_path %>
```
- The second render just defines the partial template we want to render (`comments/form`)
- Rails is smart enough to spot the forward slash in that string and know you want to render `_form.html.erb` (in the `app/views/comments` directory)
- The `@article` object is available to any partials rendered in the view
  - This is because we defined it as an _instance variable_

### 8 - Deleting Comments
People can be idiots. Or mean. Or mean idiots. To combat this, you need a way to delete "less than admirable" comments. To do this, you need to implement a link of some sort in the view and a `destroy` action in the `CommentsController`

Add the delete link to the `_comment.html` partial:
```html
<p>
  <strong>Commenter:</strong>
  <%= comment.commenter %>
</p>
 
<p>
  <strong>Comment:</strong>
  <%= comment.body %>
</p>
 
<p>
  <%= link_to 'Destroy Comment', [comment.article, comment],
               method: :delete,
               data: { confirm: 'Are you sure?' } %>
</p>
```
- Clicking this link fires off a `DELETE` for `/articles/:article_id/comments/:id` to your `CommentController`
  - This is used to find the comment you want to delete

Add a `destroy` action to `comments_controller.rb`:
```ruby
class CommentsController < ApplicationController
  def create
    @article = Article.find(params[:article_id])
    @comment = @article.comments.create(comment_params)
    redirect_to article_path(@article)
  end
 
  def destroy
    @article = Article.find(params[:article_id])
    @comment = @article.comments.find(params[:id])
    @comment.destroy
    redirect_to article_path(@article)
  end
 
  private
    def comment_params
      params.require(:comment).permit(:commenter, :body)
    end
end
```
- The `destroy` action will find the article we are looking at, locate the target comment within the `@article.comments` collection and remove it from the database
  - The user should be redirected to the show action for the article after completing the article deletion

#### Deleting Associated Objects
If you can delete an article, its associated comments will also need to be deleted, **otherwise they would simply occupy space in the database** (oops, I accidentally marooned some data when I deleted an article earlier - oh well!)
- Rails allows you to use the `dependent` option of an association to achieve this

Modify the Article model, `app/models/article.rb` as follows:
```ruby
class Article < ApplicationRecord
  has_many :comments, dependent: :destroy
  validates :title, presence: true,
                    length: { minimum: 5 }
end
```

### 9 - Security
At this point, anyone could publish to your blog if you were to post it online. This includes editing and deleting articles or deleting comments. Rails provides a very simple HTTP authentication system that will work nicely in this situation.

In the `ArticlesController` we need to have a way to block access to the various actions if the person is not authenticated. The `http_basic_authenticate_with` method allows access to the requested action if that method allows it.

**Using the Authentication System**
To use the authentication system, you need to specify it at the top of `ArticlesController` in `app/controllers/articles_controller.rb`. In the case of this tutorial, you want the user to be authenticated on every action except `index` and `show`. 

To do this, write:
```ruby

class ArticlesController < ApplicationController
 
  http_basic_authenticate_with name: "dhh", password: "secret", except: [:index, :show]
 
  def index
    @articles = Article.all
  end
 
  # snippet for brevity
```

You also want to make sure that only authenticated users can delete their own comments. In `comments_controller.rb`, write:
```ruby
class CommentsController < ApplicationController
 
  http_basic_authenticate_with name: "dhh", password: "secret", only: :destroy
 
  def create
    @article = Article.find(params[:article_id])
    # ...
  end
 
  # snippet for brevity
```
Other Authentication options are available for Rails applications. Two popular authentication add-ons for Rails are the [Devise](https://github.com/plataformatec/devise) rails engine and the Authlogic

#### Other Security Considerations
Security, _especially in web applications_, is a broad and detailed area. Security in your rails application is covered in more depth in the [Ruby on Rails Security Guide](http://guides.rubyonrails.org/security.html)

