# auto-session-timeout

Provides automatic session timeout in a Rails application. Very easy
to install and configure. Have you ever wanted to force your users
off your app if they go idle for a certain period of time? Many
online banking sites use this technique. If your app is used on any
kind of public computer system, this plugin is a necessity.

## Installation

Add this line to your application's Gemfile:

    gem 'auto-session-timeout'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install auto-session-timeout

## Usage

After installing, tell your application controller to use auto timeout:

    class ApplicationController < ActionController::Base
      auto_session_timeout 1.hour
      ...
    end

You will also need to insert this line inside the body tags in your
views. The easiest way to do this is to insert it once inside your
default or application-wide layout. Make sure you are only rendering
it if the user is logged in, otherwise the plugin will attempt to force
non-existent sessions to timeout, wreaking havoc:

    <body>
      <% if current_user %>
        <%= auto_session_timeout_js %>
      <% end %>
    </body>

You need to setup two actions: one to return the session status and
another that runs when the session times out. You can use the default
actions included with the plugin by inserting this line in your target
controller (most likely your user or session controller):

    class SessionsController < ApplicationController
      auto_session_timeout_actions
    end

To customize the default actions, simply override them. You can call
the render_session_status and render_session_timeout methods to use
the default implementation from the plugin, or you can define the
actions entirely with your own custom code:

    class SessionsController < ApplicationController
      def active
       render_session_status
      end
      
      def timeout
        render_session_timeout
      end
    end

In any of these cases, make sure to properly map the actions in
your routes.rb file:

    match 'active'  => 'sessions#active',  via: :get
    match 'timeout' => 'sessions#timeout', via: :get

You're done! Enjoy watching your sessions automatically timeout.

## Additional Configuration

By default, the JavaScript code checks the server every 60 seconds for
active sessions. If you prefer that it check more frequently, pass a
frequency attribute to the helper method. The frequency is given in
seconds. The following example checks the server every 15 seconds:

    <html>
      <head>...</head>
      <body>
        <% if current_user %>
          <%= auto_session_timeout_js :frequency => 15 %>
        <% end %>
        ...
      </body>
    </html>

## TODO

* current_user must be defined
* using Prototype vs. jQuery

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Resources

* Repository: http://github.com/pelargir/auto-session-timeout/
* Blog: http://www.matthewbass.com
* Author: Matthew Bass
