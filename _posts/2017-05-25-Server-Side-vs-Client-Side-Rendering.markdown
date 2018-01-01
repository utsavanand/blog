## Server Side vs Client side rendering.

I have recently been trying to come up with some initial pointers that could help me with optimization of registration based website at work.

That is when I started reading about the differneces between Server side and client side rendering.

They are basically very simple to understand but it took me a while to get what they really mean and the various ways we could take to go about them.

Also, I want to add that I am not a frontend expert. I haven't done any benchmarking exercies to have a good enough answer on which of these two is faster. I have done some experiments to understand the pros and cons of either approach which were quite personalized to my use-case. So, this blog is mostly to document what I have leart and in the proces explain and contract these two options as simply as possible. So, here it goes.

When I first moved to the city of Bangalore for my first job out of college, I did not have a lot of experience of living alone. And in addition to other things, one major issue that I had to tackle was "How to get the groceries?" Why, you ask! Well, mostly because I over-analyze and over-think but also because I am a hugely lethargic organism which tends to move only in the most dire situations where survival is at stake.

So, I had two options.

1. Go to the store every other day, or every week and buy the groceries I need for that time period only.
2. Go to the store once every two month, and stack everything in a refrigerator. 

Let's pro/con these options now.

#### Option 1 - Make frequent trips to the store and get only you want for a limited time period.

##### Pros
1. Each trip to the store wouldn't take a lot of time as I am only buying of a particular time period.
2. I probably won't need a refrigerator to store the stuff I buy.

##### Cons
1. I will have to make multiple trips back to the store in a month.

#### Option 2 - Make one trip to the store and get most of your stuff. Stack it in a refrigerator.

##### Pros
1. One trip per month. I will still need to get stuff from my refrigerator when I need to cook but that obviously wouldn't take a lot of time.

##### Cons
1. I need to have a good refrigerator to stock all the groceries and keep them fresh.
2. Each trip will take a lot of time as I have more stuff to buy.

There could be many more things you could factor in while comparing the above two options but you het the gist.

Now, Option 1 - Make frequent trips to the store and get only you want for a limited time period is analogous to Server Side Rendering.

Option 2 - Make one trip to the store and get most of your stuff. Stack it in a refrigerator is analogous to Client Side Rendering.

You use *Server Side Rendering*, when you build your HTML pages on the server and then send them across to your browser. With every new request, you build the HTML from scratch and then send it back. So, on each browser request, server sends only the CSS, JS and HTML which is required on that particular page.

You use *Client Side Rendering*, when most of your DOM is populated on the client side i.e. on the browser. You get everything that you want (all of the JavaScript) during your initial load that will hold the logic of parsing your backend data (think JSON) and populating your DOM. And with each new request or event, you only fetch the data from server in formats like JSON and then update your DOM with it.  So, your browser in a way acts as a client to your data-source APIs.

Most of us already and automatically start with Server Side Rendering because that has for a long time been the traditional way. But now with a sea of these new isomorphic JavaScript frameworks and the MEAN stacks, client side rendering is quite a norm. 

Both of these options have their pros and cons, some of which you might have already deduced. Not rocket science I'd say.

But one thing to note with these, is how a *search engine would crawl your page* in the two cases. Some of you, who might have had exposure on how SEO and web crawlers work would have guesses that crawling client-side rendered pages could be an issue, and so it is. It's pretty straightforward with server-side rendering because your HTML already has your content when it's sent to the browser. With client-side it's a barebones HTML document, which gets populated after the JS makes an AJAX call and gets the data. So, a web crawler might not really prioritize your page because it doesn't see any content there. Little does it know about the wonderful things it would miss after your pre-loaded JavaScript furnishes its magic wand.

Now, there are things you can do to help it. Google _Prerender + SEO_! Also, a lot of search engines have made their crawlers smarter.

They can now see the future! 

Well, sort of. As long as, you allow them to crawl and parse your JS, some of the smarter web crawlers can even see content that gets created dynamically. But not all crawler can do it. So yeah, it is a bit of a risk to just bank on that. 

To sum it up, SEO will need a bit of work in case you want to go for client side rendering of your pages but it will be a lot faster and snappy after that initial page load. But even still, unless you really feel the need to try otherwise, it might just make sense to KISS and stick to the good ol' server-side rendering.



