# A list of importance gem
Here are the list of gems that you want to use in your project.

## Development Helper
These gem will give you a better delopment enviroment, consider to use them all

### [awesome_print](https://github.com/michaeldv/awesome_print)
Awesome Print help outputing the object structure in a clean and readable way. It's super helpful for debug and inspect code.

![awesome_print](https://www.evernote.com/shard/s232/sh/12166c24-e9df-46c6-ad25-31ed5b397611/0243fb17ed3dab3be4ef3a98f5920496/res/b410a546-cfdd-4b28-9a08-2fdbd50d5490/skitch.png?resizeSmall&width=832)

Usage:
```ruby
hash = { username: 'vinh', email: 'ducvinh@siliconstraits.vn' }
ap hash
```
If using `pry` as default interactive shell for Ruby, put the following in `~/.pryrc`, all output in pry will automatically use Awesome Print
```ruby
require "awesome_print"
AwesomePrint.pry!
```

### [better_errors](https://github.com/charliesome/better_errors) & [binding_of_caller](https://github.com/banister/binding_of_caller)
Better Errors replaces the standard Rails error page with a much better and more useful error page. It has a live shell so you can find out what the problem in your page. [binding_of_caller](#binding_of_caller)
![better_errors](https://camo.githubusercontent.com/3fa6840d5e20236b4f768d6ed4b42421ba7c2f21/68747470733a2f2f692e696d6775722e636f6d2f367a42474141622e706e67) is needed to enable [better_errors](#better_errors) advanced feature (REPL, local/instance variable inspection, pretty stack frame names)
```ruby
group :development do
  gem 'better_errors'
  gem 'binding_of_caller'
end
```

### [meta_request](https://github.com/dejan/rails_panel/tree/master/meta_request)
You need to install Rails Panel extension on [Chrome WebStore](https://chrome.google.com/webstore/detail/railspanel/gjpfobpafnhjhbajcjgccbbdofdckggg) to use this gem.
![meta_request](https://cloud.githubusercontent.com/assets/4494/3090049/917e5378-e586-11e3-9bd4-1db232968126.png)
This will show you the infomation about the request, sql query, log, error ... in your browser.

### [quiet_assets](https://github.com/evrone/quiet_assets)
Quiet Assets turns off the Rails asset pipeline log. Now development log only contains importance information about your request data, sql query ... No more
```
Started GET "/assets/application.js?body=1" for 127.0.0.1 at 2012-02-13 13:24:04 +0400
Served asset /application.js - 304 Not Modified (8ms)
```

### [spring](https://github.com/rails/spring)
Spring is a Rails (official) application preloader. It speeds up development by keeping your application running in the background so you don't need to boot it every time you run a test, rake task or migration.

```ruby
  group :developement do
    gem 'spring'
  end
```
```bash
$ bundle install
$ bundle exec spring binstub --all
```

Now, use `bin/rake` or `bin/rails` instead of `rake` or `rails` to get the performance boost.

### [pry-rails](https://github.com/rweng/pry-rails)
Need to debug somewhere in your app (models, controllers, views or even gems), puts `binding.pry` to that line and debug for apps.

```ruby
group :developement, :test do # maybe you will need in when run test too
  gem 'pry-rails'
end
```

### [rack-mini-profiler](https://github.com/MiniProfiler/rack-mini-profiler)
This gem help profiling every request and giving you the result in a mini popup on top corner (can be configured). It let you know which part of your request consume too much time and detect unoptimized queries.

![rack-mini-profiler](http://i.imgur.com/PsjLY.png)

To hide the rack-mini-profiler popup, press `Alt / Option + p` or add params `pp=disable` to url.

### [did_you_mean](https://github.com/yuki24/did_you_mean)

Act like Google "Did you mean?" feature, it provide some suggestion when you accidentally use a wrong method or class name, super helpful for development.

![did_you_mean](https://camo.githubusercontent.com/3e15b35252cc585660699e447a438722261b0506/68747470733a2f2f726177322e6769746875622e636f6d2f79756b6932342f6469645f796f755f6d65616e2f346536643462343035373737663434656362326130653238633461626231653433386161333366662f646f63732f6265747465725f6572726f72735f6578616d706c652e706e67)

## Application Feature
## Testing
