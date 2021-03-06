Being a pretty standard Rails application, GitLab is built using the MVC design pattern. Please describe in as much detail as you think is appropriate what the responsibilities of the Model, View and Controller are, both in general and in Rails specifically, and what the benefits of this separation are. Also touch on how the Concern and Service patterns fit into this.

<b>Model–view–controller (MVC)</b> is a software architectural pattern for implementing user interfaces on computers. It divides a given software application into three interconnected parts, so as to seperate internal representations of information from the ways that information is presented to or accepted from the user.

In object-oriented programming development, model-view-controller (MVC) is the name of a methodology or design pattern for successfully and efficiently relating the user interface to underlying data models.

<b>Model</b> - Model represents an object carrying data. It represents the underlying, logical structure of data in a software application and the high-level class associated with it. Model objects encapsulate the data specific to an application and define the logic and computation that manipulate and process that data. For example, a model object might represent a level in a game or a contact in an address book. A model object can have to-one and to-many relationships with other model objects. Because model objects represent knowledge and expertise related to a specific problem domain, they can be reused in similar problem domains. Ideally, a model object should have no explicit connection to the view objects that present its data and allow users to edit that data—it should not be concerned with user-interface and presentation issues.

<b>View</b> - View represents the visualization of the data that model contains. It consists of all the things a user can see and respond to on the screen, such as buttons, display boxes, and so forth. A view object knows how to draw itself and can respond to user actions. A major purpose of view objects is to display data from the application’s model objects and to enable the editing of that data.

<b>Controller</b> - Controller acts on both model and view. It controls the data flow into model object and updates the view whenever data changes. It keeps view and model seperate. A controller object acts as an intermediary between one or more of an application’s view objects and one or more of its model objects.

<b>What the benefits of this separation are?</b>

The MVC pattern is a useful pattern for the reuse of object code and a pattern that allows significant reduce to the time it takes to develop applications with user interfaces. The benefits of adopting this pattern are numerous. Many objects in these applications tend to be more reusable, and their interfaces tend to be better defined. Applications having an MVC design are also more easily extensible than other applications.

### The MVC Architecture in Rails

![MVC Architecture in Rails](https://upload.wikimedia.org/wikibooks/en/0/0a/Mvc_rails.jpg)

Active Record is the M in MVC - the model - which is the layer of the system responsible for representing business data and logic. Active Record facilitates the creation and use of business objects whose data requires persistent storage to a database. It is an implementation of the Active Record pattern which itself is a description of an Object Relational Mapping system.

Active Record gives us several mechanisms, the most important being the ability to:

* Represent models and their data.
* Represent associations between these models.
* Represent inheritance hierarchies through related models.
* Validate models before they get persisted to the database.
* Perform database operations in an object-oriented fashion.

##### Active Record Migrations

Migrations are a convenient way to alter our database schema over time in a consistent and easy way. Each migration is a new 'version' of the database.
Here's an example of a migration:

    class CreateProducts < ActiveRecord::Migration
      def change
        create_table :products do |t|
          t.string :name
          t.text :description
      
          t.timestamps
        end
      end
    end

We can change a column, rename a column, remove a column, and there are many more migration definitions.
Example:

    class AddPartNumberToProducts < ActiveRecord::Migration[5.0]
      def change
        add_column :products, :part_number, :string
      end
    end
<hr>
    class RemovePartNumberFromProducts < ActiveRecord::Migration[5.0]
      def change
        remove_column :products, :part_number, :string
      end
    end

##### Active Record Validations

Validations are used to ensure that only valid data is saved into your database. For example, it may be important to your application to ensure that every user provides a valid email address and mailing address. Model-level validations are the best way to ensure that only valid data is saved into your database.

A validation in Rails Active Record looks like:

    class Person < ApplicationRecord
      validates :name, presence: true
    end

The following methods trigger validations, and will save the object to the database only if the object is valid:

* create
* create!
* save
* save!
* update
* update!

##### Active Record CallBacks

Callbacks are methods that get called at certain moments of an object's life cycle. With callbacks it is possible to write code that will run whenever an Active Record object is created, saved, updated, deleted, validated, or loaded from the database.

Available callbacks

Creating an Object:

* before_validation
* after_validation
* before_save
* around_save
* before_create
* around_create
* after_create
* after_save
* after_commit/after_rollback

Updating an Object:

* before_validation
* after_validation
* before_save
* around_save
* before_update
* around_update
* after_update
* after_save
* after_commit/after_rollback

Destroying an Object:

* before_destroy
* around_destroy
* after_destroy
* after_commit/after_rollback

Example:

    class User < ApplicationRecord
      validates :login, :email, presence: true

      before_validation :ensure_login_has_a_value

      protected
      def ensure_login_has_a_value
        if login.nil?
          self.login = email unless email.blank?
        end
      end
    end

##### Active Record Associations

In Rails, an association is a connection between two Active Record models. Active Record associations can be used to describe one-to-one, one-to-many and many-to-many relationships between models. Each model uses an association to describe its role in the relation.

 The <b>belongs_to</b> Association

 Example:
 
    class Book < ApplicationRecord
      belongs_to :author
    end

 which means each book belongs to exactly one author.
 
 ![belongs_to](http://guides.rubyonrails.org/images/belongs_to.png)

 The <b>has_one</b> Association

     class Supplier < ApplicationRecord
       has_one :account
     end

 which means each supplier in the application has only one account.
 
 ![has_one](http://guides.rubyonrails.org/images/has_one.png)

 The <b>has_many</b> Association

     class Author < ApplicationRecord
       has_many :books
     end

 An author can have many books.
 
 ![has_many](http://guides.rubyonrails.org/images/has_many.png)

 The <b>has_many :through</b> Association

 A has_many :through association is often used to set up a many-to-many connection with another model.

![has_many_through](http://guides.rubyonrails.org/images/has_many_through.png)

The <b>has_one :through</b> Association
A has_one :through association sets up a one-to-one connection with another model.

![has_one_through](http://guides.rubyonrails.org/images/has_one_through.png)

The <b>has_and_belongs_to_many</b> Association

A has_and_belongs_to_many association creates a direct many-to-many connection with another model, with no intervening model.

![habtm](http://guides.rubyonrails.org/images/habtm.png)

##### Active Record Query Interface

Active Record will perform queries on the database and is compatible with most database systems, including MySQL, MariaDB, PostgreSQL and SQLite. Regardless of which database system we're using, the Active Record method format will always be the same.

#### ActionController

Action Controller is the C in MVC. The controller is responsible for making sense of the request and producing the appropriate output. For most conventional RESTful applications, the controller will receive the request, fetch or save data from a model and use a view to create HTML output. A controller can thus be thought of as a middleman between models and views. It makes the model data available to the view so it can display that data to the user, and it saves or updates user data to the model.

A controller is a Ruby class which inherits from ApplicationController and has methods just like any other class. When our application receives a request, the routing will determine which controller and action to run, then Rails creates an instance of that controller and runs the method with the same name as the action.

    class ClientsController < ApplicationController
      def new
      end
    end

As an example, if a user goes to /clients/new in your application to add a new client, Rails will create an instance of ClientsController and call its new method.

##### Parameters:

There are two kinds of parameters possible in a web application. The first are parameters that are sent as part of the URL, called query string parameters. The query string is everything after "?" in the URL. The second type of parameter is usually referred to as POST data. This information usually comes from an HTML form which has been filled in by the user. It's called POST data because it can only be sent as part of an HTTP POST request. Rails does not make any distinction between query string parameters and POST parameters, and both are available in the params hash in your controller:

    class ClientsController < ApplicationController
      # This action uses query string parameters because it gets run
      # by an HTTP GET request, but this does not make any difference
      # to the way in which the parameters are accessed. The URL for
      # this action would look like this in order to list activated
      # clients: /clients?status=activated
      def index
        if params[:status] == "activated"
          @clients = Client.activated
        else
          @clients = Client.inactivated
        end
      end

      # This action uses POST parameters. They are most likely coming
      # from an HTML form which the user has submitted. The URL for
      # this RESTful request will be "/clients", and the data will be
      # sent as part of the request body.
      def create
        @client = Client.new(params[:client])
        if @client.save
          redirect_to @client
        else
          # This line overrides the default rendering behavior, which
          # would have been to render the "create" view.
          render "new"
        end
      end
    end


##### Strong Parameters
With strong parameters, Action Controller parameters are forbidden to be used in Active Model mass assignments until they have been whitelisted. This means that we have to make a conscious decision about which attributes to allow for mass update. This is a better security practice to help prevent accidentally allowing users to update sensitive model attributes.

In addition, parameters can be marked as required and will flow through a predefined raise/rescue flow to end up as a 400 Bad Request.

    class PeopleController < ActionController::Base
      # This will raise an ActiveModel::ForbiddenAttributes exception
      # because it's using mass assignment without an explicit permit
      # step.
      def create
        Person.create(params[:person])
      end

      # This will pass with flying colors as long as there's a person key
      # in the parameters, otherwise it'll raise a
      # ActionController::ParameterMissing exception, which will get
      # caught by ActionController::Base and turned into that 400 Bad
      # Request reply.
      def update
        person = current_account.people.find(params[:id])
        person.update!(person_params)
        redirect_to person
      end

     private
     # Using a private method to encapsulate the permissible parameters
     # is just a good pattern since you'll be able to reuse the same
     # permit list between create and update. Also, you can specialize
     # this method with per-user checking of permissible attributes.
      def person_params
        params.require(:person).permit(:name, :age)
      end
    end

#### Filters

Filters are methods that are run "before", "after" or "around" a controller action.

Filters are inherited, so if you set a filter on ApplicationController, it will be run on every controller in your application.

"before" filters may halt the request cycle. A common "before" filter is one which requires that a user is logged in for an action to be run. You can define the filter method this way:

Example:

    class ApplicationController < ActionController::Base
      before_action :require_login

      private

      def require_login
        unless logged_in?
          flash[:error] = "You must be logged in to access this section"
          redirect_to new_login_url # halts request cycle
        end
      end
    end

#### Action View

In Rails, web requests are handled by Action Controller and Action View. Typically, Action Controller will be concerned with communicating with the database and performing CRUD actions where necessary. Action View is then responsible for compiling the response.

Action View templates are written using embedded Ruby in tags mingled with HTML. To avoid cluttering the templates with boilerplate code, a number of helper classes provide common behavior for forms, dates, and strings. It's also easy to add new helpers to our application as it evolves.

2 Using Action View with Rails
For each controller there is an associated directory in the app/views directory which holds the template files that make up the views associated with that controller. These files are used to display the view that results from each controller action.

Let's take a look at what Rails does by default when creating a new resource using the scaffold generator:

    $ bin/rails generate scaffold article
      [...]
      invoke  scaffold_controller
      create    app/controllers/articles_controller.rb
      invoke    erb
      create      app/views/articles
      create      app/views/articles/index.html.erb
      create      app/views/articles/edit.html.erb
      create      app/views/articles/show.html.erb
      create      app/views/articles/new.html.erb
      create      app/views/articles/_form.html.erb
      [...]

There is a naming convention for views in Rails. Typically, the views share their name with the associated controller action, as you can see above. For example, the index controller action of the articles_controller.rb will use the index.html.erb view file in the app/views/articles directory. The complete HTML returned to the client is composed of a combination of this ERB file, a layout template that wraps it, and all the partials that the view may reference.

#### Templates and Partials

Action View templates can be written in several ways. If the template file has a .erb extension then it uses a mixture of ERB (Embedded Ruby) and HTML.
Rails supports multiple template systems and uses a file extension to distinguish amongst them. For example, an HTML file using the ERB template system will have .html.erb as a file extension.

##### ERB

Within an ERB template, Ruby code can be included using both <% %> and <%= %> tags. The <% %> tags are used to execute Ruby code that does not return anything, such as conditions, loops or blocks, and the <%= %> tags are used when you want output.

##### Partials

Partial templates - usually just called "partials" - are another device for breaking the rendering process into more manageable chunks. With partials, you can extract pieces of code from your templates to separate files and also reuse them throughout your templates.

    <%= render "menu" %>

This will render a file named _menu.html.erb at that point within the view that is being rendered. 

#### Concerns:

Concerns are essentially modules that allow us to encapsulate model roles into separate files to DRY up our code. Concerns can be included in different models or controllers and can be reused.

A typical Model concern looks like this:

    module Votable
      extend ActiveModel::Concern

      included do
        has_many :votes, as: :votable
      end

      def upvote!
        votes.create(type: :upvote)
      end

      def downvote!
        votes.create(type: :downvote)
      end
    end

A controller concern might look like this:

    module FeedbackPerformer
      extend ActiveSupport::Concern

      included do
        before_action :restrict_with_user_token
      end

      def create
        # some code
      end

      private
      def feedback_params
        params.require(:feedback).permit(:someparams)
      end
    end


Traditionally, the models may look like this:

Comment Model:

    class Comment < ActiveRecord::Base
      belongs_to :commentable, polymorphic: true
    end

Article Model:

    class Article < ActiveRecord::Base
      has_many :comments, as: :commentable

      def find_first_comment
        comments.first(created_at DESC)
      end

      def self.least_commented
       #return the article with least number of comments
      end
    end

Event Model

    class Event < ActiveRecord::Base
      has_many :comments, as: :commentable

      def find_first_comment
        comments.first(created_at DESC)
      end

      def self.least_commented
        #returns the event with least number of comments
      end
    end

As we can notice, there is a significant piece of code common to both Event and Article. Using concerns we can extract this common code in a separate module Commentable.

For this create a commentable.rb file in app/models/concerns.

    module Commentable
      extend ActiveSupport::Concern

      included do
        has_many :comments, as: :commentable
      end

      # for the given article/event returns the first comment
      def find_first_comment
        comments.first(created_at DESC)
      end

      module ClassMethods
        def least_commented
          #returns the article/event which has the least number of comments
        end
      end
    end

And now your models look like this :

Comment Model:

    class Comment < ActiveRecord::Base
      belongs_to :commentable, polymorphic: true
    end
 
Article Model:

    class Article < ActiveRecord::Base
      include Commentable
    end

Event Model:

    class Event < ActiveRecord::Base
      include Commentable
    end

#### Services:

Services are used to keep our Rails controllers clean and DRY. Service has the benefit of concentrating the core logic of the application in a separate object, instead of scattering it around controllers and models. When we apply some basic principles (like ‘fat models, slim controllers’) to our application, we can live happily very long with this basic fragmentation. However, when our application grows, our skinny controllers become not so skinny over time. We can’t test in isolation, because we’re highly coupled with the framework. To fix this problem, we can use service objects as a new layer in our design.

We get a lot of benefits when we introduce services, including:

* Ability to test controllers - controller becomes a really thin wrapper which provides collaborators to services - thus we can only check if certain methods within controller are called when certain action occurs,

* Ability to test business process in isolation - when we separate process from it’s environment, we can easily stub all collaborators and only check if certain steps are performed within our service.

* They make controllers slim - even in bigger applications actions using service objects usually don’t take more than 10 LoC.

Example:
A common design pattern for performing tasks after an object is created is to use an ActiveModel Callback. For example:

    class User < ActiveRecord::Base
      after_create :send_welcome_email

      def send_welcome_email
        # Send an email
      end
    end

Yes, this is simplistic, but there are a few problems with this.

* It's not the User models responsibility to send an email.
* Unless it modifies internal state, callbacks should be avoided.
* Testing becomes painful and often times requires stubbing.

Lets talk about responsibility for a moment. In my opinion, if it's an interaction, it shouldn't belong to one specific model. What if you need a send_invoice_email to go with send_welcome_email? This can quickly get out of hand. This is why I use service objects.

So what exactly is a service object? It's really just an object that encapsulates operations. Using our initial callback example, lets refactor it to use a service object by adding the following to app/services/send_welcome_email.rb

    class SendWelcomeEmail
      def self.call(user)
        UserMailer.welcome_email(user).deliver
      end
    end

Now to send a welcome email, you would do:

    SendWelcomeEmail.call(user)

This makes it far easier to test and decouples the responsibility.

