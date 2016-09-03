# README

This is a recipe application following the tutorial by [Mackenzie Child](https://mackenziechild.me/) in his
[12-apps-in-12-weeks](https://mackenziechild.me/12-in-12/) series.  
* The video for the tutorial is located here: [Week 3: How To Build A Recipe Box With Rails 4](https://mackenziechild.me/12-in-12/3/) and here: [Youtube](https://www.youtube.com/watch?v=QhdzE1yNs-0&list=PL23ZvcdS3XPLNdRYB_QyomQsShx59tpc-&index=3)
* Mackenzie's GitHub repo for this project is here: [recipe_box](https://github.com/mackenziechild/recipe_box)

####Things to look up:
* How does "recipe_params" work?  What is it doing?  How does permit work?
* What does 'resources :recipes' do for me in the config/routes.rb file?
* Explain how the delete link_to works.

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
* Q: so we added the update method - do we not need to define what @recipe is?

####~22:50 - 'delete' link_to

* Sublime fun fact: Command + W closes a file

####~28:00
* Just finished editing the application.html.erb file and turned it into a HAML file.  This is what it looks like:
```haml
!!! 5
%html
%head
	%title Recipe App
	= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' 
	= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' 
	= csrf_meta_tags

%body
	= yield
```
* With HAML, remember that indentation is important and I don't need the '=> true' part of the
* There is a difference between '-' and "=" in HAML

####~30:00
* add the paperclip gem
* these two lines added to the recipe.rb file (recipe model)
```ruby
has_attached_file :image, styles: { medium: "400x400#" }, default_url: "/images/:style/missing.png"
validates_attachment_content_type :image, content_type: /\Aimage\/.*\z/
```
```shell
rails db:migrate
```
* after adding paperclip, I updated the _form.html.haml to give myself a place to upload the image AND updated
the strong parameters to allow :image 
* show it in the show.html.haml and index.html.haml files

####~41:00
* learn how the image zoom works when you hover over it
```css
img {
	width: 100%;
	-webkit-transition: all .3s ease-out;
  -moz-transition: all .3s ease-out;
  -o-transition: all .3s ease-out;
  transition: all .3s ease-out;
	&:hover {
		transform: scale(1.075);
	}
}
```
####~41:50 - Cocoon gem

####~43:30
* create the Ingredient and Direction models:
```shell
rails generate model Ingredient name:string recipe:belongs_to
rails g model Direction step:text recipe:belongs_to
```
  * How does recipe:belongs_to differ from recipe:references?
* We created these models after installing cocoon but the cocoon gem GitHub tells us how to associate the two models
to recipe.rb.
```ruby
has_many :ingredients
accepts_nested_attributes_for :ingredients, reject_if: :all_blank, allow_destroy: true
has_many :directions
accepts_nested_attributes_for :directions, reject_if: :all_blank, allow_destroy: true
```
* add validation for presence in recipe model:
```ruby
validates :title, :description, :image, presence: true
```

####~52:45 
* at this point, there seems to be a problem with the validation.  Despite having text in the text-box for an 
ingredient or direction, it doesn't seem to recognize anything.  It keeps saying that there is an error...maybe I need
to restart the server?
* Cocoon does not appear to be compatible with Rails 5...maybe there is another way to build nested forms.  Look this up:
[http://guides.rubyonrails.org/form_helpers.html#nested-forms](nested forms)
### PAUSE
* I am going to hold for now until I get the nested forms to work.  I am not great with HAML and my instead convert the 
form into erb and do everything like that.

