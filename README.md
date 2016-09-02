# README

This is a recipe application following the tutorial by [Mackenzie Child](https://mackenziechild.me/) in his
[12-apps-in-12-weeks](https://mackenziechild.me/12-in-12/) series.  
* The video for the tutorial is located here: [Week 3: How To Build A Recipe Box With Rails 4](https://mackenziechild.me/12-in-12/3/) and here: [Youtube](https://www.youtube.com/watch?v=QhdzE1yNs-0&list=PL23ZvcdS3XPLNdRYB_QyomQsShx59tpc-&index=3)
* Mackenzie's GitHub repo for this project is here: [recipe_box](https://github.com/mackenziechild/recipe_box)

####Things to look up:
* How does "recipe_params" work?  What is it doing?  How does permit work?
* What does 'resources :recipes' do for me in the config/routes.rb file?

####~3:00
* Completed the rails new recipe_box command and initialized git...ready to start to developing!

* install HAML gem

####~5:30
* rails generate controller recipes
* create index action in recipes controller and update routes file
* building first view: index.html.haml
* create blank show, new and create actions in recipes controller
* he defined a private method to find the recipe and then a before_action :find_recipe for some of the actions we
have created or will create
* we created the new and create actions as well as the recipe_params method which is private.

####~10:30
* rails generate model Recipe title:string description:text user_id:integer
* rails db:migrate
* we then try to go to localhost:3000/recipes/new and we get an error because there is no view!
  * Just to recap...thus far, we have created a recipes controller (rails generate controller recipes) and a recipe
  model (rails generate model Recipe ...) and in the app/views/recipes folder, the only view we have is the one that
  we created ourselves (index.html.haml).
  * So scaffolding creates those views??
* add new.html.haml and _form.html.haml
* add simple_form gem and bundle install

####~16:00
* I struggled to get the HAML to work...indentation is a big deal.  My panel-body was indented too deep and thus hidden
inside the errors tag.

####~17:50
* Got the form to work but there was no show template so we created one.  Here is what show.html.haml looks like:
```haml
%h1= @recipe.title
%p= @recipe.description

= link_to "Back", root_path, class: "btn btn-default"
```
* update recipes_controller with completed index action
* update index.html.haml file to loop through all the recipes and create a link for the title and show the text.  This step 
is not available on the GitHub repo so here is how he accomplishes that:
```haml
- @recipe.each do |recipe|
	%h2= link_to recipe.title, recipe
	%p= recipe.description
```
####~19:40
* add edit, update and destroy actions (blank as of now)
