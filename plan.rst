Python, with methods
====================

Apply TDD, the pythonic way
---------------------------

*Why you should matter*

TDD stands for Test Driven Development. The idea is quite basic, write a test before doing anything else.
How do I write feature X? No idea, but the first step is test_feature_X. 
All the benefits has already been fully describe on all books explaining deeply the method. From "you write clean code and best design" to "you'll be healthier". But all these great rewards to use this great approache rely on a static typed language. And mostly Java. Well, as a dynamic typed developer, you can expect even more. Expect it, then it's more.

As a python developer, you know how dense this language can be, in features by line of code or in bugs. No compiler to remove basic issues, not even mistyping variable name. What about object not implementing the protocol? Which protocol? No even a way to specify why expect a protocol.

Then you will win:
  - All the basic TDD benefits.(already a long list)
  - Dynamic typing language errors

The other side is Python is a not compiled language, which means the cycle "write a test/ build / Run" is "write / run". Other win, it will even take less overhead than a strong typed language.

Now, you agree.
Ok, final argument, that do work. Convinced!

*Why you should even more matter*

Something else than you may not be aware of is any other methodology, tools and so rely somewhere on TDD.
Refactoring without TDD is as hazardous as ##########.  TDD ensure you refactoring keep the feature is the same working state.
Continous integration means running unit testing continously.
Domain driven design means Refactoring meaning TDD as design patterns.

Even performance testing is really easy if you have an already existing set a test to check for performance issue.



