!SLIDE
# Enough talk! Let's make something! #

## What should we build? ##

!SLIDE center
<embed style='display:block' src='http://media.mtvnservices.com/mgid:cms:item:comedycentral.com:162725' width='1080' height='903' type='application/x-shockwave-flash' wmode='window' allowFullscreen='true' flashvars='autoPlay=false' allowscriptaccess='always' allownetworking='all' bgcolor='#000000'></embed>

!SLIDE
# Create the player #

	@@@ bash
	$ ./script/generate actor Player
	
# creates the actor and spec

!SLIDE incremental
# Player to the Stage #

## change demo_stage.rb to create :player instead of :my_actor ##
## have Player spawn at 200, 550 ##

!SLIDE
# answer #
	@@@ ruby
	@player = spawn :player, :x => 200, :y => 550

!SLIDE
# Uncloak the Player #
## create an ActorView, nah ##
## use canned :graphical behavior from Gamebox ##
## copy player.png to data/graphics ##

!SLIDE
# answer #
	@@@ ruby
	has_behavior :graphical

!SLIDE
# Undock the ship #
## use input_manager.while_key_pressed to monitor a key down ##

!SLIDE
	@@@ ruby
	attr_accessor :move_left, :move_right
	def setup
	  i.while_key_pressed :left, self, :move_left
	  i.while_key_pressed :right, self, :move_right

!SLIDE
## now to use those variables ##
## add the :updatable behavior ##
## define update(time) method that modifies position ##

!SLIDE
	@@@ ruby
	has_behaviors :graphical, :updatable
	def update(time_delta)
      velocity = 0.12 * time_delta 
      @x += velocity if @move_right
      @x -= velocity if @move_left
    end

!SLIDE
# Player needs to shoot #

	@@@ bash
	$ ./script/generate actor FrickinLaser
	$ ./script/generate actor_view FrickinLaserView 

!SLIDE
# FrickinLaser needs to update its position #

!SLIDE
	@@@ ruby
	has_behavior :updatable
 
  	def update(time_delta)
      velocity = 4*0.1*time_delta
      @y -= velocity

      remove_self if @y < 30
    end

!SLIDE
# FrickinLaserView needs to draw a box #
	@@@ ruby
	def draw(target, x_offset, y_offset)

* target is a Rubygame screen
* x_offset, y_offset viewport offsets (ignore for now)

!SLIDE
# Draw it #
	
	@@@ ruby
	target.draw_box_s([x,y], [x2,y2], color_array)
## use the actor's x and y ##
## color is RGBA array ##

!SLIDE
# Get a FrickinLaser on the Stage #

!SLIDE
# Back to shooting .. #
## Player#setup should use input_manager#reg to shoot on KeyPressed, :space ##
## Player#shoot should spawn :frickin_laser at player's location ##

!SLIDE
# Get the key press #
	@@@ ruby
	input_manager.reg KeyPressed, :space do
  	  shoot
	end

!SLIDE
# Shoot the FrickinLaser #
	@@@ ruby
	def shoot
	  laser = spawn :frickin_laser, 
	    :x => @x + (image.width/2), :y => @y
	end
	
!SLIDE
# What kind of silent laser is that? #

## copy shoot.wav into data/sounds ##
## add :audible behavior to Player ##
## play_sound takes the name of the file as a sym ##	
	
!SLIDE
	@@@ ruby
	has_behaviors :audible, :graph...
	play_sound :shoot

!SLIDE
# Create Alien actor #
## Now let's shoot him! ##

!SLIDE
# Gamebox Collisions #
## circles only, for now ##
## :collidable behavior (takes args) ##
	@@@ ruby
	:collidable => {:shape => :circle, 
		:radius => 20}


!SLIDE bullets incremental
# Who should collide? #

* Player
* FrickinLaser
* Alien

!SLIDE 
# Add those behaviors #

!SLIDE
# what to do when things collide? #

## setup in our stage says what to do ##
## remove_self on alien and laser ##
## play the :death sound ##

!SLIDE
	@@@ ruby
	on_collision_of :alien, :frickin_laser do |alien,laser|
	  alien.remove_self
	  laser.remove_self
	  sound_manager.play_sound :death
	end

!SLIDE bullets small
# Explore #

* add more sounds
* aliens shoot back (add_timer)
* add scores, backgrounds
* add multiple waves, lives
* add ufos
* anything!
