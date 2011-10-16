A simple website to echo foursquare user token from the URL hash. 

Why?  Foursquare doesn't seem to support access-token based oauth2, so for a CLI app you're going to need to get that user_token back from the web browser somehow.  This is a simple website that echoes it so the user can cut and paste.

### Example use in ruby

```ruby
#!/usr/bin/env ruby
require 'oauth2'
require 'foursquare2'
require 'launchy'

CLIENT_ID='1I3DQTBWVM1Z41CSQYBR1DAS3WTWJA2MFVRSXBQCEMENHN21'
CLIENT_SECRET='1UUG1BJBELIVEHWOJHSGMXO050IAZETDPUMBZ1U0CQ3I35QF'
$user_token=nil

if $user_token.nil?
  puts "No foursquare user token, go auth us and get one, sucker!"
  o_client = OAuth2::Client.new(CLIENT_ID, CLIENT_SECRET, {:site => 'https://foursquare.com/', :authorize_url => 'oauth2/authenticate'})
  auth_url = o_client.auth_code.authorize_url(:redirect_uri => 'http://mroth.github.com/foursquare-token-echo/', :response_type => 'token')
  Launchy.open(auth_url)
  print 'Okay, now gimmee ur token: '
  $user_token = gets.chomp
end
fs_client = Foursquare2::Client.new(:oauth_token => $user_token)
puts fs_client.user('self')
```
