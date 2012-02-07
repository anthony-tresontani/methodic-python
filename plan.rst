Python, with methods
====================

Apply TDD, the pythonic way
---------------------------

**Why you should matter**

TDD stands for Test Driven Development. The idea is quite basic, write a test before doing anything else.
How do I write feature X? No idea, but the first step is test_feature_X. 
All the benefits has already been fully describe on all books explaining deeply the method. From "you write clean code and best design" to "you'll be healthier". But all these great rewards to use this great approache rely on a static typed language. And mostly Java. Well, as a dynamic typed developer, you can expect even more. Expect it, then it's more.

As a python developer, you know how dense this language can be, in features by line of code or in bugs. No compiler to remove basic issues, not even mistyping variable name. What about object not implementing the protocol? Which protocol? Not even a way to specify which protocol you expect.

Then you will win:
  - All the basic TDD benefits.(already a long list)
  - Dynamic typing language errors

The other side is Python is a not compiled language, which means the cycle "write a test/ build / Run" is "write / run". Other win, it will even take less overhead than a strong typed language.

Now, you agree.
Ok, final argument, that do work. Convinced!

**Why you should even more matter**

Something else than you may not be aware of is any other methodology, tools and so rely somewhere on TDD.
Refactoring without TDD is as hazardous as writing the second line in perl.  TDD ensure you refactoring keep the feature is the same working state.
Continous integration means running unit testing continously.
Domain driven design means Refactoring meaning TDD as design patterns do.

Even performance testing is really easy if you already have an existing set of test to check for performance issue.

**Why I Am using it**

As a lazy guy, I can rest during tests run. It also make your life predictable, soon, you will run a test. Still lazy, it ask me to do the minimum to make the test pass.
As a really bad keyboard user, it ensures I am not doing any mistakes.

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
  - Ensure the test is detected and run. Remove any test method name mislabelled.
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
Test should be readible and explicit, pyhamcrest improve that. Simple

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
`Triangularisation`
`obvious implementation`


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

    my_object = new(Object, field1="Jojo")
    my_object.save()

That's it. What if field9 is added to Object class (what a silly class name), still working. Field 7 and 12, still working.
.... end introduction

