---
layout: post
title: "Random Walk Header"
modified:
categories: 
tags: [site, javascript, probability, random walk]
date: 2014-10-20T18:40:29-04:00
---

I thought putting something on the [front page](/) that gave some indication of what I work on would be a good idea. So, I wrote a bit of javascript to generate a two dimensional random walk.  Nothing fancy, but it works.  

{% highlight javascript %}    
<script>
  width = $( window ).width();
  height = 260;
  x_pos = width/2;
  y_pos = height/2;
  step = .005*width;

  var canvas = document.getElementById('walk');
  canvas.setAttribute('width', width);
  canvas.setAttribute('height', height);

  function draw(){
    if (canvas.getContext){
      var ctx = canvas.getContext('2d');
      ctx.lineWidth = 2;
      ctx.strokeStyle = "#666";
      y_step = 0;
      x_step = step*(Math.floor(Math.random() * 3) - 1);
      if (x_step === 0){
        y_step = step*(Math.floor(Math.random()*3)-1);
      }
      ctx.beginPath();
      ctx.moveTo(x_pos, y_pos);
      x_pos = x_pos+x_step;
      y_pos = y_pos+y_step;
      ctx.lineTo(x_pos, y_pos);
      ctx.stroke();
    }
  }
  setInterval(draw, 6);
</script>
{% endhighlight %}

**Looking at the code, can you figure out whether or not this is a *simple* random walk or not?**  Simple means that the random walk has equal probability of jumping in any of the four directions: north, south, east, or west.  

###**Answer**:

It is *not* simple.  It's *biased*.  Here's how the walk decides where to go.  First, it randomly draws the step in the x-coordinate, which could be -1, 0, or 1.  If it's -1 or 1, then we don't bother drawing the y-coordinate step, as only one coordinate can change at a time (no moving diagonally).  So, there's a 2/3 chance we move in the x direction, and only a 1/3 chance we move in the y direction (if we pull a 0 on our first draw).  

I purposefully made the walk biased to move horizontally, because the space it can move is much wider than tall, and I wanted it to travel a bit in the gray area before it disappears out of the zone.