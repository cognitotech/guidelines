# Rubocop
### What
**RuboCop** is a Ruby static code analyzer. Out of the box it will
enforce many of the guidelines outlined in the community
[Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide).

### Why
* Make everybody write code with proper style, format and convention
* Prevent some code smells: long method, unuse variable, nested condition, wrong syntax ...
* Easy for new member to catch up with the team

### How
* Install `rubocop` gem
```bash
gem install rubocop
```
* Install editor plugin
  * SublimeText
    - Install [`Package Control`](#faq)
    - Install package `SublimeLinter`
    - Install package `SublimeLinter-rubocop`
  * Vim
  * Atom

### Configuration
Add `.rubocop.yml` to your project, in this file edit all config to meet your need ([here is the default config](https://github.com/bbatsov/rubocop/blob/master/config/default.yml)).

Here is a sample, max line length is 120 instead of 80:
```ruby
# Common configuration.
AllCops:
  # Include gemspec and Rakefile
  Include:
    - '**/*.gemspec'
    - '**/*.podspec'
    - '**/*.jbuilder'
    - '**/*.rake'
    - '**/Gemfile'
    - '**/Rakefile'
    - '**/Capfile'
    - '**/Guardfile'
    - '**/Podfile'
    - '**/Thorfile'
    - '**/Vagrantfile'
    - '**/Berksfile'
    - '**/Cheffile'
    - '**/Vagabondfile'
  Exclude:
    - 'vendor/**/*'

  RunRailsCops: true
  
Metrics/LineLength:
  Max: 120
```

If you enable Rails cops, consider add `schema.rb`, `migrations` (maybe) or some generated config into exclude section.

### Disable cops
In some rare case you want to disable the cops for one class or one method, you use comment to temporary disable the cops then renable them later. (take a look at default config file to find what you need to disable)

```ruby
# long line
# rubocop:disable Metrics/LineLength
a = super_long_long_long_long_long_long_long_long_long_long_long_long_long_long_long_long_long_long_long_long_long_long_long_method
# rubocop:enable Metrics/LineLength

# long method
# rubocop:disable Metrics/MethodLength
def long_method
  # super long method
end
# rubocop:enable Metrics/MethodLength

```

If you have some 3rd party code or generated file, you can disable all cops on them

```ruby
# rubocop:disable all
[third party code]
# rubocop:enable all
```

### FAQ
**Q**: How to install `Package Control` for SublimeText

**A**: Follow the instructions in this [link](https://sublime.wbond.net/installation)

**Q**: I can't find `SublimeLinter`, `SublimeLinter-rubocop`, ... in SublimeText package control

**A**: Try to reinstall (remove and then install) SublimeText package control using following steps
``` 
1. Click the Preferences > Browse Packages… menu
2. Browse up a folder and then into the Installed Packages/ folder
3. Remove `Package Control.sublime-package`
4. Restart SublimeText
5. Install `PackageControl` again
```

**Q**: I have installed all plugin but the linter didn't run in my editor

**A**: Check if you have rubocop installed?
```bash
$ rubocop -v
2.60.1
```
