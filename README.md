[gitter]: https://gitter.im/dry-rb/chat
[gem]: https://rubygems.org/gems/dry-auto_inject
[travis]: https://travis-ci.org/dry-rb/dry-auto_inject
[gemnasium]: https://gemnasium.com/dry-rb/dry-auto_inject
[code_climate]: https://codeclimate.com/github/dry-rb/dry-auto_inject
[inch]: http://inch-ci.org/github/dry-rb/dry-auto_inject

# dry-auto_inject [![Join the Gitter chat](https://badges.gitter.im/Join%20Chat.svg)][gitter]

[![Gem Version](https://img.shields.io/gem/v/dry-auto_inject.svg)][gem]
[![Build Status](https://img.shields.io/travis/dry-rb/dry-auto_inject.svg)][travis]
[![Dependency Status](https://img.shields.io/gemnasium/dry-rb/dry-auto_inject.svg)][gemnasium]
[![Code Climate](https://img.shields.io/codeclimate/github/dry-rb/dry-auto_inject.svg)][code_climate]
[![API Documentation Coverage](http://inch-ci.org/github/dry-rb/dry-auto_inject.svg)][inch]
![No monkey-patches](https://img.shields.io/badge/monkey--patches-0-brightgreen.svg)

A simple extensions which allows you to automatically inject dependencies to your
object constructors from a configured container.

It does 3 things:

- Defines a constructor which accepts dependencies
- Defines attribute readers for dependencies
- Injects dependencies automatically to the constructor with overridden `.new`

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'dry-auto_inject'
```

And then execute:

```sh
$ bundle
```

Or install it yourself as:
```sh
$ gem install dry-auto_inject
```

## Usage

You can use `AutoInject` with any container that responds to `[]`. In this example
we're going to use `dry-container`:

```ruby
# set up your container
my_container = Dry::Container.new

my_container.register(:data_store, -> { DataStore.new })
my_container.register(:user_repository, -> { container[:data_store][:users] })
my_container.register(:persist_user, -> { PersistUser.new })

# set up your auto-injection function
AutoInject = Dry::AutoInject(my_container)

# then simply include it in your class providing which dependencies should be
# injected automatically from the configured container
class PersistUser
  include AutoInject[:user_repository]

  def call(user)
    user_repository << user
  end
end

persist_user = my_container[:persist_user]

persist_user.call(name: 'Jane')
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/dry-rb/dry-auto_inject.

