!SLIDE
# Create a Gamebox game #

	@@@ bash 
		$ gamebox omg_aliens
		$ cd omg_aliens
		$ rake

!SLIDE
# Setting the Stage #

 src/demo_stage.rb
	@@@ ruby
	class DemoStage < Stage
	  def setup
	    super
	    @my_actor = create_actor :my_actor
		# ...

!SLIDE
# Setting the Stage #

 src/demo_stage.rb
	@@@ ruby
	target.fill [25,25,25,255]
	for star in @stars
  	  target.draw_circle_s([star.x,star.y],1,[255,255,255,255])
	end

!SLIDE
# Actors #
src/my_actor.rb
	@@@ ruby
	class MyActor < Actor
	end
## Actor is just a location by default ##

!SLIDE
# ActorViews #
src/my_actor.rb
	@@@ ruby
	class MyActorView < ActorView
  	  def draw(target, x_off, y_off)
	    target.draw_box [actor.x,actor.y], [20,20], [240,45,45,255]
		# ...

## ActorView doesn't exist by default ##

!SLIDE
# Behaviors #

audible.rb (gamebox src)
	@@@ ruby
	require 'behavior'
	class Audible < Behavior

	  def setup
	    @sound_manager = @actor.stage.sound_manager
		# ...
	
## modifies an actor; mostly adds methods ##