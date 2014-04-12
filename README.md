jekyll-DiscourseBestOf
======================

>##Hey! You should check out the [official way to do this.][8] This script probably still works, but I'm no longer maintaining it.

The ruby component to https://github.com/trident523/js-discourseBestOf.

    add_header "Access-Control-Allow-Origin" "*";

This allows all sites to load your content via javascript. If you would like, replace the * with the public location of your jekyll install, including http://. The security of this is debated all over, but I find it safe. For more security, add the header only to your json files using a location block!


<h3>Preparing Octopress</h3>

This plugin requires httparty, a cool gem that makes HTTP requests easy. It also requires json, which is likely installed. We'll double check by adding it anyway. To install it, simply remove your `Gemfile.lock`, and to your `Gemfile` add the lines below, and run a bundle install. You can't just use gem install to do it.

    gem 'httparty'
    gem 'json'


<h3>Preparing Jekyll</h3>
*If you didn't install httparty and json above, do it here with:*

    gem install httparty json

You'll need the plugin files. Using some cool git submodules, we can add them in where needed and you can update on demand. For octopress, these are the paths, assuming you are in your octopress folder.

    git submodule add https://github.com/trident523/jekyll-DiscourseBestOf.git plugins/discourse
    git submodule add https://github.com/trident523/js-discourseBestOf.git source/javascripts/js

For just jekyll:

    git submodule add https://github.com/trident523/jekyll-DiscourseBestOf.git _plugins/discourse
    git submodule add https://github.com/trident523/js-discourseBestOf.git javascripts/js

Now, if you want the latest version just run git submodule update. Easier than it used to be! Remember when updating that the javascript file will need the correct URL.


### Javascript Guide ###

* Edit your ```/source/_includes/custom/head.html```  file to include this script. jQuery is included with Octopress. As long as it's placed on your posts page it should work, but it'd be best to put it in the header. Here's an example, assuming you placed the files in the source folder. 

```diff
+<script src="//code.jquery.com/jquery-2.0.0.js"></script> *if you need jQuery*
+<script src="/javascripts/js/js-discourse.js"></script>
```

* Edit your posts.html to include the below somewhere. Feel free to re-write the noscript tag.

```diff
+  <section>
+    <h1>Comments</h1>
+    <div id="comments" style="padding-left:35px; padding-right:35px" {% discourse_comments %}></div>
+    <noscript>Javascript is off, so comments don't load.</noscript>
+  </section>
```

In octopress, you can remove:

```diff
-{% if site.disqus_short_name and page.comments == true %}
-  <section>
-    <h1>Comments</h1>
-    <div id="disqus_thread" aria-live="polite">{% include post/disqus_thread.html %}</div>
-  </section>
-{% endif %}
```
 and include the above code.

When the pages are created, this will add an attribute to the div tag called "tid", which holds the related topic ID. 

### Jekyll/Octopress Guide ###

Back out of your source directory, and create a directory called `_discourse`. Next, edit your `_config.yml` to include:

    discourse_api_key: Your api key
    discourse_api_username: Username to post as
    discourse_api_category: Category to post to
    discourse_api_url: The URL of your discourse install, include http:// and no trailing slash.

Remember that spacing in a yaml file is important. Each entry is a new line, and one space after the colon.


Next, edit `js-discourse.js` and view the configuration at the top. Remember that java-script is run client side, so setting the update time low will hammer your server from many locations.

To recap:

1. Add Access-Control headers to your nginx/apache config.
2. Install httparty and json. Edit your Gemfile if you use one, otherwise install directly.
3. Add the liquid tag to whatever layout you want comments for.
4. Add needed information to your configuration yaml file.
5. Add needed information to `js-discourse.js` 
6. Generate your site!

 [8]: http://eviltrout.com/2014/01/22/embedding-discourse.html
