###Cookies in Rails

1 Set a cookie with a hash which has two members 
```
cookies[:remember_token] = { value: remember_token, 
expires: 20.years.from_now.utc }
```
2 Set a cookie by permanent method 
```
cookies.permanent[:remember_token] = remember_token
```
3 Delete cookie 
```
cookies.delete(:remember_token)
```
==================================================================
How to process the "Sign in" in Rails

1 Create a Session 
. generate a session controller 
. have three method in the session controller: create, new and destroy 
. under the new method, we create a page as log in page 
. verify the email and password in the create method, two steps: get user by email and check password by user.authenticate

2 Add a Cookie Store on Both Server Side and Browser Side 
. generate a cookie 
. save that cookie to the client's browser 
. save the cookie to the database which on the user's table, recommend to encrypt

3 Create an instant variable @current_user 
. each time open a new page, all the variable will be cleared. So get the cookies from the client's browser when each time users open a new page

4 Using @current_user to identify the user

========================================================================

Render and Redirect_to

http://guides.rubyonrails.org/layouts_and_rendering.html

======================================================================

Question!
How to call a class in the module?

In the module, it can require a file then the class can be used.

But in the redis, the module is background running, it can't call any classes in other files.

=================================================================================

debugger can't be installed in 2.1.2

use byebug replace it temparary

=======================================================================

Run production env

rake db:migrate RAILS_ENV='production'


==========================================================================

Git ignore

if you want to remove an exist file in git repo

1  must rm this file in the local

2  then type git rm file

3  edit .gitignore add this file into it

4  commit it

=========================================================================

unicorn to background run rails 

Gem

unicorn_rails -E production -D -p 3000

=======================================================================

Run Resque background

nuprake resque:work QUEUE=* BACKGROUND=yes RAILS_ENV=production

nohup rake resque:work QUEUE=*  BACKGROUND=true

=======================================================================

foreman

Gem

start all the rails related services in single line

Start resque with rails
http://blog.daviddollar.org/2011/05/06/introducing-foreman.html 


Start all the rails service and related jobs when start up
sudo foreman export --app sms --user amen upstart /etc/init

this command will export several files that can start all the services by service start command

reference
http://railscasts.com/episodes/281-foreman

========================================================================

Disable auto run service

sudo sh -c "echo 'manual' > /etc/init/SERVICE.override"

========================================================================

acts_as_taggable_on

Gem

ref:http://railscasts.com/episodes/382-tagging

Question: can read, but can't edit.

======================================================

delegate

ref: http://apidock.com/rails/Module/delegate

================================================

Use default ruby version in project

You can tell RVM to always use a specific ruby version and gemset when you are in a project, by creating a simple .rvmrc file with a one line instruction like:

rvm ruby-1.9.3-p374@mygemset

===========================================================

kaminari

A Gem for page split and jumping

=============================================================

Devise

redirect to another page when login failed

http://stackoverflow.com/questions/5832631/devise-redirect-after-login-fail

============================================================

Render 

render a partial and pass a variable, must add 'partial' before partial file name

render partial: 'partial_file', locals {var: @var} 

=============================================================

has_many

has_many :mapping_table, dependent: :destroy
has_many :a_table, ->{uniq}, through: :mapgging_table

===============================================================

Add index

add_column :cog_issues_supports, :id, :primary_key

==============================================================

Git

change branch before commit code, merge modifying code to new branch
git checkout -m branch

=======================================================================

Form Helper

select_tag "people", options_from_collection_for_select(@people, "id", "name")

==================================================================

Skinny Controller and Skinny Model

http://robots.thoughtbot.com/skinny-controllers-skinny-models

============================================================================

Simple Form

https://github.com/plataformatec/simple_form

===========================================================================

persisted?()

Returns true if the record is persisted, i.e. it's not a new record and it was not destroyed, otherwise returns false.

=============================================================================

.rvmrc

rvm use ruby-2.1.2@rvmname --create

=========================================================================

Sql

update_all

http://apidock.com/rails/ActiveRecord/Base/update_all/class

=======================================================================

Google Chart

https://google-developers.appspot.com/chart/interactive/docs/gallery/piechart

======================================================================

Splat Agument
```
  def self.by_user_ids(*ids)
    where(:user_id => ids.flatten.compact.uniq).order('created_at DESC')
  end
  
  Twet.by_user_ids(id, *follows.map(&:following_id))
```
ref: http://endofline.wordpress.com/2011/01/21/the-strange-ruby-splat/

=========================================================================
Image Magic

sudo apt-get install libmagickwand-dev imagemagick

====================================================================

Devise 

add attribute need permit

validate :registration_code_valid?, on: :create, if: Proc.new { |u| u.front_flag.present? }

=========================================================================================
form helper

https://github.com/superbay/knowledge/blob/master/rails_basic/form/select_tag.markdown

=======================================================================================

In model, if do this, it will be a dead loop

before_save :do_some_thing

def do_some_thing
  update_attributes(attr_a: a; attr_b: b)
end

To solve that

def do_some_thing
  self.attr_a = a
  self.attr_b = b
end

=======================================================================

###ruby send method

```ruby
 %w(mon tue wed thu fri sat sun).each do |week_day|                                                                                             │·······
        eval("hash[:#{week_day}] = row.#{week_day}")                                                                                                 │·······
        #eval("hash[:#{week_day}] = row.#{week_day}")                                                                                                │·······
        hash.send(:[]=, week_day.to_sym, row.send(week_day))                                                                                         │·······
 end 
 
```
======================================================================

###Best in place

[https://github.com/bernat/best_in_place](https://github.com/bernat/best_in_place)


### Markdown Basic
https://help.github.com/articles/markdown-basics

### Turbolinks Turn Off

###### application.html.erb
```ruby
<%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
<%= javascript_include_tag 'application' %>
```

http://railscasts.com/episodes/390-turbolinks

### gemset
###### use default project ruby version

```
vim .rvmrc
rvm use 2.1.2@tweter


rvm use 2.1.2

rvm gemset create tweter
```

### belongs_to class name
```ruby
#retwet model 
#attr: origin_id => user_id

belongs_to :origin, class_name: 'User'

```

### ruby running path
```
ruby -I . test/test_calculator.rb
```
### move rake tast to db migration
```ruby
Rake::Task['task name'].invoke
```
### delete table quick way
```ruby
namespace :db do
  desc "Truncate all existing data"
  task :truncate => "db:load_config" do
   begin
    config = ActiveRecord::Base.configurations[::Rails.env]
    ActiveRecord::Base.establish_connection
    case config["adapter"]
      when "mysql", "postgresql"
        ActiveRecord::Base.connection.tables.each do |table|
          ActiveRecord::Base.connection.execute("TRUNCATE #{table}")
        end
      when "sqlite", "sqlite3"
        ActiveRecord::Base.connection.tables.each do |table|
          ActiveRecord::Base.connection.execute("DELETE FROM #{table}")
          ActiveRecord::Base.connection.execute("DELETE FROM sqlite_sequence where name='#{table}'")
        end                                                                                                                               
       ActiveRecord::Base.connection.execute("VACUUM")
     end
    end
  end
end
```
### how do I subtract values from two select statements

http://stackoverflow.com/questions/2611174/how-do-i-subtract-values-from-two-select-statements

### SQL 
attr is not empty string
```
where.not(text_value: '')
where( "text_value <> ''" )
```
### combine two db search results
```ruby
s1 = Support.where(timer: 1) 
s2 = Support.where(prep: true)
s1 |= s2 # combine all records and uniq
s1 &= s2 # combine all same records
```
### Lib Folder In Rails

load code in the lib folder 

```ruby
#config/application.rb:
config.autoload_paths += %W(#{Rails.root}/lib)

#require a file under lib
require "easter"

#require a file under subdirectory of lib
require "shipping/airmail"
```
### Table Association With Non-index Foreign Key

Two talbes - players and battings, player has many battings, battings belongs to a player

```ruby
belongs_to :player, primary_key: :player_id, foreign_key: :player_id
has_many :battings, primary_key: :player_id, foreign_key: :player_id
```

### Agile Web Development

show table columns name

```ruby
Order.column_names
#=> ["id", "name", "address", "email", "pay_type", "created_at", "updated_at"]
```

show column detail
```ruby
Order.columns_hash["pay_type"]
#<ActiveRecord::ConnectionAdapters::SQLite3Column:0x00000003618228
#@name="pay_type", @sql_type="varchar(255)", @null=true, @limit=255,
#@precision=nil, @scale=nil, @type=:string, @default=nil,
#@primary=false, @coder=nil>
```

SQL
```ruby
#params[:order] = {name: "xxx", pay_type: "xxx"}
Order.where("name = :name and pay_type = :pay_type", params[:order])

# equl to 
Order.where(params[:order])

#wrong
User.where("name like '?%'", params[:name])

#correct
User.where("name like ?", params[:name]+"%")

#order
orders = Order.where(name: 'Dave').order("pay_type, shipped_at DESC")

#offset
def Order.find_on_page(page_num, page_size)
  order(:id).limit(page_size).offset(page_num*page_size)
end

#select
list = Talk.select("title, speaker, recorded_on")

#joins
LineItem.select('li.quantity').where("pr.title = 'Programming Ruby 1.9'").joins("as li inner join products as pr on li.product_id = pr.id")

#get staticstic
average = Order.average(:amount) # average amount of orders
max = Order.maximum(:amount)
min = Order.minimum(:amount)
total = Order.sum(:amount)
number = Order.count

#something mix
Order.group(:state).order("max(amount) desc").limit(3)
```

```ruby
orders = Order.find_by_sql("select name, pay_type from orders")

first = orders[0]

p first.attributes

p first.attribute_names

p first.attribute_present?("address")

#This code produces the following:

#{"name"=>"Dave Thomas", "pay_type"=>"check"}

#["name", "pay_type"]

#false
```

```ruby
order = Order.update(12, name: "Barney", email: "barney@bedrock.com")

result = Product.update_all("price = 1.1*price", "title like '%Java%'")
```

```ruby
def save_after_edit
  order = Order.find(params[:id])
    if order.update(order_params)
    redirect_to action: :index
  else
    render action: :edit
  end
end
```

```ruby
Order.destroy_all(["shipped_at < ?", 30.days.ago])
```

### AcitiveController

```ruby
resources :comments, except: [:update, :destroy]

resources :products do
  resources :reviews
end
```
### Migration

```ruby
rake db:migrate VERSION=20121130000009

rake db:migrate:redo STEP=3
```

change table name , include other changes related
```ruby
class CreateOrderHistories < ActiveRecord::Migration
class Order < ActiveRecord::Base; end

class OrderHistory < ActiveRecord::Base; end
  def change
    create_table :order_histories do |t|
      t.integer :order_id, null: false
      t.text :notes
      t.timestamps
    end
    
    order = Order.find :first
    OrderHistory.create(order: order_id, notes: "test")
  end
end
```

### Add Secret Key
in the console

```
echo "Twetter::Application.config.secret_key_base = '`bundle exec rake secret`'" > config/initializers/secret_token.rb
```
### MySQL

Fix the problem of storing Chinese char failed

http://stackoverflow.com/questions/9479607/rails-show-question-marks-for-my-input-utf8-data

### Try to know Stripe

https://stripe.com/docs/checkout/guides/rails

### Validation of Model

```ruby
validates_numericality_of :count, greater_than: 4

validates :start_at, time_period: { scope: :end_at }

validates :oprice_in_cents, :amount, numericality: true
```

### Test Database clean after and before each test

http://stackoverflow.com/questions/12220901/sqlite3sqlexception-when-using-database-cleaner-with-rails-spork-rspec

### carrierwave and minimagick work together 

example code:
https://github.com/brtr/group/commit/f442f751328a83a828bfae027e84586361d44720

### Thin run in background
```
thin -p 9292 -P tmp/pids/thin.pid -l logs/thin.log -d start
```

### DateTime Parse

http://stackoverflow.com/questions/18461798/rails-4-convert-datetime-into-separate-date-and-time-fields

### Form Helper Select

```ruby
<%= f.select q_param_name(:category), options_for_select(category_options, saved_selected_item(:category)), {prompt: 'All'}, {class: "form-control"} %>
```

### Hash can accept both sym and string key type

http://api.rubyonrails.org/classes/ActiveSupport/HashWithIndifferentAccess.html


### Get Model name in Controller

```ruby
controller_name.classify.constantize
```
### Get Controller Name in View

```ruby
controller.controller_name
```

### rake task with arguments

http://robots.thoughtbot.com/how-to-use-arguments-in-a-rake-task

### deploy with apache and passenger on nitrous.io

https://www.digitalocean.com/community/tutorials/how-to-setup-a-rails-4-app-with-apache-and-passenger-on-centos-6

1 install apache, use autopart tools

2 gem install passenger

```
gem install passenger
```

3 run console command 

```
passenger-install-apache2-module  
```
4 see the apache config file example -- passenger config part

```
</IfModule>                                                                                                                                                        
LoadModule passenger_module /home/action/.gem/ruby/2.1.1/gems/passenger-4.0.53/buildout/apache2/mod_passenger.so                                                   
<IfModule mod_passenger.c>                                                                                                                                         
  PassengerRoot /home/action/.gem/ruby/2.1.1/gems/passenger-4.0.53                                                                                                 
  PassengerDefaultRuby /home/action/.parts/packages/ruby2.1/2.1.1/bin/ruby                                                                                         
</IfModule>                                                                                                                                                        
                                                                                                                                                                   
<VirtualHost *:3000>                                                                                                                                               
  ServerName epoch-152230.use1-2.nitrousbox.com                                                                                                                    
  # !!! Be sure to point DocumentRoot to 'public'!                                                                                                                 
  DocumentRoot /home/action/workspace/www/epoch-newspaper-boxes/public                                                                                             
  <Directory /home/action/workspace/www/epoch-newspaper-boxes/public>                                                                                              
    # This relaxes Apache security settings.                                                                                                                       
    AllowOverride all                                                                                                                                              
    # MultiViews must be turned off.                                                                                                                               
    Options -MultiViews                                                                                                                                            
    # Uncomment this if you're on Apache >= 2.4:                                                                                                                   
    #Require all granted                                                                                                                                           
   </Directory>                                                                                                                                                    
</VirtualHost>  
```

5 precompile rails resources

```
rake assets:precompile RAILS_ENV=production
```

6 restart apache service





### Can't show pictures on Production Environment

```
#config/environments/production.rb
config.assets.compile = true
```




### Whenever and Schedule Job

http://eewang.github.io/blog/2013/03/12/how-to-schedule-tasks-using-whenever/



### Rake Task with Arguments

http://davidlesches.com/blog/passing-arguments-to-a-rails-rake-task

```ruby
namespace :seeder do
  desc "create random employees"
  task :seed, [:amount] => :environment do |task, args|
    puts "Your amount is #{args.amount}"
  end
end
```

```
rake seeder:seed[100]
```

### Multiple Languages Support
http://yangzb.iteye.com/blog/598150

### Fonts

http://anotheruiguy.roughdraft.io/7379570-custom-web-fonts-and-the-rails-asset-pipeline

### Production Env Setting

http://railsguides.net/how-to-define-environment-variables-in-rails/

### Read Attribute to prevent slack deep error / Call helper method in model

```ruby
def oprice
  ActionController::Base.helpers.number_to_currency(read_attribute :oprice)
end
```
============================================

### application.yml

config/application.rb
```
ENV.update YAML.load_file('config/application.yml')[Rails.env] rescue {}
```
