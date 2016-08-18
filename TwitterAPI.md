I just set up a Twitter API for my profile app. The idea was to automatically send a tweet whenever I added a new blog post. This hit a couple o snags early on. Blog creation on this app is handled by rails admin (so that only I may make blog posts) but this means that there is no create posts action in the posts controller, which means in turn that I can't build the twitter functionality into the create posts action. I'm sure I could overwrite the Rails Admin controllers, but I decided it would be simpler to just set up a tweets controller and add a button accessible only to the admin that linked to it. So now I have a button on each post, visible only to me, which allows me to send the URL of the post out in a tweet.

Setting up the twitter api is surprisingly easy. I followed  [this blog post](http://blog.benmorgan.io/post/79339120263/how-to-use-the-twitter-api-for-ruby-on-rails).


Except that I am using the .env file to conceal the sensitive variables, do mine looks like:

    $twitter = Twitter::REST::Client.new do |config|
        config.consumer_key = ENV['TWITTER_CONSUMER_KEY']
        config.consumer_secret = ENV['TWITTER_CONSUMER_SECRET_KEY']
        config.access_token = ENV['TWITTER_ACCESS_TOKEN']
        config.access_token_secret = ENV['TWITTER_ACCESS_TOKEN_SECRET']
    end


I'm not crazy about making the rest client a global variable, but it definitely expedites the problem of how to get at the client from the controller.

From there, it's actually pretty simple to write the the message you want to send. client.update("Message") sends the tweet you want to send, and rails provides a method that gives you the url of the page you are being sent FROM (request.referrer). So really all you have to do is something like this:

    def create
        $twitter.update("I just wrote a new blog post which is available at #{request.referrer}")
        redirect_to :back
    end

And Voila, you've got it. I did run into trouble when I was testing with local host because twitter didn't read the localhost url as a real url,  but it worked fine when I pushed it to Heroku.
I'm feeling pretty pleased with myself this evening.
