---
layout: post
title: test
---

This is a test post. Please ignore.

Some things are ok, but this lecture is not. It is pretty boring.

{% highlight ruby %}
def show
    @widget = Widget(params[:id])
    respond_to do |format|
        format.html # show.html.erb
        format.json { render json: @widget }
    end
end
{% endhighlight %}