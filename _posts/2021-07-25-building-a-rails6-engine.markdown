
---
layout: post
title:  "How I built my first ever Rails-6 engine - Developer Notes"
date:   2021-07-25 10:46:44 +0530
categories: developer-notes rails rails-engine
excerpt_separator: <!--more-->
---



# Step 1 - Setup

## Create a rails engine

Create a new engine in rails using the rails plugin command
```
rails plugin new engagers --mountable
``` 

The plugin generator in rails is used to create different types of plugins like Gems, Engines etc. The option infront defines what we build. `--full` option creates an engine with the full framework structures. This includes the `app` directory, the `config/routes` file and a `lib/engine-name/engine.rb` This file is similar to the `config/application.rb` file of a full . 

The `--mountable` option does all of what the full option does plus creates a separate, isolated namespace for the engine to operate in. It does a lot more that we can read about on [rails guides](https://guides.rubyonrails.org/engines.html)

## Optional - Add this rails engine to your main rails application

When we create a rails engine, the engine does provide us with a dummy test application to test this engine. Instead of using the dummy application to test the engine I'll use my main user facing app to test this. 

Rails guide says that we can do this by adding this line in the Gemfile of the MainUserFacing application

`main-user-facing-app/Gemfile`

```
gem 'content_engager', path: 'engines/content_engager'
```

Howwever this only works if you're building your engine within your main application. If that's the case, your structure would look like this 

If we were creating an engine called admin, the structure would look like this - 

```
|-app/
|-bin/
|-...
|-config/
  |-application.rb
  |-routes.rb
|-engines/
  |-admin/
    |-app/
      |-controllers/admin/
          |- foo_controller.rb
    |-config/
      |-routes.rb # <- this is engine's routes
    |-lib/
      |-admin/
        |-engine.rb
    |- admin.gemspec
``` 

Instead, if you'd like to continue building the engine separately, you can do so by adding the path to the folder of this engine-application

`gem 'content_engager', path: '../content-marketing-engine/'`


# --Step 2 Generate the models --

`bin/rails generate scaffold article title:string body:text permalink:string nature:integer status:integer excerpt:text`

This generated an error
```
The gemspec at /home/dev/projects/engagers/engagers.gemspec is not valid. Please fix this gemspec. (Gem::InvalidSpecificationException)
The validation error was '"FIXME" or "TODO" is not a description'
```

# Step 2 
Replace all the TODO and fill all the http-urls in the content_engager.gemspec file

# Step 3
Install the gems for mysql or sqllite3 and change the Gemfile of the engine by adding 
```
group :development do
  gem 'mysql2'
  gem 'sqlite3'
end
```

The reason we have to put sqlite3 is because the dummy app for testing in `/test/dummy` has a `database.yml` which has `sqlite database` defined in them. This dummy app essetially behaves like a host app where this engine could be added.

# Step 4

Generate the scaffold for your very first model. 
```
bin/rails generate scaffold article title:string body:text permalink:string nature:integer status:integer excerpt:text
```

This creates all the usual files that a scaffold adds including the migrations etc. 

# Step 5

The next step is to run the migration of the engine in the main user-facing application. However `bin/rails db:migrate` in the root folder of the user-facing application does not make any difference

Rails provides a solution where once you have a fully-complete and ready engine, at the time of installation of that engine, you can copy all the migrations from the engine to the main user-facing app. This can be done by an out of box functionality provided by engines - 

```
bin/rails content_engager:install:migrations
```

With this the migrations from the engine are copied to the userfacing app. If we add newer migrations to the engine, then running this command again will only add newer migrations. If we've changed a migration in the engine while we are developing the engine (like the way I am doing) we'll just need to migrate the engine migrations down to `VERSION=0`. This can be achieved through the use of this command - 
```
  bin/rails db:migrate SCOPE=content_engager VERSION=0
``` 

### Troubleshooting 
The `rails db:migrate` in the UserFacingApp failed with error `Failed to open the referenced table 'keywords`. This happened because we added a join  table with the `references` keyword. Now when the migration run, they look for the table with the model we are referencing. 

The way we solve for that is through the  `add_foreign_key` directive. Avoid the `references` 

```
class CreateEngineNameArticlesKeywords < ActiveRecord::Migration[6.1]
  def change
    create_table :engine_name_articles_keywords do |t|
      t.integer :keyword_id, null: false, foreign_key: { to_table: :engine_name_keywords }
      t.integer :article_id, null: false, foreign_key: { to_table: :engine_name_articles }

      t.timestamps
    end
  end
end
```

# Step 6

Now that we have the article model and keywords model, the immediate next thing would be to try out the basic tasks of writing a new article and displaying it. Lets start with adding a markdown   

# Step 6

We have the basic models in place. Next we wish to create the front end views that the users will be able to use to write articles. For this, we will be using `Bootstrap 5.1` as our front end UX framework. We do this by adding the following into our `content_engager.gemspec` file. 

`spec.add_dependency "bootstrap", ">= 5.1.3"`

But with webpacker as the default packaging system, gems alone is not sufficient. In fact even if we go to the bootstrap website, it suggests that we use webpacker for bootstrap. We've therefore decided to use webpacker to add bootstrap and its dependency which is `popper.js/core`. 

```
yarn install bootstrap
yarn install popper.js
```

After doing this we tried adding  `require bootstap` to application.css but that did not work. We were able to make it work by changing the file name from `application.css` to `application.scss` and then added the following to the header.

`import @bootstrap;`

# Step 7

Next, we want to use the bootstrap javascript and added this line `<%= javascript_pack_tag 'application' %>` to application.html.erb 

Unfortunately, that doesn't work in case of an engine. For all new Rails 6 apps, webpacker is already added as default but for Rails Engine, webpacker is not available. Instead, Rails Engine are still expected to use Sprockets for packaging its javascript and css. The engines do come with a `app/assets/config/content_engager_manifest.js` file. I tried to import the bootstrap javascripts into this js file but that did not work. It wouldn't have because this manifest.js file is created by sprockets. 

In order to create manifest.js using sprockets, we added the command to import bootstrap to `app/content_engager/assets/javascript.js` and compiled the sprockets using `bin/rails app:assets:precompile --trace`. However, it still did not add bootstrap.js into my manifest.js. Not sure but it seems Rails 6 uses sprockets for stylesheets and webpacker for javascript assets. 

In any case, that got us thinking that we should probably try and get webpacker running for the engine. Somehow Rails hasn't yet setup a way for Webpacker to be installed into a rails engine. Adding the webpacker gem and running 'bin/rails webpacker:install' only generates an error. 

We added webpacker to a rails engine by following several links. This [github gist](https://gist.github.com/pioz/805d0887ebfdde6322c4db4b46b0120f) is incorrect, especially the task that they use to compile webpacker assets. Instead, [this post by Ben Vandgrift](http://ben.vandgrift.com/posts/rails-engine-webpacker-1/) and especially the [github repository](https://github.com/bvandgrift/saddlebag) was what eventually got us through. 

Another challenge that we faced was that several posts talk about adding two seperate bootstrap files. Js and CSS. Instead it only worked after we added this to our `app/javascript/packs/application.js` 

`import 'bootstrap/dist/js/bootstrap.bundle';`

With that, we were able to get the webpacker running and all javascripts were now available to our engine. The good thing is that webpacker serves a different pack for our engine and it doesn't contaminate the UserFacingApp's javascripts.

While `import bootstrap/dist/js/bootstrap.bundle` worked, using `require` also works. Therefore the final `app/javascript/packs/application.js` looked like this - 

`require('bootstrap/dist/js/bootstrap.bundle');`


This got our bootstrap javascript operational.

# Step 

With the bootstrap operational, the next step was to add specific javascript to run our engine. We therefore added a new javascript called `app/javascript/articles.js`. This would contain all of the javascript we need to run and operate our editor. We tried two ways to add this to our rails engine. 

The first was to add it to our 
