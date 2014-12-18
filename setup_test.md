# Setup Rspec, Guard and Spring
This guide will help you install `rspec`, with `spring` and `guard` to make spec run faster and automatically. You will love writing test again

**This guide use Ruby 2.1.4, Rails 4.1.6 and Rspec >=3**

## Rspec
To setup `rspec` in your project, add following line to your Gemfile

```ruby
group :development, :test do
  gem 'rspec-rails', '~> 3.0.0'
end
```

Run

```bash
$ bundle install
$ rails generate rspec:install
```

This adds the following files which are used for configuration:

- `.rspec`
- `spec/spec_helper.rb`
- `spec/rails_helper.rb`

**NOTE** remove `--warning` in `.rspec` (if you wonder why run `bundle exec rspec` once, there are a lot of unnecessary warning that we don't need).

Run `bundle exec rspec` to make sure everything work. Done :D

# Guard and Spring
Add following line to your Gemfile

```ruby
group :development do
  gem 'guard'
  gem 'guard-bundler'
  gem 'guard-rspec'
  # if you run rails > 4.1, you should have gem spring 
  # somewhere in your Gemfile. If not, uncomment next line
  # gem 'spring'
end

group :development, :test do
  # ...
  # This gem implements the rspec command for Spring.
  gem "spring-commands-rspec" 
end
```

```bash
$ bundle
$ bundle exec spring binstub rspec 
$ bundle exec guard init
```

Open `Guardfile`, change:

```ruby
guard :rspec, cmd: 'bundle exec rspec' do
```

into

```ruby
guard :rspec, cmd: 'bin/rspec' do
```

Now run `guard` and enjoy.

## FAQ:
- If you don't spring is working or not, run `spring status` to check
- You should use `bin/rake`, `bin/rails` or `bin/rspec` to make use of `spring` (too tired of typing, `direnv` may help you)
- If `guard` command complain something about `rspec` (after you binstub rspec), stop spring with `spring stop` then try `guard` again