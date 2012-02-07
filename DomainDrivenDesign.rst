====================
Domain Driven Design
====================

Yet another driver.
Domain Driven Design is another adjacent approach to TDD which imply understand and solving problem of medium to big project.

Writing software is complex, but should not be complicated. However, we all know that the bigger our project is, the more inertia there is to CRUD ( Create, Read, update or Delete any part of the project). Domain driven design is a part of the solution.

It implies many thinks:
  - Design is alive and unpredictable. Should be apply with agility ie being changed during the whole project lifecycle.

  - Speak the same language than your customer, even in your design. If your customer call a product a SKU, use SKU.
    It will ease to communicate and give more expressiveness to your model.

  - Reveal your intention. For any part on your program, remember it will be more read than write. Than, don't be lazy with your method, variable or class name. Same apply for your test. If you need 50 characters for a method name, do it.

  - Know your core and push out what do not belong to it. Any project implies a bunch of different aspect. UI, database mapper (ORM), interfaces with external solution. Learn to distinguish what is core and what is not and, and, and, push it out.

This is just a small part of the overall domain design and I will only explain the part which bring a real value in my python projects.

Values / Entity
---------------

Values/Entity is a concept shared by many technical programming best seller author. Domain driven highlight it also.

If you have to call something by a unique identifier, that's an entity. An object with it own identity, different from it peers even if they have sames attributes values. Sorry to say that, you are an entity. Being 1,82 meter tall and weighting 80 kilos don't make you identical than your cousin Jojo with the same attributes.

Then, if you manage entity, find a unique identifier in the whole lifecycle of the object. Card processor company use an ARN which is even valid outside your program.

Everything which is not an entity should be a value. Do you mind to have the first of the second piece of cake if they all have the same size, No. I JUST WANT MY CAKE!. Here, if the size attribute is identical, there the Value object is identical.

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




