In reference to the last post on dealing with git branching, the command to remotely delete your branch on Github turns out to be:

    git push origin --delete <branchName>
What I actually accomplished in my last experiment with git branching on Cream of the Crop was converting several of my views from Erb to Ruby-Slim (as well as handling some of the views through iteration). Slim, like Haml, is a templating language that generates html markup. The advantages of using such a language are obvious from looking at my views:

In Erb (I’m just going to go through one iteration; I wrote this without bothering to sit down and figure out how I might iterate through the views):

    <div class="row">
	<div class="box">
		<div class="col-lg-12">
			<hr>
			<h2 class="text-center">
				<strong>Your Wrestlers in the Main Event! </h2></strong>
				<h3 class ="intro-text text-center"><u>Babyfaces (Tecnicos)</u> </h3>
			<%unless @main_event_babyfaces.blank?  %>
			<% @main_event_babyfaces.each do |wrestler|  %>
					<div class="col-sm-4 text-center">
						<%=image_tag wrestler.image.url, class: "img-responsive"  %>
					<div class="btn-group pull-right wrestler-options-group">
						<button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown"
										aria-haspopup="true" aria-expanded="false">
							<i class="glyphicon glyphicon-cog"></i>
							<span class="caret"></span>
						</button>
						<ul class="dropdown-menu">
							<li><%= link_to 'Wrestler Profile', wrestler%></li>
							<li><%= link_to 'Change Details', edit_wrestler_path(wrestler) %></li>
							<li><%= link_to 'You\'re Fired', wrestler, method: :delete, data: { confirm: 'Are you sure?' } %></li>
						</ul>
					</div>
						<h3 class="text-left"><%= wrestler.name %></h3>
					</div>
					<%end  %>
			<%else  %>
				<h4 class ="intro-text text-center">
				You have no babyface wrestlers able to compete in the Main Event! <%= link_to 'Hire a new wrestler', new_wrestler_path %><br>
					Turn a heel face! Turn a face heel! Promote somebody! Alternatively
				</h4><br>
					<h1 class="text-center"> SHOVE THAT CONTROL INTO A NOSE DIVE!!!</h1>
			<%end  %>
			</div>
		</div>
	</div>
Versus the Slim version:

    - (0..4).each do |position|
			.row.box
				.col-lg-12
					hr
					.intro-text.text-center
						h2 Your Wrestlers in the #{@positions[position]}!
						- (0..1).each do |alignment|
							- index = (position*2 + alignment)
							.row.box
								.col-lg-12
								hr
									h3.intro-text.text-center
										u = @alignments[alignment]
										hr
									- unless @wrestler_categories[index].blank?
										- @wrestler_categories[index].each do |wrestler|
											.col-sm-4.text-center
												= image_tag wrestler.image.url, class: "img-responsive"

												.btn-group.pull-right.wrestler-options-group
													button.btn.btn-default.dropdown-toggle type="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false"
														i.glyphicon.glyphicon-cog
														span.caret
													ul.dropdown-menu
														li = link_to 'Wrestler Profile', wrestler
														li = link_to 'Change Details', edit_wrestler_path(wrestler)
														li = link_to 'You\'re Fired', wrestler, method: :delete, data: { confirm: 'Are you sure?' }
												h3.text-left = wrestler.name
									- else

										h4.intro-text.text-center
											| You have no babyface wrestlers able to compete in the Main Event! #{link_to 'Hire a new wrestler', new_wrestler_path}
											br
											| Turn a heel face! Turn a face heel! Promote somebody! Alternatively
											br
										h1.text-center SHOVE THAT CONTROL INTO A NOSE DIVE!!!
The limitations of WordPress, make it a bit hard to tell, but the second one is much simpler.
A couple of tricks that I eventually picked up:

You can chain classes together with ‘.”s. This is crucial because having a class on the same indent level closes out the previous div, so if you need something with multiple classes that’s pretty much the only way you can do it.
Anything you do with a rails helper, like ‘image_tag’ or ‘link_to’, you have to manipulate it’s class in the old way, i.e. ‘, class = “”‘.
One last point is whether to use Slim or Haml. Haml is more popular, and that’s kind of an argument in and of itself. As far as I can tell, the only substantial difference between Haml and Slim is that Haml has a ‘%’ in front of all the templating commands. It’s a small difference, but if we are trying to make our code minimal and increase readability, then surely Slim’s lack of a ‘%’ everywhere accomplishes our goal better than Haml does.
