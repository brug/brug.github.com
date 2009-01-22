---
layout: post
title: Latent semantic indexing
---

# {{page.title}}

If you were wondering how the 'Related posts' are generated it's via this pleasingly short bit of code - lines 105 to 125 of [jekyll/lib/jekyll/post.rb][1] - using the [Classifier gem][2].

[1]: http://github.com/mojombo/jekyll/blob/4b2fa43642056b30c514ed2c8d9a1974c64b549a/lib/jekyll/post.rb
[2]: http://www.rubyinside.com/classifier-gem-bayesian-and-lsi-classification-for-ruby-507.html

{% highlight ruby %}
# Calculate related posts.
#
# Returns [<Post>]
def related_posts(posts)
  return [] unless posts.size > 1
  
  if Jekyll.lsi
    self.class.lsi ||= begin
      puts "Running the classifier... this could take a while."
      lsi = Classifier::LSI.new
      posts.each { |x| $stdout.print(".");$stdout.flush;lsi.add_item(x) }
      puts ""
      lsi
    end

    related = self.class.lsi.find_related(self.content, 11)
    related - [self]
  else
    (posts - [self])[0..9]
  end
end
{% endhighlight %}