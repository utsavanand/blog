## Dependency Injection for Dummies.

One of my intentions of writing this is to remember and understand different ways of injecting dependencies within the Spring framework.

The other is to explain dependency injection in a manner that a lay-software-engineer[1] can understand. My goal is to explain the concept as organically as possible and then explain how it applies within the world of spring (or spring-based[2]) frameworks. 

Firstly, let's just list down the terms that are important to understand this concept. 
1. Container 
2. Dependencies 
3. Inversion of Control (Facetiously[3] referred to as the "Hollywood Principle: Don't call us, we'll call you"

Before I start explaining what is Inversion of Control and how does it actually relate to Dependency Injection, I wanted to call out that be it IoC (Inversion of Control) or Dependency Injection or Auto-wiring (this will come in later), all of these are fancy terms to explain very basic software design concepts. But still, the more clarity you get about these concepts and techniques and understand their application and actual cost benefits, the more your appreciate the absolutely apt naming.

### Inversion of Control?
In traditional programming which was procedural in nature, the control of the program was with the program itself. Inversion of control is when we delegate the control of the program to someone or something else which will then drive the flow. We shall try to understand this with an example but imagine a large concert where a lot of musicians are playing a complex composition, the orchestrator or the concert master is responsible to manage the control of the tune and not the musicians themselves.

Let's say you have designed a registration pipeline for your product where at each step you need to run some validation checks.

Procedural Programming

{% if jekyll.environment == "development" %}
![Procedural]({{ site.url }}/assets/images/IoC-1.png)
{% else %}
![Procedural]({{ site.url-prod }}/assets/images/IoC-1.png)
{% endif %}

Event driven programming  (Over here the control is now with the notifications and not with the program itself).
	
{% if jekyll.environment == "development" %}
![Event driven]({{ site.url }}/assets/images/IoC-2.png)
{% else %}
![Event driven]({{ site.url-prod }}/assets/images/IoC-2.png)
{% endif %}	

This is one of the examples where the control is taken away from the program itself. Some other examples include Factory Pattern, Observer Pattern, Dependency Injection, etc.

### Why did the term Dependency Injection came into being and what is it?

{% if jekyll.environment == "development" %}
![Martin Fowler]({{ site.url }}/assets/images/Martin-Fowler.jpg)
{% else %}
![Martin Fowler]({{ site.url-prod }}/assets/images/Martin-Fowler.jpg)
{% endif %}

This guy in the picture above, Mr. Martin Fowler was the first one to use it. This is the original article where it was used.

Let's start with what this concept is all about before we understand what does it mean from a software design perspective.

Let's say you tend to travel a lot for work. And whenever you travel you need an airline ticket, cabs for drop and pickup and hotel accommodations. The general process that you need to call up the airline agency, cab agency and hotel. So, you need to know the contact information and booking process for all of these. If these agencies for whichever reason change, you will have to re-learn the new contact information and process.

Now imagine, having a different protocol where over a telephone you answer some standard questions and another system takes care of making the bookings. Your job is to supply all the information over a telephone and your entire itinerary will be taken care of. In this scenario, if the agencies change you would not need to relearn anything. This is Dependency Injection in the real world scenario.

Image the savings this could and actually does have in a large organization. Here, you are the client and you are dependent on the services provided by these agencies.

Let's see the same thing via some code.

~~~~

	public class client { 
		Booking newBooking  = new BookingImpl_1("source", "destination","number_of_days");
	}  

	Client clientObj = new Client(); // Can’t change to BookingImpl_2

~~~~
	
~~~~

	public class client {
		Booking newBooking;
		public client (int impl_type, String source, String destination, int number_of_days) {
			if(impl_type==1) this.newBooking = new BookingImpl_1("source", "destination","number_of_days");
			this.newBooking = new BookingImpl_2("destination", "source", "number_of_days");
		}
	}
	
	Client clientObj = new Client(1, something, something, something);  // Uses BookingImpl_1
	Client clientObj = new Client(<anything other than 1>, something, something, something);  // Uses BookingImpl_2
	
~~~~

In the second case we have injected the dependency. So, imagine if the way bookings are made changes (i.e. you need to use BookingImpl_2 instead of BookingImpl_1), the client i.e. you will not have to worry. 

So, whenever you inject a dependency you have more control over it. As leaving the job of creating and using a dependency to the program itself leads to tight coupling.

More on different types of dependency injection mechanisms in part-2 here.

***

[1] What is a lay-software-engineer? Personally coined. Not proud of it. But it refers to any software enthusiast without much of a specialized computer science knowledge or experience but a very basic understanding of what software is and that you can't really touch it. 

[2] What are some other spring based frameworks? Struts I believe is one. And any other web framework that inherently uses spring but has other libraries and services built on top of it to ease and organize software development.

[3] Glad to learn a new word while writing this blog - Facetiously!
