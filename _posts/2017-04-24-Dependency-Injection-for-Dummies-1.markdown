One of my intentions of writing this is to remember and understand different ways of injecting dependencies within the Spring framework.

The other is to explain dependency injection in a manner that a beginner-software-engineer can understand. My goal is to explain the concept starting from its fundamentals and then explain how it applies within the world of spring (or spring-based[1]) frameworks. 

Firstly, let's just list down the terms that are important to understand this concept. 
1. Container 
2. Dependencies 
3. Inversion of Control (Facetiously referred to as the "Hollywood Principle: Don't call us, we'll call you"

Before I start explaining what is Inversion of Control and how does it actually relate to Dependency Injection, I wanted to call out that be it IoC (Inversion of Control) or Dependency Injection or Auto-wiring (this will come in later), all of these are fancy terms to explain very basic software design concepts. But still, the more clarity you get about these concepts and techniques and understand their application and actual cost benefits, the more your appreciate the absolutely apt naming.

#### Inversion of Control?
In traditional programming which was procedural in nature, the control of the program was with the program itself. Inversion of control is when we delegate the control of the program to someone or something else which will then drive the flow. We shall try to understand this with an example but imagine a large concert where a lot of musicians are playing a complex composition, the orchestrator or the concert master is responsible to manage the control of the tune and not the musicians. So, over here the control of the music has been delegated to orchestrator. 

Now onto a little more serious example. Let's say you have designed a registration pipeline for your product where at each step you need to run some validation checks.

Procedural Programming

{% if jekyll.environment == "development" %}
![Procedural]({{ site.url }}/assets/images/IoC-1.png)
{% else %}
![Procedural]({{ site.url-prod }}/assets/images/IoC-1.png)
{% endif %}

Event driven programming  (Over here the control is now with the data submission event and not with the program itself).
	
{% if jekyll.environment == "development" %}
![Event driven]({{ site.url }}/assets/images/IoC-2.png)
{% else %}
![Event driven]({{ site.url-prod }}/assets/images/IoC-2.png)
{% endif %}	

If you see the program flow it’s not sequential, its event based. So now the control is inverted. So rather than the internal program controlling the flow, events drive the program flow. Event flow approach is more flexible as their no direct invocation which leads to more flexibility.

Now it doesn't mean that IoC is implemented by events only. Some other ways of doing it could be Factory Pattern, Observer Pattern, Dependency Injection, etc.

Yes, that's right. IOC (Inversion of control) is a general parent term while DI (Dependency injection) is a subset of IOC. IOC is a concept where the flow of application is inverted.

#### Why did the term Dependency Injection came into being and what is it?

{% if jekyll.environment == "development" %}
![Martin Fowler]({{ site.url }}/assets/images/Martin-Fowler.jpg)
{% else %}
![Martin Fowler]({{ site.url-prod }}/assets/images/Martin-Fowler.jpg)
{% endif %}

This guy in the picture above, Mr. Martin Fowler was the first one to use it. This is the [original article](https://martinfowler.com/articles/injection.html) where it was used.

In simple words, Dependency Injection means that an external entity takes care of supplying the dependencies to an object instead of the object configuring its dependencies on its own. Now, this wouldn't make any sense unless we look at it with an example and some code.

Say you travel a lot to difference places for work and use a particular Travel Agency to make your bookings which could include your flights, rental cars, hotel reservations, etc.

~~~~

public class TravelBookings {
	private TravelAgency travelAgency = new TravelAgencyImpl("airlinePreference", "roomSize", "paymentType");

	// Method that accesses the above object
	public Booking makeBooking(String firstName, String lastName, ..) {
		..
	}
}

~~~~

In the code above, the class TravelBookings needs a TravelAgency instance in order to make a booking.  Notice how the TravelBookings class instantiates a TravelAgencyImpl instance. The fact that the TravelBookings class needs a TravelAgency implementation means that it DEPENDS on it. TravelBookings cannot make a booking without TravelAgency and therfore it has a dependency on the TravelAgency interface and on some of its implementation.

TravelBookings class itself instantiates a TravelAgencyImpl as its TravelAgency implementation. Therefore, it is satisfying its own dependencies. When a class satisfies its own dependencies it automatically also depends on the classes it satisfies the dependencies with. In this case TravelBookings will now also depend on TravelAgencyImpl, and on the three hardcoded string values passed as parameter to the TravelAgencyImpl constructor. You cannot use a different value for the three strings, nor use a different implementation of the TravelAgency interface, without changing the code. So, let's say that your company switches to a new Travel Agency, you will have to change your code. Which makes it clear that when a class satisfies its own dependencies it becomes inflexible with regards to these dependencies. Bad, right! Imagine you have different classes using TravelAgency and its implementation, you would need to make changes to all those classes whenever your company switches to a new travel agency.

What else? Give it a thought! 

Hint! How do you test this? 

You wouldn't be able to mock the implementation of TravelAgency. So, you wouldn't be able to unit test this class. 

Let's try something else

~~~~

public class TravelBookings {
	private TravelAgency travelAgency = null;

	public TravelBookings (String airlinePreference, String roomSize, PaymentType paymentType) {
		this.travelAgency = new TravelAgencyImpl(airlinePreference, roomSize, paymentType);
	}

	public Booking makeBooking(String firstName, String lastName, ..) {
		..
	}
}

~~~~

The TravelAgency implementation has been moved into a constructor. TravelBookings still needs the three parameters to use TravelAgencyImpl but it no longer satisfies these dependencies on its own. They are provided by whichever class would create the instance of TravelBookings. The three values are injected into the TravelBookings constructor. It is now possible to change the airlinePreference, roomSize and paymentType without actually having to change the TravelBookings class.

Remember, this is not restricted to constructors. You can also inject dependencies using setter methods, and directly into public fields.

Good until now. But if you see TravelBookings still depends on TravelAgency interface and TravelAgencyImpl. So let's try to do something about that.

~~~~

public class TravelBookings {
	private TravelAgency travelAgency = null;

	public TravelBookings (TravelAgency travelAgency) {
		this.travelAgency = travelAgency;
	}

}
~~~~

Take a min, and think about what we did there. 

And yes, now TravelBookings doesn't depend on TravelAgencyImpl. You can now inject any dependency as long as it implements TravelAgency interface. So, you can inject TravelAgencyImplABC or TravelAgencyImplXYZ. 

So, that was our example. I hope it helped. But one other thing you can try is, revisit this example keeping the two SOLID principles [(1) Open Closed](https://en.wikipedia.org/wiki/Open/closed_principle) and [(2) Single Responsibility](https://en.wikipedia.org/wiki/Single_responsibility_principle) in mind. You will how the DI design approach complements the two principles.

More on different types of dependency injection mechanisms in part-2 here.

***
[1] What are some other spring based frameworks? Struts I believe is one. And any other web framework that inherently uses spring but has other libraries and services built on top of it to ease and organize software development.
