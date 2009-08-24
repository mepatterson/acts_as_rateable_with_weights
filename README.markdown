Acts As Rateable (now, with Weights!)
================

This plugin allows rating on models. It's a fork from r0man's fork of the original
plugin developed by Juixe Software to work with newer versions of
Rails. The API has changed slightly. See the credits section for a
link to the original code.

[mepatterson]
I've modified r0man's fork to added a 'weight' field on the ratings table so
that a rating can be weighted.  If not used, a rating will default to a weight of 1.

I've also added the ability to look up a ratings "bayesian weight" in addition to its straight
average.  This gives you a rating that skews lower than its actual average rating if it has a low
number of votes compared to the average number of votes most entities are getting in your current
database. (in other words, if an entity gets one vote of 5 stars, that "5.0" average is not as
"believable" as an entity that gets 100 votes with an average of "4.5" and shouldn't be treated the same)

Installation
------------

Install the plugin:

        ./script/plugin install git://github.com/mepatterson/acts_as_rateable_with_weights.git

Usage
-----

Create a migration:

       ./script/generate acts_as_rateable_with_weights

Make your model rateable:

      class Article < ActiveRecord::Base
        acts_as_rateable_with_weights :range => (1..5)
      end

      article = Article.create(:text => "Lorem ipsum dolor sit amet.")
      alice, bob = User.create(:name => "alice"), User.create(:name => "bob")

      article.rate(1, alice, 3)
      article.rating # => 1

      article.rate(2, bob, 4)
      article.rating # => 1 (cached)
      article.rating(:force_reload => true) # => 1.57

Look at the tests to see more examples...

Credits
-------

Originally written by Juixe Software.

- <http://www.juixe.com/techknow/index.php/2006/07/05/acts-as-rateable-plugin>

Forked from a modified version by r0man.

---

Copyright (c) 2009 Roman Scherer, released under the MIT license
