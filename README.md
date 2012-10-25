variant_mvc a simple library for .net, written in cobra
===========

variant_mvc is (will be) a library to help you organize your code using the mvc pattern.

In this mvc variant, there is a strict separation and dumb views. [[BR]]
- views don't see models, 
- a view only talks to the controller
- models only talk to roles, 
- and there is one controller that contains the roles objects.

the view is passed as an argument when constructing the controller, but it's possible to attach more views.
a role is just an object of a sealed class, a manager for a certain aspect, meant as a word for the single-responsibility-principle.

there is no enforcement of that in the library.
you should describe a role with just one line: "tasked with ..."
if it does too much split it to another ManageSomething class.

models are the domain objects and where the internal rules of a system are defined.
models should be passive, that is, preferably structs with (almost) no methods inside.
the view is "dumb", and you only teach it how to draw its parts.
the controller triggers the redraw_all method in the view (which invokes all the draw_part methods).
however the view can also request to redraw all.
the controller knows the view's parts and can update just a certain part, with the state's simplified data for that part.
since the view doesn't know the state it cannot do it on its own, and relies on the controller.
models have a simplified representation of their state, according to what the view needs to know.
the view should also be taught how to parse this information. it can be a devised notation, a string or a struct.
but this simplified data should remain small and specific to the view's needs.
(the view shouldn't calculate anything)
the controller plays such an important part, everything passes through it,
which allows for a single decision point. (allows for easy record, replay, log, undo/redo, etc. while different views - commmand line, gui, unit testing, interact with one piece of code)
this design is raw, you understand how things work, and is transparent enough to change things easily.

it is also recommended to have static functions that are easily testable, and they only accept primitives and structs as arguments.
the methods in the roles or controller will forward to those static functions.
those static functions have the main functionality and should be unit tested thoroughly.
the controller have the logic and control flow. (recommended to verify the flow with decision tables)

usually the controller will be a large switch case, with validation of arguments and small methods, each tackling a different "command".
PerformAction(action:string) - the command passed is always just a string,
so it is easy to use from any view and to see what happens.
though this means more parsing and validation, I think the benefit outweighs the cons.
you can decide the format, but usually something like
- command											(quit)
- command subcommand								(save state)
- command subcommand arg1:value1 arg2:value2 ...	(file save filename:"c:/...")
(in this format, there is no space after the colon, and a search for opening and closing " is also needed. regexp can probably do those)

it is planned to have a little more helpers for the common usages and some features that easily build upon this design,
but basically it will remain a simple library to help organize your code.

see generic_project for general "pluggable" services for the normal application.