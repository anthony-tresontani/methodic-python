-----------
Refactoring
-----------

Refactoring is a process to tidy up your code, day after day, like you would do with your bedroom.
Every day, you remove your socks on the ground, your shirt on the chair.
Do it in your code. Simplify useless code, rewrite unreadable algorithm, rename class with a meaningless name.

Code badly write (that happens to everybody) can be easy to find. Just look in your ticketing system where you have many bugs for a simple part of the program.
On my side, I just can't let an awful code be. I am an eraser.

Step by step, you will gradualy improve or at least stabilize your design quality making future change easier.
My main advice is "Don't be lazy, today". Anytime I saw a piece of code and I didn't refactor it by lazyness, it had required few days after lot's more work.
I you are as lazy as I am, invest in the present to prevent the future.

Refactoring don't implies redesign. You clean small piece of code, you do not change all the code infrastructure.
You will most of the time do tiny step in design quality, code more readable and a bit more extensible.
Sometimes, you will jump a cliff and find a awesome design for your previously awefull piece of code.

The only requirement to refactor is first having some unit tests. (TDD power). If you don't, start by writing some to be sure you will not include any regressions.
Never refactor any code without unit tests or you will see the previous code author face saying "you rewrite my code and introduce new bugs, wtf?" (Personnal experience)
You still may introduce new bugs by remove implicit constraints in the previous design.
I did faced this problem a lot with ordering. By magic, the previous code was ordering the output the right way and your more advanced class don't, bbboooouuuhhhh.
Resist the temptation of the abandon and do it, refactor, that always worth.


