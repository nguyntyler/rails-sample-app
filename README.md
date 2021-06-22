# Sample App
[Heroku](https://powerful-waters-62992.herokuapp.com/)
___
## Getting started
To get started with the app, clone the repo and then install
the needed gems:
```
$ bundle install --without production
```
Next, migrate the database:
```
$ rails db:migrate
```
Finally, run the test suite to verify that everything is
working correctly:
```
$ rails test
```
If the test suite passes, you'll be ready to run the app in a
local server:
```
$ rails server
```
# INSERT RAILS SHORTCUT IMAGE
### Undo a mistake in generation
```
$ rails generate controller StaticPages home help
```
The code below undos the code above.
```
$ rails d controller StaticPages home help
```
Similarily...
```
$ rails generate model User name:string email:string
```
Code below undos above.
```
$ rails d model User
```
___
When generating controllers, the routes.rb file will automatically populate with the corresponding routes. In this case:
```ruby
Rails.application.routes.draw do
  get 'static_pages/home'
  get 'static_pages/help'
end
```
___
## Testing
Rails has built in testing. By default, when creating controllers, rails provides a file for testing under 
> test > controllers > static_pages_controller_test.rb
```ruby
test "should get home" do
  get static_pages_home_url
  assert_response :success
  assert_select 'title', 'Home | Ruby on Rails Tutorial Sample App'
end
```
When destructuring this test:
- `get static_pages_home_url` Tries to go to the url site /static_pages/home.
- `assert_response :success` is the first test that makes sure the route can be reached.
- `assert_select 'title', 'Home | Ruby on Rails Tutorial Sample App'` tests if the title in the reached url has a title of 'Home | Ruby on Rails Tutorial Sample App'.

If this is done multiple times, the portion `Ruby on Rails Tutorial Sample App` is duplicated. We can set that to a variable and interpolate it to keep it dry.
```ruby
def setup
  @base_title = 'Ruby on Rails Tutorial Sample App'
end
test "should get home" do
  get static_pages_home_url
  assert_response :success
  assert_select 'title', "Home | #{@base_title}"
end
```
The function `setup` is ran before every test.
___
### ERB (Embedded Ruby)
```
<% provide(:title, "Home") %>
<!DOCTYPE html> <html>
<head>
<title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
</head> 
    <body>
        <h1>Sample App</h1> 
        <p>
        This is the home page for the
        <a href="https://www.railstutorial.org/">Ruby on Rails Tutorial</a>
        sample application.
        </p> 
    </body>
</html>
```
Here, the code `<% provide(:title, "Home") %>` indicates using **<% ... %>** that Rails should call the provide function and associate the string **"Home"** with the label **:title**. Then, in the title, we use the closely related notation **<%= ... %>** to insert the title into the template using Ruby’s **yield** function
In this code `<title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>`, the use of **<%= ... %>** differs from the one before in that,
> `<% ... %>` *executes* the code inside.

> `<%= ... %>` executes and *inserts* the result into the template.
___
### Helpers
Helpers allow us to have conditional renders. Inside `app > helpers > application_helpers.rb`, we can write a function that will return something.
```ruby
def full_title(page_title = '')
  base_title = "Ruby on Rails Tutorial Sample App"
  if page_title.empty?
    base_title
  else
    page_title + " | " + base_title
  end
end
```
In this case, the function `full_title`, when given a page_title as a parameter, will return one thing or another. 

Using `yield` from before, we can use `:title` as the parameter.
Instead of...
```
<title><%= yield(:title) | Ruby on Rails Tutorial Sample App</title> 
```
We can have...
```
<title><%= full_title(yield(:title)) %></title>
```
Now, in the case that a `:title` is not given, it will default to the base title. 
