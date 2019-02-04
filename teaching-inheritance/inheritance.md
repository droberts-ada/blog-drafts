# Teaching Inheritance

> Not one of them<br>
> is like another.<br>
> Don't ask us why.<br>
> Go ask your mother.<br>
> -- <cite>Dr. Seuss</cite>

Think back to when you were learning your first object-oriented programming language. If you learned to program in the last 15 years, there's a good chance that was your first language, period. You probably started off with simple concepts like conditionals and loops, moved on to methods, and then to classes. And then at some point your instructor (or mentor or tutorial video) introduced a concept called _inheritance_.

If you're like me, your first exposure to inheritance was a mess. Maybe you  

I have never seen inheritance taught well. This includes the several times it's been taught to me, as well as the three different programs in which I've taught it.

In this blog post I'll dive into why teaching inheritance is hard, why current methods don't work, and what Ada Developers Academy is doing to try and address this problem.

## Why Teaching Inheritance is Hard

Teaching inheritance is hard for two reasons:
1. The syntax and semantics of inheritance are tricky
1. Problems that are well-suited to inheritance are complex

Let's dive deeper into each of these

### Inheritance is Tricky

At a high level inheritance is easy to explain: one class gets all the code from another class, and can override pieces and add its own bits.

As so often happens, the devil is in the details. For example, with Ruby the following questions arise:
- How are static method handled?
- How are instance variables, class variables, and class-instance variables handled?
- Can constants be overridden? If not, what should you do instead?
- How does inheritance interact with nested classes?
- What has precedence, methods from the parent class or from a mixin?

And that's for Ruby, which is supposed to be beginner friendly! Other languages have their own wildcards:
- Python: multiple inheritance
- JavaScript: prototypical inheritance model, multiple types of functions
- C++: static typing, interfaces, templating, object slicing

Whatever language you choose, there's going to be a lot of rules to remember. How do you encode all these, especially for a novice? How do you decide what to include up-front, what to put in the appendix, what to omit entirely? This is an important part of the problem - all your real-world knowledge of how inheritance is used and what kinds of problems it solves won't do you any good if you can't actually apply it in code.

Fortunately, this part of the problem of teaching inheritance is well-understood. There are many excellent texts that round up the complicated syntax and semantics of inheritance into digestible, memorizable chunks. Any alternative treatment of inheritance needs to acknowledge this challenge and build upon this existing work.

### Problems that Need Inheritance are Complex

- Inheritance is used to share code between classes. If you're writing code that benefits from inheritance, that means there are several classes that are 

## Thinking About Inheritance

The first question to ask is, "do we really need to teach inheritance?" This might seem like a silly question - everyone teaches inheritance, it's a key part of object-oriented programming! However our curriculum is jam-packed into our 6-month course, and everything we _do_ teach means there's something else we _don't_ teach. We've found more often than you might expect that we can drop something that "everyone" does, and end up with a leaner curriculum that is more valuable to both our students and their employers.

But as it turns out, we do have to teach inheritance. Both Rails and React use inheritance at a fundamental level, and our curriculum wouldn't make sense without it. Moreover, inheritance is an important technique for building real-world software, and our graduates use it on a regular basis in the wild.

### How is Inheritance Used?

The answer to the previous question leads us to two others: "How is inheritance used in the real world? How is inheritance most likely to be used by junior engineers in their first few years on the job?" Understanding how inheritance is used can give us some direction on how it should be taught. Let's look at a few examples.

#### Rails

In Rails, almost every class you write will inherit from something. The two most common are
- `ActiveRecord::Base` - superclass for models
- `ActionController::Base` - superclass for controllers

You also see inheritance used for everything from database migrations to configuration management - its the Rails Way&trade;. If you want to do something, you inherit from a class somewhere in the Rails framework. These superclasses are generally quite abstract, and each covers some functionality specific to the domain of an MVC framework.

Another important idiom is the [template method pattern](https://en.wikipedia.org/wiki/Template_method_pattern), as made famous in the Gang of Four book. A great example of this is with [database migrations](https://edgeguides.rubyonrails.org/active_record_migrations.html), where you define an class that inherits from `ActiveRecord::Migration` and implement the `change` method, and Rails takes care of the rest. Controller actions also mimic the template method pattern, particularly if your application uses the builtin tools for RESTful routing.

For the most part, Rails does not have you define your own superclasses. The exception to this is `ApplicationRecord` and `ApplicationController`, which sit in the hierarchy between concrete models or controllers and the abstract Rails implementation - these are generated automatically by Rails, but are open for you to modify.

#### React

React isn't quite as broad in its use of inheritance as Rails. However, [every component class inherits from `React.Component`](https://reactjs.org/docs/components-and-props.html).

In React we again see the template method pattern pop up. Whether you're implementing `render` or `componentDidMount`, React knows the algorithm and you fill in the details.

React also does not encourage defining your own superclasses. In fact, their official documentation is rather explicit that [inheritance between components should be avoided](https://reactjs.org/docs/composition-vs-inheritance.html).

#### Other Frameworks

Rails and React are the two industry-grade frameworks I'm most familiar with, but I've dabbled in some others, namely Android (Java) and Unity (C#).

Android follows a similar pattern: everything you write inherits from some builtin class, template methods abound, and developers are discouraged from building their own inheritance relationships.

Unity matches the pattern as well, but they seem to be more lenient about extending your own classes, at least as far as I can tell from [the Unity documentation on inheritance](https://unity3d.com/learn/tutorials/topics/scripting/inheritance).

#### Summary

There are a few clear takeaways from this quick survey:

- Inheritance solves a complex problem. Programs that benefit from inheritance tend to be fairly large
- Writing a subclass is much more common than writing a superclass
- Often the superclass is provided for you by whatever framework you're using
- Superclasses tend to be abstract, both semantically (embodying a high-level concept) and functionally (never instantiated)
- The template method pattern is extremely important

This matches my intuition about how engineering is done. Design work, in this case identifying the abstraction and building the superclass, is done by the team as a whole or by someone with an impressive sounding job title like "lead consulting systems architect". Implementing the details in a subclass is the job of an individual engineer.

## What Ada is Doing

OK, we've got some knowledge about inheritance is used. How do we use this to structure curriculum?

- Students' first exposure to inheritance should be through extending an existing superclass. This matches the way inheritance is used in the real world, and makes the benefits (not having to re-write a bunch of code) immediately clear.
- It's OK to use toy hierarchies (e.g. animal/dog/cat) to introduce the syntax, but the lesson should move on to 

## Existing Work

I claim that I have never seen inheritance taught well. To back up this claim, let's pull a few books off my shelf and see how they address inheritance.

### POODR

We'll start with one of my favorites: _Practical Object-Oriented Design: An Agile Primer Using Ruby_ (POODR) by Sandi Metz. In chapter 6 of POODR, Metz targets experienced engineers trying to up their game. She assumes familiarity with the syntax and semantics of inheritance, focusing instead on when inheritance is appropriate, useful design idioms, and common pitfalls to avoid. It's a great treatment of the topic, but its role is not as an introduction.

Metz uses the example of inventory management at a bicycle touring company. `Bicycle` is the superclass, and `RoadBike` and `MountainBike` are subclasses. It's a toy problem that meets Metz's goals: it effectively shows syntax and idioms, but is not something you would be likely to use inheritance for in the real world.

### AP Computer Science A

The test prep book for AP Computer Science A, published by Baron's, uses `Student`, `UnderGrad` and `GradStudent`, a similarly trivial example. If you want to know the syntax for inheritance in Java they've got you covered, but there's almost no mention of what inheritance is used for in the real world.

### Building Java Programs

_Building Java Programs_ by Stuart Reges and Marty Stepp, used for the introductory Computer Science courses at the University of Washington.

### Unity Tutorials

The Unity game engine has a [short tutorial on inheritance](https://unity3d.com/learn/tutorials/topics/scripting/inheritance) in their tutorials. This one is interesting to me because they 




## What Ada is Doing



### Real World Inheritance