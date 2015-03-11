
index.html
Original PageSpeed score: 28/100

Research:
Minification build tools
- https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=minification%20build%20tools

I did a Google search for build tools to help with minification and it suggested a plug-in to analyze HTML,
	as well as tools for CSS and Javascript. I installed the PageSpeed Insights plug-in for Chrome.

General improvements
- http://www.smashingmagazine.com/2014/09/08/improving-smashing-magazine-performance-case-study/
This case study of the redesign of the Smashing Magazine website walked through their entire process, 
	from how their website was underperforming, their thoughts on how to fix it, the solutions they
	came up with and why they worked.

Browser caching
- https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching#defining-optimal-cache-control-policy
- https://www.mnot.net/cache_docs/#IMP-SERVER

I didn't understand how to properly set up browser caching. At first, I thought it was just another option
	that could be included within a tag. For instance, setting the 'Cache-Control: private, max-age=600' on an image
	so that it would be cached for ten minutes. But, apparently, the caching information needs to be included in the
	header information and implementing it is different based on the type of server; it can also be accomplished
	in different ways based on the scripting language used. I wasn't sure how to make it happen with my project
	files on github.io.

I worked on each improvement individually as per the suggestions given by the PageSpeed Insights plugin for Chrome:

- Inlined print.css.
- Inlined perfmatters.js.
- Moved the inlined CSS and Javascript before the call to fetch the external CSS resource.
- Scaled pizzeria.jpg.
- Created non-compressed PNGs of all images and specified their widths and heights in the HTML.
	Even after creating PNGs and selecting to have them interlaced (apparently, this makes a difference), the
	PageSpeed plugin was still telling me that I needed to optimize the images to achieve a higher PageSpeed. I'm
	not sure if this means there's another option to the PNGs that I need to set or if I should be using another
	image format entirely.
- Minified style.css and linked to the minified file in index.html.
	This wasn't a suggestion made by the PageSpeed plugin and it didn't affect the score.

Final PageSpeed score: 98/100

*** RE-SUBMIT ***

I had not been using ngrok to view my work; I was uploading my work-in-progress to GitHub Pages, thinking it would achieve the same effect. Apparently, it didn't. I had some trouble getting the python SimpleHTTPServer set up and getting ngrok to talk to it, but the ngrok documentation helped, and I figured it out after a little while.

First I needed to optimize the web fonts. I read up on [Google's suggestions](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/webfont-optimization?hl=en#optimizing-loading-and-rendering) for inlining the font styling content, and did so. Then, I minified index.html. I also noticed that the Google Analytics code is inlined in the <head> and that there is another call to a Google Analytics Javascript file further on down. I removed the link to the second file. After these optimizations, the PageSpeed score was up to 95/100, through ngrok.

Fingers crossed that this has done it!!!

pizza.html

Research:
I struggled mightily with this at first. I thought that both corrections to main.js (as outlined in the intro
	comments) pertained to the FPS of the scrolling pizzas. I figured out the first optimization, but then spent
	most of a day making further changes to updatePizzas() and not reducing my average time to generate the last
	10 frames. Finally, I consulted the Project 4 office hours, which pointed out that the second fix
	specifically pertained to the time to resize the pizzas after changing the slider.

- Original average time to generate last 10 frames: 30-40ms
	I looked at Ilya's demo (https://www.igvita.com/slides/2012/devtools-tips-and-tricks/jank-demo.html) and
	determined what (if anything) was being done differently there. What I noticed, was that calculating the
	scrollTop value as part of calculating the phase value was taken out of the loop, so as only to be done once.
	I moved this calculation out of the for loop and assigned it to the cachedScrollTop variable and used
	cachedScrollTop when calculating the phase value. Although my average time to generate the last 10 frames was
	still relatively high, this brought my timeline well within the 60fps mark.

	Final average time to generate last 10 frames: 1-1.5ms

- Original time to resize pizzas: 100+ms
	I used a somewhat similar technique for this puzzle. I moved the calculation to determine the windowwidth (line 425),
	and saved all the random pizzas to an array with one call (line 429) rather than get the "current pizza" with
	every iteration through the loop. Then, since every random pizza had the same properties (or, rather, the image for
	every pizza had the same dimensions), I just took the first pizza and used it as a template for the other 199 (line 462).
	I passed that value into determineDx() (line 434) instead of the image for the "current pizza."

	Final time to resize pizzas: 1.17ms
## Website Performance Optimization portfolio project

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

To get started, check out the repository, inspect the code,

### Getting started

####Part 1: Optimize PageSpeed Insights score for index.html

Some useful tips to help you get started:

1. Check out the repository
1. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. Download and install [ngrok](https://ngrok.com/) to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ngrok 8080
  ```

1. Copy the public URL ngrok gives you and try running it through PageSpeed Insights! [More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

Profile, optimize, measure... and then lather, rinse, and repeat. Good luck!

####Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js. 

You might find the FPS Counter/HUD Display useful in Chrome developer tools described here: [Chrome Dev Tools tips-and-tricks](https://developer.chrome.com/devtools/docs/tips-and-tricks).

### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>

### Sample Portfolios

Feeling uninspired by the portfolio? Here's a list of cool portfolios I found after a few minutes of Googling.

* <a href="http://www.reddit.com/r/webdev/comments/280qkr/would_anybody_like_to_post_their_portfolio_site/">A great discussion about portfolios on reddit</a>
* <a href="http://ianlunn.co.uk/">http://ianlunn.co.uk/</a>
* <a href="http://www.adhamdannaway.com/portfolio">http://www.adhamdannaway.com/portfolio</a>
* <a href="http://www.timboelaars.nl/">http://www.timboelaars.nl/</a>
* <a href="http://futoryan.prosite.com/">http://futoryan.prosite.com/</a>
* <a href="http://playonpixels.prosite.com/21591/projects">http://playonpixels.prosite.com/21591/projects</a>
* <a href="http://colintrenter.prosite.com/">http://colintrenter.prosite.com/</a>
* <a href="http://calebmorris.prosite.com/">http://calebmorris.prosite.com/</a>
* <a href="http://www.cullywright.com/">http://www.cullywright.com/</a>
* <a href="http://yourjustlucky.com/">http://yourjustlucky.com/</a>
* <a href="http://nicoledominguez.com/portfolio/">http://nicoledominguez.com/portfolio/</a>
* <a href="http://www.roxannecook.com/">http://www.roxannecook.com/</a>
* <a href="http://www.84colors.com/portfolio.html">http://www.84colors.com/portfolio.html</a>
