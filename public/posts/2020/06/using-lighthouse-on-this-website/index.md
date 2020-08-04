# Using Lighthouse on this Website


## The beginning
A couple of weeks ago, I decided to start this personal website. The main issue I have with starting projects/ideas is that I am obsessed with performance. I always find myself at a place where I want something efficient, cheap (as it usually is just a hobby used for learning), and that follows best practices. As you are probably thinking right now, it is not possible to have everything :joy:. 

After evaluating several options, I decided to go with a static site generation approach. The fact of having a DB and having to pay/maintain for another component was just going to take more than 5 minutes, and honestly, I did not know if it was worth it.

Going with a static site generation approach also had some terrific advantages that came almost out of the box:
- Hosting it on Netlify, I could take advantage of the CDN, translating in better latencies, all for free.
- Pushing content will auto-deploy master and promote the assets to the CDN in a transparent, hassle-free way.
- It is static content. You don't have to worry about DB data bottlenecks at all (at least for this site).
- It is a Server-side prerender, meaning, no page creation at runtime (SSR), and no hacks for SEO (client-side SPA).

> **Netlify** is a company that offers hosting and serverless backend services for web applications and static websites. Netlify is currently the company I chose to host this site. Still, the information in this post can be applied to any other company that hosts static websites such as **GitHub Pages**, **Vercel**, **Amazon S3**, Google Drive, etc...

The thing that made me think of adopting this approach was the performance (did I comment I am obsessed with performance? :joy:). I ran [Lighthouse](https://developers.google.com/web/tools/lighthouse) with a couple of systems I built (more on this on a later post), and the numbers were just too good to consider any other option.

## What is Lighthouse
From its [website](https://developers.google.com/web/tools/lighthouse):
> Lighthouse is an open-source, automated tool for improving the quality of web pages. You can run it against any web page, public, or requiring authentication. It has audits for performance, accessibility, progressive web apps, and more.

Having a tool like this is excellent for someone like me that is not a Frontend Performance expert and typically doesn't do these tests regularly. Paint refresh, CPU and Network throttling, Largest Contentful Paint... are metrics that I am aware of, but I would struggle to measure them on my own with a self-made script (you can always use the Performance tab on Chrome if you are curious.)

## Running Lighthouse on this website

At the time I am writing this post, I decided to rerun Lighthouse and see what improvements can be done (as part of learning through the process). I am not going to lie; my threshold is having a minimum score (over 100) of 80, with an ideal rating of 95 up: I want the site to be blazing fast.
Of course, this is not needed for my site, and it is just the overengineering aspect I like to do to learn something extra during the process :wink:.

### Lighthouse on Chrome
To run Lighthouse, you can use Chrome DevTools. If you have a relatively up to date Chrome browser, you should see a tab named `Lighthouse`.
These are the current reports for the site at the time of writing this post.

#### Desktop
![Lighthouse Results for Desktop](posts/using-lighthouse-on-this-website/lighthouse-desktop-2020-06-06.png "Lighthouse Results for Desktop")

#### Mobile
![Lighthouse Results for Mobile](posts/using-lighthouse-on-this-website/lighthouse-mobile-2020-06-06.png "Lighthouse Results for Mobile")

### Lighthouse on the web
The PageSpeed Insights team at Google made [a web tool](https://developers.google.com/speed/pagespeed/insights/) to analyze the content of a website and propose suggestions to improve the site. This [tool](https://developers.google.com/speed/pagespeed/insights/) uses Lighthouse underneath at the moment of writing this post. For this alternative, you don't need the Chrome Dev Tools, and you might get more up to date stats (just because Google controls the UI as opposed to your tab on Dev tools). Also, PageSpeed Insights can tailor your stats with the [Chrome User Experience Report](https://developers.google.com/web/tools/chrome-user-experience-report). 
Here is a preview of the site and the [link](https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Fwww.josedavidbaena.com%2F&tab=desktop) in case you want to go deep by yourself:
![PageSpeed Insights](posts/using-lighthouse-on-this-website/pagespeed-insights-desktop.png "PageSpeed Insights: Desktop")

### Improvements
Based on this iteration of the website, I still can see some improvements that can be made. One of the easy fixes is image compression on the avatar, for example:
![Avatar size-reduction improvement](posts/using-lighthouse-on-this-website/avatar-size-reduction-improvement.png "Avatar size-reduction improvement")

While other improvements can be made, such as making smaller the scripts (around 0.6s drop over the render metrics), for the moment, I will leave them as they are, as it would involve additional plugin configuration and time invested on it.

One cool thing I saw that would make you think twice about the size of your images on your front page is the timeline for painting/repainting the screen that the tool provides:
![Timeline rendering](posts/using-lighthouse-on-this-website/rendering-timeline.png "Timeline rendering")
The bigger/heavy the image to load is the latest you will have a full content render. It seems obvious, but it is good to see it frame by frame.

### Learning with Lighthouse
Every time I run Lighthouse, I learn something new. So if you have not run it ever, I encourage you to run it with your website, your company's website, you name it!

I bet you can use the report to learn with a potential fix/contribution to the site. If you are part of a big company, even better! It might be your time to shine :wink:.

If you don't know where to start, I would recommend starting having a look at the rendering timeline. From there, I would recommend creating a hypothesis of what is happening on each frame and then correlate it with the information on the Performance tab. 
For example, for the current rendering in time:
![Annotated rendering timeline](posts/using-lighthouse-on-this-website/annotated-rendering-timeline.png "Annotated rendering timeline")
I would infer the following:
- **Before step 1**: HTML text is being downloaded from the Lighthouse server app.
- **Step 1**: HTML text first render happens. At this time, not all scripts have been loaded yet.
- **Step 2**: image avatar gets loaded. Hint: If we have a small enough image, we might make it load on the Step1 Frame.
- **Step 3**: you can see some CSS being applied and elements repositioning.
- **Step 4,5,6**: some minimal HTML elements repositioning. `Typeit` script is doing its thing.

This hypothesis is always great to validate with the Performance tab on the Chrome Developer tools:
![Chrome Dev tools: Performance tab](posts/using-lighthouse-on-this-website/chrome-dev-tools-performance.png "Chrome Dev tools: Performance tab")
Looking into the timings section, you can correlate the information from Lighthouse to the millisecond. Also, it is always good to know where the time on your first paint is going. In my case, Scripting and Rendering are the guilty ones.


## Ideas and future investigation
I made this for fun, although I can see a lot of ideas popping in my head related to this. The one that I have been thinking while writing this post is to have a CI job running Lighthouse and posting the performance stats into a folder/service. It would be great if your CI can determine whether to deploy a site or not based on your perf stats. Something like that can be done quickly following the next steps:
- Have a deploy preview URL (with Netlify there is an option to have a deploy preview for free)
- Push the code to GitHub. Github action with CI job quickstarts asking for when the preview site is up to date.
- Netlify hook starts deploying the code into the deploy preview site.
- CI runs Lighthouse, checks with the current stats, and decides if the deploy preview site is ok to use.
- CI tells Netlify (or any other provider) to continue forward and make the preview site the default URL for the domain.

Going a bit further, I can see Lighthouse used as a tool for reporting for sites (especially static ones). 

{{< admonition type=tip title="Idea" open=true >}}
It would be great to have a website that lets you store stats associated with your site and expose an API for performance diffs, etc... I believe this is a quick and easy solution that devs around the world could benefit, so if you are interested in something like that, **please send me an email!** If enough people are interested, I will put some time on it :muscle:.
{{< /admonition >}}

