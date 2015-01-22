### Barrel Development Best Practices

# Performance
 
### Minification/Concat JS/CSS
- Establish workflow for minimizing asset groups at the beginning of the project. (It's probably Grunt. Gulp?)
- If you're using an existing framework utilize it's asset pipeline. (Rails, Shopify)
- Always create source maps for minified assets.
- If using assistant tasks, like Autoprefixer, declare minimum browser version support.

### Handle Requests Responsibly
Minify scripts, but not to the point of squeezing requests into too large of chunks.

- Determine what CSS and JS is necessary on page load.
- Minimize the use of blocking CSS and JS libraries.
- Stagger and defer problematic or non-essential scripts.
- Thread large JS scripts by not minifying them with others.

For problematic libraries like Facebook and YouTube, use asynchronous loading and callbacks to determine when the scripts are loaded and then execute.

### Server-side Caching*
Caching is a really great thing. USE IT! Often platforms have caching built right in, and the process of activating it is simply flipping a switch. If not, plugins exist for nearly every platform to make it possible.

Server-side caching cannot always be a "full-page" cache since many sites have dynamic elements that change and need to change frequently (ecommerce, web-apps). This doesn't mean you can't use caching. Some options still exist:

#### Partial Caching
Partial or "fragment" caching uses key/value datastores like Redis or Memcache to store smaller parts of the page in the cache to quickly process smaller sections that have a longer refresh rate. These can usually be activated through framework extensions (see Rails, Magento, Wordpress, Shopify).

#### DB Caching
Database caching remembers common queries and their results so that they don't need to be called over and over. This is also referred to as Model Caching.

#### Smart Back-end Code
Minimize repetitious queries by storing common data in class variables that can be checked prior to a new database request.

#### Common Settings / Considerations
- Cache / datastore freshness and age
- Refresh triggers (database, foreign key)

### Client-side Caching
- Use "Web Page Test" ya dummy!

#### Version string
Append version string for cached assets so you know you'll be getting the latest.

- Make sure the versions string is based on when the asset was saved, not the time is was looked up.
- Defer browser caching until launch, as not to mess with QA and provide realistic first-view times.
- Set `max-age = "1" # Like, never` during development.*
- Set `max-age = "2592000" # 30 days` pre-launch.*
- See [this article](http://www.mobify.com/blog/beginners-guide-to-http-cache-headers/)

```
<script type="text/javascript" src="http://www.foobar.com/public/scripts/main.js?v=20150204121515" />
```

Version strings may be ineffective for particular CDNs. For example CloudFront provides the option of ignoring version strings when setting up the distribution. Be mindful of how your CDN will process the asset.

#### CDNs
A CDN (content delivery network) is a distributed network that will provide web assets to the client from the closest available location to limit network latency. You should totally set one of these up on all of your projects.

- When scoping projects, CDN should be part of the recommendation and budgeting.
- CDN should be set up early in a project's lifecycle and used for both staging and production environments.

For more information on setting up a CDN, please refer to [Dev How-Tos: CDN](https://github.com/barrel/barrel-dev-how-tos/blob/master/cdns.md).

#### Domain Sharding
Domain sharding is the process of separating assets to different domains so that more things can be downloaded simultaneously (6 threads per domain).

- Separate and serve sets of assets from different domains on your CDN: CSS/JavaScript, static images, user uploads, etc.
- More domains != more speed, typically content from the network should not be served from more than three domains (local, 2 x CDN)

#### Managing Slow Libraries
Some examples of really slow libraries: Facebook, Twitter, LinkedIn, social integrations in general, RT Pixel, Mixpanel, Hubspot, user lead tracking in general.

- Offload scripts for slow libraries so that they load either at the end of the page or asynchronously (if that option is available).
- Manage project expectations surrounding social integrations and other slow libraries.
- Use back-end integrations when possible (Intercom, Mixpanel, Hubspot, etc).
- When possible, don't block page render with lead tracking.


##TODO
---
###Scott
- Image minification/compression
    - Control upload size (ideal?)
	- Communicate and leverage the best way
 
###Eric
- Image sprites (load less)
 
###Ben
- Optimize backend code

###Wes
- Databse indexing
- CSS Hardware Acceleration (opacity warning)

###Zack
- Use of SSL should be limited
- Server Config
    - gzip
	- keep alive (connections)

###Kevin
- Identify when ajax can happen
- Queued Jobs
    - Email
	- Curl
