---
layout: post
title:  "How I built my first ever Rails-6 engine - Developer Notes"
date:   2021-07-25 10:46:44 +0530
categories: developer-notes rails rails-engine
excerpt_separator: <!--more-->
---

These are my notes from when I began building a Rails engine. What I am creating here is a blog++ application. It is not a tutorial. Instead these are steps and stumbles along the journey to build my very first Rails Engine. My principle reason to learn engines was because I wanted to explore an alternative to microservices. However, I am going to keep this first engine simple and not build them as APIs just yet. That's because I want to avoid having to build a separate front end for the purpose of this exercise.

<!--more-->

# Step 1 - Setup

## Create a rails engine

Create a new engine in rails using the rails plugin command
```
rails plugin new engagers --mountable
``` 

The plugin generator in rails is used to create different types of plugins like Gems, Engines etc. The options  define what we build. `--full` option creates an engine with the full framework structures. This includes the `app` directory, the `config/routes` file and a `lib/engine-name/engine.rb` This file is similar to the `config/application.rb` file of a full . 

The `--mountable` option does all of what the full option does plus creates a separate, isolated namespace for the engine to operate in. It does a lot more that we can read about on [rails guides](https://guides.rubyonrails.org/engines.html)

### Optional - Add this rails engine to your main rails application

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


## Step 2 Generate the first few models

`bin/rails generate scaffold article title:string body:text permalink:string nature:integer status:integer excerpt:text`

This generated an error
```
The gemspec at /home/dev/projects/engagers/engagers.gemspec is not valid. Please fix this gemspec. (Gem::InvalidSpecificationException)
The validation error was '"FIXME" or "TODO" is not a description'
```
 
I was able to finx this when I replaced all the TODO and fill all the http-urls in the content_engager.gemspec file

## Step 3
Install the gems for mysql or sqllite3 and change the Gemfile of the engine by adding 
```
group :development do
  gem 'mysql2'
  gem 'sqlite3'
end
```

The reason we have to put sqlite3 is because the dummy app for testing in `/test/dummy` has a `database.yml` which has `sqlite database` defined in them. This dummy app essetially behaves like a host app where this engine could be added.

### Step 2 should now work, so execute it

Generate the scaffold for your very first model. 
```
bin/rails generate scaffold article title:string body:text permalink:string nature:integer status:integer excerpt:text
```

This creates all the usual files that a scaffold adds including the migrations etc. 

## Step 4

The next step is to run the migration of the engine in the main user-facing application. However `bin/rails db:migrate` in the root folder of the user-facing application does not make any difference

Rails provides a solution where once you have a fully-complete and ready engine, at the time of installation of that engine, you can copy all the migrations from the engine to the main user-facing app. This can be done by an out of box functionality provided by engines - 

```
bin/rails content_engager:install:migrations
```

With this the migrations from the engine are copied to the userfacing app. If we add newer migrations to the engine, then running this command again will only add newer migrations. If we've changed a migration in the engine while we are developing the engine (like the way I am doing) we'll just need to migrate the engine migrations down to `VERSION=0`. This can be achieved through the use of this command - 
```
  bin/rails db:migrate SCOPE=content_engager VERSION=0
``` 

#### Troubleshooting 
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

## Step 5 - Add markdown capabilities until we decide in favour of RichText

Now that we have the article model and keywords model, the immediate next thing would be to try out the basic tasks of writing a new article and displaying it. Lets start with adding a markdown or RichText. Here is an article by [k15t guys who argue that UserFacingApplications should choose RichTextFormating over Markdown](https://www.k15t.com/blog/2022/01/announcing-orderly-replace-confluence-page-properties-with-notion-like-databases). Confluence and Google Docs use RichTextFormating whereas Markdown is used on StackOverflow and other developer focused sites. 

Without really committing to one or the other, we did simply add markdown capabilities. The way we did that was by adding the `redcarpet` gem to our engine. In case of engines, we don't simply add gems to a gemfile. Instead, we have to add it to the engine's gemspec file. This file helps us specify to the UserFacingApp what gems that UserFacingApp must have. The `content_engager.gemspec` file now has this line appended to it.

`spec.add_dependency "redcarpet", ">= 3.5.1"`

However that alone did not help. We had to initialize the markdown rendering in our engine. We did this by adding a file called `content_marketing_engine/config/initiaizers/markdown_renderer.rb` in our engine. 

```
require 'redcarpet'

module MarkdownRenderer
 
  def self.markdown
    @@markdown ||=
      Redcarpet::Markdown.new(
        Redcarpet::Render::HTML.new(
          escape_html: true,
          hard_wrap: true,
          safe_links_only: true,
          with_toc_data: true
        ),
        autolink: true,
        fenced_code_blocks: true,
        no_intra_emphasis: true,
        space_after_headers: true,
        tables: true
      )
  end

  def self.render(text)
    markdown.render(text)
  end
end
``` 

We further added a require dependency to our `lib/content_engager/engine.rb`
`require_relative '../../config/initializers/markdown_renderer'`

## Step 6 - Add Bootstrap

We have the basic models in place. Next we wish to create the front end views that the users will be able to use to write articles. For this, we will be using `Bootstrap 5.1` as our front end UX framework. We do this by adding the following into our `content_engager.gemspec` file. 

`spec.add_dependency "bootstrap", ">= 5.1.3"`

But with webpacker as the default packaging system, gems alone is not sufficient. In fact gems are no longer the preferred way of adding third-party modules (even the bootstrap website suggests that we use webpacker for bootstrap). We've therefore decided to use webpacker to add bootstrap and its dependency which is `popper.js/core`. 

```
yarn install bootstrap
yarn install popper.js
```

After doing this we tried adding  `require bootstap` to application.css but that did not work. We were able to make it work by changing the file name from `application.css` to `application.scss` and then added the following to the header.

`import @bootstrap;` 

## Step 7 - Get Bootstrap's javascript to work

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

## Step 8 - Add javascript to Rails 6 views

With the bootstrap operational, the next step was to add specific javascript to run our engine. We therefore added a new javascript called `app/javascript/packs/articles.js`. This would contain all of the javascript we need to run and operate our editor. We tried two ways to add this to our rails engine but before I describe the two ways, here is the challenge that I faced. Inspite of the fact that I was making changes to the `app/javascript/packs/application.js`, webpacker wasn't picking up those changes. 

### Troubleshooting Webpacker

The first step was to restart the `webpacker-dev-server` by using this command

`bin/webpack-dev-server restart`

The best way to check if your changes have been picked up is to go a look at the webpack manifest file located at `pubic/content-engager-packs/manifest.json`. Ideally this file tells us about all the javascript that has been packaged by Webpacker. The challenge that we faced here was that even though the manifest.json file mentioned it, the actual javascript files weren't created in the `public/content-engager-packs/js` folder. Even after deletion and repeatedly restarting the webpacker-dev-server, the files weren't generated. [Seems this is a known error on OSX](https://stackoverflow.com/questions/26708205/webpack-watch-isnt-compiling-changed-files) and the only way I was able to get my compiled files to be generated was by completely deleting the public folder. On restarting the rails server, the webpack did serve all the files correctly. 

Next I added another file called `/app/javascript/packs/js/articles.js`. Now if it looks like this javascript is going to be used everywhere, then we can import it into application.js by `require(packs/article);` Alternatively we can simply add this to the articles view. Since most of this javascript is actually only for the editing and not so much for reading articles, I have added this javascript only to special views. I did this by adding the following line to file `app/views/layouts/content_engager/editor_layout.html.erb`. If you don't have this and are simply using the `application.html.erb` as your base layout, you can do add this line there as well.

  `<%= javascript_pack_tag 'articles' %>`

  
