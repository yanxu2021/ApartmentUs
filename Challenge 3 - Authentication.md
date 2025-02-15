# Challenge 3 - Authentication
A key component of web applications is the ability for a user to log in. This requires using the Devise gem to create authentication and authorization for a Rails application.
## 1. Add RSpec
```
$ bundle add rspec-rails
$ rails generate rspec:install
```
## 2. Add Devise
```
$ bundle add devise
$ rails generate devise:install
$ rails generate devise User
$ rails db:migrate
```
## 3. Add mailer settings

Here need to set up the default URL options for the Devise mailer in each environment. 

**config/environments/development.rb** , add the following code at the end of the previous code inside the file:
```
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```

Devise is a Rails gem that gives developers a collection of methods that create authorization and authentication. Using Devise to create a special model called User that gets Devise code injected into each new model instance. Just by running the setup commands to get Devise *sign in* and *sign up* forms as well as a lot of additional functionality.

Navigate to `http://localhost:3000/users/sign_up` and see a sign up page.

![user sign up](https://github.com/yanxu2021/ApartmentUs/blob/main/img/2.png)

Navigate to `http://localhost:3000/users/sign_in` and see a log in page.

![user log in](https://github.com/yanxu2021/ApartmentUs/blob/main/img/3.png)

## 4. Apartment Resource

The Devise User model is going to have an association with the Apartment model. In this situation, the User will have many apartments and the Apartments will belong to a User.
```
$ rails g resource Apartment street:string city:string state:string manager:string email:string 
price:string bedrooms:integer bathrooms:integer pets:string user_id:integer
$ rails db:migrate
```

## 5. User and Apartment Associations

The Apartments will belong to a User and a User will have many apartments.

**app/models/apartment.rb**
```ruby
class Apartment < ApplicationRecord
  belongs_to :user
end
```

**app/models/user.rb**
```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable
  has_many :apartments
end
```

## 6. Devise Code

Devise gets added to a few different spots. 

1. The User model, which we already looked at. 
2. The controller. This code is predominantly /prɪˈdɑːmɪnəntli/adv. 主要地；显著地  behind the scenes. 
3. The routes. We can see that we have a resource for apartments and Devise routes for users.

**config/routes.rb**
```ruby
Rails.application.routes.draw do
  resources :apartments
  devise_for :users
end
```

## 7. Created Seeds

**Seeds** are mock data that developers can load into the backend database and used during for scaffolding an application. Seeds will live with the application file structure rather than on each developer's computer.

**Managing Seeds**

To add seeds to the database, first we must create a database, create a model or resource, and run a migration.

To add seeds run the command `$ rails db:seed` in the terminal. *One thing to keep in mind is that every time your run `$ rails db:seed` the code will execute and add data to your database.*

From there you can drop into the Rails console `$ rails c` and look for the user and apartment with `User.all` and `Apartment.all`. There will be a collection of cat hashes with unique ids, created_at timestamps, and updated_at timestamps.

![seeds](https://github.com/yanxu2021/ApartmentUs/blob/main/img/seeds.png)

**Troubleshooting**

If something goes awry with the seeds file, we can use a couple commands to simply reset the database.

```
$ rails db:drop
$ rails db:create
$ rails db:migrate
$ rails db:seed
```
## 8. API CORS and API Endpoints
CORS stands for Cross-Origin Resource Sharing. CORS manages requests that occur between decoupled applications, or from another "origin". Browsers have security built in to protect users from submitting their data to servers that they are not intending to, so we have to tell the frontend that the backend is indeed the correct place for it to be communicating with.

**Allowing External Requests and skip Authenticity Token**

Update the application controller to allow requests from applications outside the Rails application.

To do this, add a line to the ApplicationController:

**app/controllers/application_controller.rb**
```ruby
class ApplicationController < ActionController::Base
  skip_before_action :verify_authenticity_token
end
```

**API Endpoints**
Endpoints are the location from which APIs can access the resources they need to perform CRUD actions. Endpoints can be tested through request specs and model specs. Implementing request specs in a Rails application. Implementing appropriate endpoints in the apartments controller

Create endpoints for the actions in the React application. 

**app/controllers/apartments_controller.rb**
```ruby
class ApartmentsController < ApplicationController

  def index
  end

  def create
  end

  def update
  end

  def destroy
  end

end
```
Start with the 'index' route, which used to return all of the cats that the application knows about.

Then finish create, update, destroy route.

## 9. Git add/commit/push to new branch-authentication
```
$ git status
$ git checkout -b authentication
$ git add .
$ git commit -m "complete authentication"
$ git push origin authentication
```
Pull request and review or waiting for review, then merge and delete the branch

[ Go to Next Step ](https://github.com/yanxu2021/ApartmentUs/blob/main/Challenge%204%20-%20Main%20UI.md)

[ Go Back ](https://github.com/yanxu2021/ApartmentUs/blob/main/README.md)
