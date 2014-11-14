#Form_helpers

First you want to get your rails app started and to set up the routes easily with resources, you want to do:

```
resources :controller_name
```
If you wanted to have RESTful routing and a direct relationship between models, you can nest your resources like so:

```
resources :controller_name do
	resources :related_controller
end
```
For form_helpers you can create an instance variable in your controller like so:
```
@player = Player.new
```
And in your form you can pass it in so that the form knows what model you are talking about with the form, like so:

```
<%= form_for @player do |f| %>
```
If you need to use PUT or DELETE, you can specify it in the form:
```
<%= form_for @player, method: "put" do |f| %>
```
Sometimes you need to specify the path as well:
```
<%= form_for @team, url: new_team_player_path(@team.id), method: "delete" do |f| %>
```
An alternate, much simpler way to write this is like so:
```
<%= form_for [@team, @player] do |f| %>
```
If you want to refactor your Controller code, for instance if you are created the same instance variable in multiple methods, you can do this at the top of the controller:
```
before_filter :assign_team, only: [:new, :edit]
```
You can name the `:assign_team` anything, and in the brackets put which methods you want to give the variable to. In a private section at the bottom of your controller, you define a method to create that instance variable like so:
```
private
	def assign_team
		@team = Team.find(params[:team_id])
	end
```
To protect form data, you should make your params in a private method as well, like so:
```
def player_params
		params.require(:player).permit(:name, :jersey)
end
```
And when you are creating or updating you would pass it in like so:
```
@player.update(player_params)
```
And if you need to pass in a reference id, you can do a merge like so:
```
Player.create(player_params.merge(team_id: params[:team_id]))
```
This is merging in a hash with a key and value pair. This essentially replaces three steps of finding the team, creating the player without the team reference and then running `team.players << player`
	