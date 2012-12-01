Heroku buildpack: Middleman
===========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for [Middleman](http://middlemanapp.com/) apps. It uses [Bundler](http://gembundler.com) for dependency management.

Usage
-----

In `Gemfile` have at least the following, commented out gems are useful, but not necessary.

```ruby
source :rubygems

ruby "1.9.3"

gem "thin"
gem "rack-contrib"

gem "rake"
gem "middleman"
# gem "middleman-smusher"
# gem "middleman-bourbon"
# gem "susy"
```

In `config.ru` have something along these lines (note: project-specific paths in here, change for your own project).

```ruby
require "rack/contrib/try_static"
require "rack/contrib/not_found"

use Rack::TryStatic,
  :root => "build",
  :urls => %w[/],
  :try  => [".html", "index.html", "/index.html"]

run Rack::NotFound.new("build/errors/404.html")
```

In `Procfile`, assuming you want to use Thin, add the following.

```sh
web: bundle exec thin start -p $PORT
```

With all that in place, create the application container on Heroku.

```sh
heroku create <appname> --buildpack git://github.com/meskyanichi/heroku-buildpack-middleman.git
```

Simply push your code, as always, and it'll build your app on Heroku and serve it through Rack.

```sh
git push heroku master
```

