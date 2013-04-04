# ChalkDust

ChalkDust can be used to build activty feeds such as followings and friendships
by allowing models to subscribe to activity feeds published by other models.

Every time an activity occurs it is copied to all subscribers of the target of
that activity. This creates an activty feed per subscriber (more data) which
scales better and allows additional features such as feed to delete/hide
activity items.

[![Code Climate](https://codeclimate.com/github/krisleech/chalk-dust.png)](https://codeclimate.com/github/krisleech/chalk-dust)
[![Build Status](https://travis-ci.org/krisleech/chalk-dust.png)](https://travis-ci.org/krisleech/chalk-dust)

## Installation

Add this line to your application's Gemfile:

    gem 'chalk_dust'

## Usage

### Subscribing

A subscription connects two objects, e.g "Sam" follows "My first blog post"

```ruby
ChalkDust.subscribe(user, :to => post)
```

```ruby
ChalkDust.subscribers_of(post) # => [user, post]
```

#### Self-subscribing

Typically you will want to self-subscribe models on creation to themseleves so
an activity feed is built for the model itself.

```ruby
ChalkDust.self_subscribe(user)
# or
ChalkDust.subscribe(user, :to => user)
```

#### Undirected subscriptions

All subscriptions are directed. You can create a two way subscription, e.g a
friendship, as follows:

```ruby
ChalkDust.subscribe(bob, :to => alice, :undirected => true)
# or
ChalkDust.subscribe(bob,   :to => alice)
ChalkDust.subscribe(alice, :to => bob)
```

### Publishing activity

Describes an event where X (performer) did Y (activity) to Z (target).

```ruby
ChalkDust.publish_event(user, 'added', comment)
```

If the activity should be published to the feed of an object other 
than the target then it can be either be passed as an additional argument or
provided as a method called `activity_root`.

```ruby
ChalkDust.publish_event(user, 'added', comment, :root => comment.post)
```

or

```ruby
class Comment < ActiveRecord::Base
  belongs_to :post

  def activity_root
    post
  end
end
```

### Activity Feeds

```ruby
ChalkDust.activity_feed_for(user, :since => 2.weeks.ago)
ChalkDust.activity_feed_for(post, :since => 2.weeks.ago)
```

## Contributing

Contributions welcome, please fork and send a pull request.

## Thanks

Thanks to Igor @ Lifetrip Limited for allowing this code to be open sourced.

## License

Copyright (c) 2013 Lifetrip Limited

MIT License

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.