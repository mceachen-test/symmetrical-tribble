---
ID: 942
post_title: 'HOWTO: Fix Ruby on Rails 2.3.x textarea Value Truncation'
author: matthew
post_date: 2010-10-07 11:04:56
post_excerpt: ""
layout: post
permalink: >
  http://matthew.mceachen.us/blog/howto-fix-ruby-on-rails-2-3-x-textarea-value-truncation-942.html
published: true
dsq_thread_id:
  - "160997933"
---
Turns out that Rails versions 2.3.6 through 2.3.8 have a <a href="https://rails.lighthouseapp.com/projects/8994/tickets/4808-textarea-input-silently-truncated-in-238">pretty horrid parameter-parsing bug</a>, and I'm surprised there hasn't been more hoopla about it.

If you have a textarea form that someone types a quote character into, the value you get back from <code>params[:key]</code> is truncated at the point of the quote.
<h3>Monkey patching to the rescue!</h3>
I decided to add a new <strong><code>raw_params</code></strong> method to <code>ActiveRecord::Request</code>, rather than fixing the bug and dealing with more potential side-effects, assuming this mess will be fixed in Rails 3. If it isn't, we should fix <code>ActionController::Base.param_parsers[:url_encoded_form]</code> to return the properly-parsed parameters from the raw_post.

In your rails application, create <tt>config/initializers/request.rb</tt>, with the following contents:

<!--more-->

<pre lang="ruby">
class ActionController::Request

  def raw_params
    @raw_params ||= begin
      raise "unsupported content_type" if content_type != "application/x-www-form-urlencoded"
      ActionController::Request.parse_raw_post raw_post
    end
  end

  def self.parse_raw_post raw_post
    h = {}
    raw_post.split('&').each do |s|
      k, v = s.split("=", 2)
      v = CGI.unescape v
      v.gsub!("\r\n", "\n")
      h[k.to_sym] = v
    end
    h
  end
end
</pre>

And remember, always feel guilty if you <code>git push</code> without a test. Here's <code>test/unit/request_test.rb</code> (which explains my using a class method, rather than an instance method). It'd be nice to have an integration test instead, but I'm using .haml templates that confuse rails integration tests. :-\

<pre lang="ruby">
require 'test_helper'
class RequestTest < ActiveSupport::TestCase
  test "raw_post parsing" do
    h = ActionController::Request.parse_raw_post("key=value&key2=%221+kw%22%0A%5B2+kw%5D%0A3+kw%0Afour")
    assert_equal "value", h[:key]
    assert_equal "\"1 kw\"\n[2 kw]\n3 kw\nfour", h[:key2]
  end
end
</pre>