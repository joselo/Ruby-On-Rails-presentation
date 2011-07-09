!SLIDE center transition=scrollRight

# MVC #

!['MVC'](MVC.png)

!SLIDE transition=scrollRight
#Model#
##Migrations##
!SLIDE transition=scrollRight code small

    @@@ruby
    class CreateProducts < ActiveRecord::Migration
      def self.up
        create_table :products do |t|
          t.string :name
          t.text :description
          t.timestamps
        end
      end

      def self.down
        drop_table :products
      end
    end
!SLIDE transition=scrollRight
#Model#
##Queries##
!SLIDE transition=scrollRight code smaller
    @@@ruby

    class Article < ActiveRecord::Base
      has_many :comments
      belongs_to :user
      validates_presence_of :name
    end
    article = Article.new(:name => "Ossconf2011", :user => @user)
    article.valid?
    Article.order("published_at desc").limit(10)
    Article.where("published_at <= ?", Time.now).includes(:comments)

    scope( :published,
	  lambda { where("published_at <= ?", Time.zone.now) }
	 )

!SLIDE transition=scrollRight

#Controller#
!SLIDE transition=scrollRight code smaller
    @@@ruby
    class UsersController < ApplicationController
      respond_to :html, :xml, :json

      def index
        @users = User.all
        respond_with(@users)
      end

      def show
        @user = User.find(params[:id])
        respond_with(@user)
      end
    end
!SLIDE transition=scrollRight

#View#

!SLIDE bullets transition=scrollRight
## XSS protection by default ##

!SLIDE code smaller transition=scrollRight

    @@@ruby
    = form_for @person do |f|
      content_tag(:h1) do
        = @person.title
      end
      = f.label :name, "Name"
      = f.text_field :name
      = f.label :profile, "Profile"
      = f.text_area :profile
      = f.submit
    end
***
    @@@html
    <form action="/posts" method="post">
      <h1>&lt;script&gt;more evil content here&lt;/script&gt;</h1>
      <label for="person_name">Name</label>
      <input type="text" name="person[name]" \
        id="person_name" value="Darth Vader" />
      <label for="person_profile">Profile</label>
      <textarea name="person[profile]" id="person_profile">
        &lt;script&gt;some evil content here&lt;/script&gt;
      </textarea>
      <input type="submit">Create</input>
    </form>

!SLIDE transition=scrollRight
## Helpers ##
!SLIDE bullets code smaller transition=scrollRight
    @@@ruby
    def my_helper(condition)
      if condition
        "do this"
      else
        "do that"
      end
    end
***
    @@@ruby
    %div.hey=my_helper(1)
***
    @@@html
    <div class="hey">do this</div>
!SLIDE transition=scrollRight
## Rails-LaTeX ###
!SLIDE bullets code smaller transition=scrollRight

### app/helpers/application_helper.rb: ###
    @@@ruby
    def lesc(text)
      LatexToPdf.escape_latex(text)
    end
***
### app/views/stories/show.html.erb: ###
    @@@ruby
    <%= link_to "print", story_path(@story,:format => :pdf) %>
!SLIDE transition=scrollRight
## ActionMailer ##
!SLIDE code smaller transition=scrollRight
    @@@ruby
    class UserMailer < ActionMailer::Base
      default :from => "admin@testapp.com"
      def welcome(user, subdomain)
        @user = user
        @subdomain = subdomain
        attachments['test.pdf'] =
        File.read("#{Rails.root}/public/test.pdf")
        mail(:to => @user.email,
             :subject => "Welcome to TestApp") do |format|
          format.html { render 'other_html_welcome' }
          format.text { render 'other_text_welcome' }
        end
      end
    end
!SLIDE transition=scrollRight
# Routing #
!SLIDE code smaller transition=scrollRight
    @@@ruby
    match ':controller(/:action(/:id(.:format)))'

***

    @@@ruby
    # login_url or login_path
    match '/login' => 'accounts#login', :as => 'login'

***

    @@@ruby
    # root_url or root_path
    root :to => 'blogs#index'
!SLIDE center transition=scrollRight

!["Every endpoint is essentially a Rack application"](rack-logo.png)

### "Every endpoint is essentially a Rack application" ###
> http://www.railsdispatch.com/posts/rails-routing

!SLIDE code small transition=scrollRight

    @@@ruby
    class CookieMonsterApp < Sinatra::Base

      get '/'
        "Me want cookie!"
      end

      get '/eat'
        "Om NOM NOM NOM"
      end

    end

    match '/cookies' => CookieMonsterApp

