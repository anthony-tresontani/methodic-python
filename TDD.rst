===========================
Apply TDD, the pythonic way
===========================

**We, has developers, suck**

Software development is complex and there is as many way of creating a good program than the way of creating a good bug.
From the most obvious to more complex:

1. Code syntax
   Write non valid python code.
   Way to avoid: run the python command will detect it

2. Typos
   Make a typo when calling a variable name, function name or class, etc.
   Way to avoid: run each function, class etc.| Ask your IDE 

3. Bad algorithmic
   In complex code with nested loops or conditionnals and when handling many side cases, you can sometime misplace you return function
   or a nested if.
   Way to avoid: Check the returned result for the function | pair programming

4. Write over complicated code
   Most of the time, when starting a new solution for a non-obvious problem, we over-think it and produce something
   absurdly complex for a problem complexity quiet low.
   Way to avoid: Pair programming, pair review

5. Write out-of-spec program
   It may also happen we write a good program solving the wrong problem.
   Way to avoid: Pair programming, pair review

6. Write non-robust program
   Cool, that works! add(2,4) = 6. Great. What about add(2, -3)? I haven't think about that. Booo! Weak program.
   Way to avoid: Unit tests

7. Write useless code
   You fought against you problem for long hours and tried many solutions. You finally found the working one but don't have anymore
   the energy to clean the mess done by the fight or doesn't really feel confident of what an be removed.
   Way to avoid: Do not fight | Take risk

8. Write bad design
   You finally did it and all your algorithm are clean and effective. Seems like this method can be extracted to his own class.
   And this class name may be more expressive with another name.
   Way to avoid: Trust blindly your IDE and click refactor (I hope that fine, I do hope that fine) | Unit test

9. Fix a bug on a piece of code written by a collegue and break everything else
   WTF is he trying to do there. Don't know. Let's tweak it!
   Way to avoid: Always have the original author with you (sounds unlikely) | Unit tests

Well, not that simple. Fearful programmer live full of traps and ambushes. "No way, I am better than that".
Some programmer may be really better than that and been able to avoid all of them due to an increadible level of focus, memory and attention.
But, we, normal professional programmers, are interrupted, receive emails and calls, being ask by your boss to explain him why? 

Else you can void all the ambushes by apply everytime all the "way to avoid". Seems unlikely.
Or, you use the common tool to prevent all of them: TDD.

TDD stands for Test Driven Development. The idea is quite basic, write a test before doing anything else, then run it, fix the code with the most obvious method and refactor the code.
How do I write feature X? No idea, but the first step is 

  def test_feature_X(self): 

Ok, final argument, that do work. Convinced!

**Why you should even more matter**

Something else than you may not be aware of is any other methodology, tools and so rely somewhere on TDD.
Refactoring without TDD is as hazardous as writing the second line in perl.  TDD ensure you refactoring keep the feature is the same working state.
Continous integration means running unit testing continously.
Domain driven design means Refactoring meaning TDD as design patterns do.

Even performance testing is really easy if you already have an existing set of test to check for performance issue.

**Why I Am using it**

As a lazy guy, I can rest during tests run. It also make your life predictable: soon, you will run a test. Still lazy, it ask me to do the minimum to make the test pass.
As a really bad programmer, it ensures I am not doing any mistakes.

**Release your TDD power**

There is, for sure, different way to apply TDD. And I will only explain mine. Some start by a unit test performing really tiny step, some other start with an acceptance test which will stay RED during a long time before been GREEN. It's mainly a psychological aspect which differt. If you can stay a long time with a RED test, you will be able to follow my way.

1. Write an acceptance test.

Ok. Write an acceptance test. We want feature X do something, but, but... :

0. Setup your env.
To run anything we need a runner, obvious. My choice is always nose. Nice, clean, efficient. Nose is just a unittest extension which make test painless.
Organise your tests the way you want, write setup method at any level and run your test.
``pip install nose``, then we also need unittest. READY TO GO.

Back to step 1.

Jojo: "Well, I'd like to implement feature X"
Gigi: "What's this feature about"
Jojo: "Given a name, it says how many X chromosome  the person X have"
Gigi: "Predict gender, and the test will be test_predict_gender"
Jojo:

    TestPredictGender(unittest.TestCase):
      def test_predict_gender(self):

Gigi: "Fine, just add pass"
Jojo: That will not fail, is that really TDD?
Gigi: "Author ??"

Not really. TDD request to write a failing test first, this one may pass. My experience as a Python developer lead me to always write this test stub first.
And it happened to failed often.
The reason is simple: 
  - Ensure the test is detected and run. Ex: Remove any test method name mislabelled.
  - Check any setup/configuration. When using any framework, make this test pass can already take lot's of time. For example, with django, this allow you to ensure your DB setup is right.

Gigi: "Go on"

    TestPredictGender(TestCase):
      def test_predict_gender(self):
        pass

Gigi: "Then run"
Jojo: 
  nosetests
  Ran 1 tests in 0.003s

I do suggest to increase the pairing team cohesion by promoting a happy feeling. To be only applied when pairing. Can be weird alone.

Gigi: "We did it, yeah!"
Jojo: "Let's write a failing test now"
  self.assertEquals( "XX", predict_gender("Jojo"))

Introducing PyHamcrest...
Tests should be readible and explicit (remember ambush 9), pyhamcrest improve that. Simple

   assert_that(predict_gender("Jojo"), is_(equal_to("XX"))

...end introducing PyHamcrest

Gigi: 
  nosetests
  FAILED ( error=1 )

Here we are, the TDD process is initiated. Just keep the pace. Development is a marathon.
Keep on iterating, again and again.

Sometimes, after a long day, you will feel like you can skip a test. My advice is never do that (that's an advice I do not apply once a week, and pay it expensily).
Even the more obvious piece of code can fail, and especially after a long day. If you also like leave on time, fight yourself and apply.

Ok, now we want the code.
On the TDD original bible, Kent Beck give us three methods to write the next step:

`Remove duplication`

TDD implies writing the smallest solution to make your test pass. Which mean, if you have:

    self.assertEquals( my_function(1), 10)

the first usual step is to write:

    def my_function(val):
        return 10

Well, fine, but now we have the value 10 duplicated between the test and the code. You have to remove it and write real code for that.
The aim of that is to refactor under a GREEN test suite. As soon as you break something by refactoring, RED.

`Triangularisation`

This is definitely my prefered method. You still perform as the first method, but instead of removing duplication, you write another test.

     self.assertEquals( my_function(2), 20)

Can't keep anymore your weak implementation, have to write real solution. For a pure TDD point of view, there is two cons:
1. Your work with a RED bar.
2. Code duplication is test.

My advice is to remove the second test when you get the final GREEN bar.

But the process is easier and scalable. The more complex your problem is, the more effective triangularisation is.

`obvious implementation`

Sometimes, it's just so easy than doing it in one step is just obvious. Take care using this method.

Ok, but what if I have a third party application and I should connect to a remote server to test my client code.
Mock it!

Fake the world
--------------

One of the strongest python ability is it ease at mocking stuff. Anything you need, just mock it.
If you need a new attribute username, add it:

   my_object = MyObject()
   my_object.username  = "Gigi"    # that's done

Need a new method, add it:

   my_object.get_username = lambda x : "Gigi"

Need a new simple type, add it:

   my_request = type("Request", (), ["user":user"])()

Nothing cannot be mocked.

If an already existing class need some modifications, change it. 
Sometimes you might not want to perform a call to a external provider

    MyClass.get_external_data = lambda x: "Jojo, Gigi"

A django example:

Template tags in Django allow to define custom logic applied on the template level to display some information.
How damn can we test such an deep django element? ......... Mock it!

By creating a new init method ( Yes override the constructor ), you can easily test it, see after:

  def new_init(self, value, user):
      self.value = value
      self.user = user
  
  def setUp(self):
      MyTemplateTagNode.__init__ = new_init
      value = "my value"
      my_user = User.objects.get(id=1)
      node = MyTemplateTagNode(my_value, my_user))


Ok, fake everything is easy in Python, but what about data? And database value ?

Data can be created in any setup level method: before the method, before the class, before the module. Which mean, you can refactor as much as you want your data provider.
For big project, where business rules to be applied are complex, I do advice you to create a data API. Want a user, `create_user`, want an admin user, `create_admin_user()`.
That will ease your test creation and make them readable, and also ensure your collegue don't forget to create the underlying B object.

Well but what about database data?

Most of the time, amongst tens of attribute, you only need one or two in your test. The more attributes you set, the more likely you will have to change your fixture if any table modification occured.
What's why you need an abstract data generator. I don't have a generic solution but for Django, there is an awesome one:

Introducing django-dynamic-fixtures...
It's straightforward.

    my_object = get(Object, field1="Jojo")

That's it. What if field9 is added to Object class (what a silly class name), still working. Field 7 and 12, still working.
.... end introduction

**I can't apply it cause..**

- I haven't got the time, project manager put pressure on me to respect deadline.
  That exactly the most effective time to apply TDD. The more pressure on your shoulder, the more obvious mistake your make.
  Ok, if you're just ask to deliver non working code, don't use it. If working code is requested, TDD.
  There is absolutely no trade-off to apply TDD, it's just faster as soon as your problem is more than trivial (I mean more than 3 lines).


**Gotchas**

*TDD is not test*

TDD allow you to write well designed and easy to test code, but TDD is not really about testing. Tests are a side effect to TDD.
The method focus on unit test. It's not because who have a really well working set of block than the house stands.
They have to be well assembled. System, acceptance, smoke or whatever should still be considered.

*Local maximum != global maximum*

TDD is an iterative process to improve your code until the local maximum. However sometimes, after a small slope, there is a big mountain.
You still have to think of your design and try to take bigger step if you forecast a mountain.

*Be better, not the best*

There is no TDD developer supremacy, you will not become the top-level developer of your company after 1 hour of TDD.
But you will definetely become better.


**TIPS**

- Do not start by a test to handle errors. Think positive first and negative after.
- TDD is not unit tests. TDD means writing a test first.
- Start every new feature by an acceptance test first. Before chosing the road, chose the direction.


**When to not apply**

Sometimes, it's really better not to apply. If:
- you are using thread programming, that can be really tough to use.
- Developing a one-shot for one hour with a peer. I faced the case during a python Dojo. Four people on a one hour problem are more effective than TDD.