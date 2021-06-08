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