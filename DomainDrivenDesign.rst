====================
Domain Driven Design
====================

Yet another driver.
Domain Driven Design is another adjacent approach to TDD, which implies understanding and solving problems from medium to big project.

Writing software is complex, but should not be complicated. However, we all know that the bigger our project is, the more inertia there is to CRUD ( Create, Read, Update or Delete any part of the project). Domain driven design is a part of the solution.

It implies many thinks:

  - We developers, are all responsible for the design of our application. Design on paper does not work.

  - Design is alive and unpredictable; should be applied with agility and modified during the whole project lifecycle.

  - We need to speak the same language as our customer, even in your design. If the customer call a product a SKU, use SKU.
    It will simplify communication and give more expressiveness to your model.

  - Reveal your intention. For any part on your program, remember developers willspend more time reading than writing. Don't be lazy with your method, variable or class names. Same applies for your tests. If you need to use 50 characters in a method name, do it.

  - Understand your core functionality and push out things that do not belong in it. Any project consists of a bunch of different layers: UI, database mapper (ORM), interfaces with external solutions. Learn to distinguish the core functionality and extranious elements and, and, and, push those out.

This is just a small part of the overall domain design and I will only explain the parts that brought a real value to my python projects.
These only include patterns that can be applied to your code, but you should know than some concept such as `bounded context`, `context map` and others exist. All of them go beyond software implemenation and aid in communication amongst teams, project management and other stake holders.

Values / Entity
---------------

Values/Entity is a concept outlined by many software development best selling authors. Domain Driven literature highlights this concept as well.

If you have to call something by a unique identifier, that's an entity. An object with its own identity, different from its peers is an entity, even if they have sames attributes and values. Sorry to say that, you are an entity. Being 1,82 meter tall and weighting 80 kilos does not make you identical to your cousin Jojo with the same attributes.

When you manage an entity, find a unique identifier in the whole lifecycle of the object. Card processor company uses an ARN, which is even valid outside your implementation's domain.

Everything which is not an entity should be a value. Do you have the first or the second of the two piece of cake, given they are both the same size? No. I JUST WANT MY CAKE!. Here, if the size attribute is identical, then the Value object is identical.

Let's code a piece of cake!

    class PieceOfCake(object):
        def __init__(self, size):
            self.size = size

        def __eq__(self, obj):
            return self.size == obj.size


Then, want we have a Value object, we should always return a new Value object, neither change it.
Example:

        def split(self):
           # Split the cake in two part 
           return (PieceOfCake(self.size / 2), PieceOfCake(self.size / 2))

This is said to be side-effect free. If for any reason, your modified the value during the object lifecycle, who can forget than the object has been modified.


Aggregates
----------

Some objects may be closely linked together. You can't speak about a stock record in your model without talking about a product. It's event more than that, you should not.
That's an example of aggregates. Some entities (ie remarkable objects distinguished by any kind of ID) may be so coupled that they don't have to be accessed outside their "parent" element.

In our previous example, that means no stock record should be accessed from outside product.

Then if you want to know how much product are available, do:

    class Product(object):
      def get_stock(self):
          self.stock.get_stock_level()

The major benefit using this pattern is to be able to keep the consistancy of the object and to validate your business rules.
Concretly, this save us a lot's of line of defensive code. No more:

    if stock:
        <do something>

and your rules are kept in a safe and visible place. Any one to one DB relationship might be a aggregate.
Definetly useful.

Distilling the core
-------------------

When you project grows, again and again, one solution to keep it maintanable is to package it in loose coupled module. The objective being if you change something in the module A, break nothing in the module B.
Obvious, but how to achieve it. By distilling the core.

There is many techniques for that but one is core for python, called writting framework translated by create a python library to handle the task (and maybe but it on pypi).
You will quickly see your program being a big core and many small technical application. The immediate benefit is you remove a code slice from the main code.
That's now managed by a different code base where you can made some modifications on the framework without breaking the code.
The other benefit is your open source resume will grow. That also matter.

Our B2B project was requesting to parse some CSV files. Parsing CSV file as nothing to do with our plateform main business, we removed it into `django-csv-importer`.
Now, adding extra CSV processor take few minutes, no code duplication and no noise. Job done Sir!








