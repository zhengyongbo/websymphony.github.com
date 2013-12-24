---
layout: post
title: "First Post"
date: 2011-09-03 21:32
excerpt: How I moved my blog from Posterous over to Octopress.
comments: true
---

So I have finally decided to move my blog from [posterous](http://websymphony.posterous.com) to [Octopress](http://octopress.org). Was planning on doing this for ages, just never got chance to go about it.
After moving to Posterous, I never really liked the way it was setup. Lack of [Disqus](http://disqus.com) commenting support, that annoying posterous header on top right corner on every page, generic looking low contrast themes, lack of enough control over the content and formatting. Just didn't feel right. So finally decided to take the plunge and take things back into my control.


Whole process of creating blog on Octopress was pretty straightforward, since documentation is pretty self explanatory. Only gotcha I ran into was to configure it with my domain using CNAME file. Had to modify the rakefile little bit to get that working.

Had to modify Rakefile

{% highlight ruby %}
desc "deploy public directory to github pages"
multitask :push do
	puts "## Deploying branch to Github Pages "
	(Dir["#{deploy_dir}/*"]).each { |f| rm_rf(f) }
	system "cp -R #{public_dir}/* #{deploy_dir}"
	puts "\n## copying #{public_dir} to #{deploy_dir}"
	cd "#{deploy_dir}" do
		system "git add ."
		system "git add -u"
		puts "\n## Commiting: Site updated at #{Time.now.utc}"
		message = "Site updated at #{Time.now.utc}"
		system "git commit -m '#{message}'"
		puts "\n## Pushing generated #{deploy_dir} website"
		system "git push origin #{deploy_branch}"
		puts "\n## Github Pages deploy complete"
	end
end
{% endhighlight %}

by adding:

{% highlight ruby %}
	f = File.new('CNAME', 'w')
	f.write ("websymphony.net\n");
	f.close
{% endhighlight %}

and after that redirects started working properly. Easy Peasy.

Octopress uses [Compass](http://compass-style.org) for styling, which makes changing of look and feel of the site a breeze. Overall am pretty happy with the system so far.
I have intentionally decided to not move over any of the old content from posterous to start afresh with a clean slate. Not having anything noteworthy on the old blog that people might miss has made the move lot more simpler.

Am planning on blogging on a regular basis from now on, atleast 2 times a week. Lets see how much success I have sticking to it.