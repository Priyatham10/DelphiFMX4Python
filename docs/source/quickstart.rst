==========
Quickstart
==========

.. note::
 Before proceeding, install DelphiFMX and make sure it's running properly by
 following the directions provided in :doc:`/installation`.

Let's check for a few available classes, functions, and objects.

.. code-block:: bash
    
    >>> delphifmx.Button
    <class 'Button'>
    >>> delphifmx.Form
    <class 'Form'>
    >>> delphifmx.Application
    <Delphi object of type TApplication at 557897D1F5C2> 

We need to create instances/objects for classes like :doc:`reference/delphifmx.Button` and 
:doc:`reference/delphifmx.Form`. There are many other classes and functions but only one object: 
``Application`` instance, which is an existing singleton instance. ``Application`` is the source 
of every GUI application that we create.

We're compiling three simple guides that explain the basics at a level where you can get started with
the Python package to build some applications.


DelphiFMX Guide 1: The Simplest App
===================================

Let's now create a simple GUI application. The code for it is:

.. code-block:: python

    from delphifmx import *

    # Initialize the GUI Application
    Application.Initialize()
    Application.Title = "Hello DelphiFMX"
    # Create the Form - GUI Window
    app = Form(Application)
    # Set the Window's Caption on Title bar
    app.SetProps(Caption = "Welcome")
    # Set the app object as the MainForm / Main Window
    Application.MainForm = app
    # Show the application on screen
    app.Show()
    # Run the GUI application
    Application.Run()
    # Destroy the GUI application as we close it
    app.Destroy()

Using the above code, we just create an empty GUI app. Please save the above code and 
run it to see the following output:

.. image:: media/images/simple_quickstart.png

Let's explore and understand the functionality of the code:

.. code-block:: python

    from delphifmx import *

    # Initialize the GUI Application
    Application.Initialize()
    Application.Title = "Hello DelphiFMX"

At first, we import everything from ``delphifmx``. Then, we initialized the GUI application and 
set a title for it. Later, we create the GUI application window using the following code:

.. code-block:: python
    
    # Create the Form - GUI Window
    app = Form(Application)
    # Set the GUI Window's Caption on Title bar
    app.SetProps(Caption = "Welcome")

We can to refer all the classes as part of the import as components. The :doc:`reference/delphifmx.Form` 
is a special component and is different from all other components that create the GUI window and 
contain all other components. We instantiated the :doc:`reference/delphifmx.Form` with ``Application`` 
as a parameter in the above code and assigned it to the ``app`` object. All the components, including 
:doc:`reference/delphifmx.Form`, has a method ``SetProps()`` to set their properties. Here we've set 
the name that appears on the title bar of the Form/GUI window using the ``Caption`` property.

Let's look at the following few lines of the code;

.. code-block:: python

    # Show the application on screen
    app.Show()
    # Run the GUI application
    Application.Run()
    # Destroy the GUI application as we close it
    app.Destroy()

As we created the application and set its properties, we shall show it on the screen using the 
``app.show()`` code snippet. ``Application.Run()`` starts the GUI interaction loop between the GUI and the 
user of the GUI application. When we close the GUI application, ``app.Destroy()`` takes care of 
not crashing it.

DelphiFMX Guide 2: Hello, DelphiFMX for Python App
==================================================

We discussed the most basic ideas about the ``delphifmx`` library in the first simplest quickstart. 
We created an empty GUI application without displaying anything on the Form/GUI window. Also, we 
didn't use any object-oriented approach to create the GUI application. So, let's expand on those 
ideas and develop an object-oriented version of that and display a text message.

First, let's look at the code to achieve our idea. You might be able to guess what the below code 
does as you understood the basics from the first guide.

.. code-block:: python

    from delphifmx import *

    class GUIApp(Form):

        def __init__(self, owner):
            self.SetProps(Caption = "Welcome")

            self.lblHello = Label(self)
            self.lblHello.SetProps(
                Parent=self,
                Text="Hello, DelphiFMX for Python")

    def main():
        Application.Initialize()
        Application.Title = "Hello DelphiFMX"
        app = GUIApp(Application)
        Application.MainForm = app
        app.Show()
        Application.Run()
        app.Destroy()

    main()

.. image:: media/images/hello_world_quickstart.png

In the following line of the code:

.. code-block:: python

        app = GUIApp(Application)

Instead of instantiating the :doc:`reference/delphifmx.Form` directly, we instantiated a class - 
``GUIApp`` that inherited the :doc:`reference/delphifmx.Form` class. Let's investigate the 
code in the ``GUIApp`` class:

.. code-block:: python

    class GUIApp(Form):

        def __init__(self, owner):
            self.SetProps(Caption = "Welcome")

            self.lblHello = Label(self)
            self.lblHello.SetProps(
                Parent=self,
                Text="Hello, DelphiFMX for Python")

As we instantiated the ``GUIApp`` using ``app = GUIApp(Application)``, the ``owner`` argument gets 
assigned with the ``Application`` object. After that, :doc:`reference/delphifmx.Form` uses the 
``owner`` in its initialization and creates an empty Form/GUI window. This ``owner`` variable can 
be of any other name as it's just a placeholder of the ``Application`` object. In the first line 
of the ``GUIApp`` initialization, we've set the ``Caption`` property of the :doc:`reference/delphifmx.Form`.

Then we instantiated the :doc:`reference/delphifmx.Label` component/class with the instance/object 
of the :doc:`reference/delphifmx.Form` as its parameter using the ``self.lblHello = Label(self)`` 
code snippet. We use :doc:`reference/delphifmx.Label` to display any single-line text messages. 
Every component other than :doc:`reference/delphifmx.Form` will have a parent and is set using 
the ``Parent`` property. The parent holds the child component in it.

In our code, we're setting :doc:`reference/delphifmx.Label`'s parent as :doc:`reference/delphifmx.Form` 
using the ``Parent=self``. So, now the :doc:`reference/delphifmx.Form` object - ``app`` holds the 
:doc:`reference/delphifmx.Label` object - ``lblHello``. Next, the text of the :doc:`reference/delphifmx.Label` 
is set using its ``Text`` property. So, the Form/GUI window gets populated by a text message - **Hello, 
DelphiFMX for Python**.

We used all the default positions and sizes of the :doc:`reference/delphifmx.Form` and :doc:`reference/delphifmx.Label` 
and didn't handle any events in this guide. However, we shall implement them and introduce some new 
components in the following advanced quick start guide.

DelphiFMX Guide 3: The TODO App
===============================

Let us create a TODO Task Application to understand some components of GUI Applications.

Let's take a look at the code to achieve that:

.. code-block:: python

    from delphifmx import *

    class TodoApp(Form):

        def __init__(self, Owner):
            self.Caption = "A TODO GUI Application"
            self.SetBounds(100, 100, 700, 500)

            self.task_lbl = Label(self)
            self.task_lbl.SetProps(Parent=self, Text="Enter your TODO task")
            self.task_lbl.SetBounds(10, 10, 125, 25)

            self.task_text_box = Edit(self)
            self.task_text_box.SetProps(Parent=self)
            self.task_text_box.SetBounds(10, 30, 250, 20)

            self.add_task_btn = Button(self)
            self.add_task_btn.Parent = self
            self.add_task_btn.SetBounds(150, 75, 100, 30)
            self.add_task_btn.Caption = "Add Task"
            self.add_task_btn.OnClick = self.__add_task_on_click

            self.del_task_btn = Button(self)
            self.del_task_btn.SetProps(Parent = self, Text = "Delete Task")
            self.del_task_btn.SetBounds(150, 120, 100, 30)
            self.del_task_btn.OnClick = self.__del_task_on_click

            self.list_of_tasks = ListBox(self)
            self.list_of_tasks.Parent = self
            self.list_of_tasks.SetBounds(300, 50, 300, 350)

        def __add_task_on_click(self, Sender):
            self.list_of_tasks.Items.Add(self.task_text_box.Text)
            self.task_text_box.Text = ""

        def __del_task_on_click(self, Sender):
            self.list_of_tasks.Items.Delete(0)

    def main():
        Application.Initialize()
        Application.Title = "TODO App"
        app = TodoApp(Application)
        Application.MainForm = app
        app.Show()
        Application.Run()
        app.Destroy()
        
    main()

As you save and run the above code, you should get the following GUI as a result:

.. image:: media/images/todo_quickstart_1.png

Let's get to the details of what our code does behind the scenes. First, take a look at 
the ``main()`` function:

.. code-block:: python

    def main():
        Application.Initialize()
        Application.Title = "TODO App"

In the above, ``Application`` instance is part of the ``delphifmx`` library that takes 
control of the GUI applications that we create. First line initializes the application, 
and the second line sets a title to the application.

Let's look at other lines of code of the ``main()`` function;

.. code-block:: python

    ...
        app = TodoApp(Application)
        Application.MainForm = app
        app.Show()
        Application.Run()
        app.Destroy()

Above, We instantiated the ``TodoApp`` class with ``Application`` as the ``Owner``. We 
can show the GUI application on the screen using the ``app.show()`` method. GUI applications 
run in interaction with the command window (console). 
``Application.Run()`` starts the GUI interaction loop between the GUI and the user of the 
GUI application. When we close the GUI application, ``app.Destroy()`` takes care of not 
crashing it.

As we instantiated the GUI using ``app = TodoApp(Application)``, the following code runs:

.. code-block:: python

    class TodoApp(Form):

        def __init__(self, Owner):
            self.Caption = "A TODO GUI Application"
            self.SetBounds(100, 100, 700, 500)

We inherit the :doc:`reference/delphifmx.Form` class from the ``delphifmx`` library to create 
our GUI. In DelphiFMX, all the GUIs are treated as forms. The name of the GUI pop-up window 
is set using the ``Caption`` property/attribute. The line ``self.SetBounds(100, 100, 700, 500)`` 
is used to set:

- GUI window's origin position comparable to screen's origin position = (100, 100)
- length of the GUI window = 700 pixels
- width of the GUI window = 500 pixels.

The upper left corner of the screen is treated as the ``(0, 0)`` coordinate with the left side 
as positive width and down as positive height. We can visualize it as shown below:

.. image:: media/images/todo_quickstart_2.png

Let's look at the following few lines of code:

.. code-block:: python

    ...
            self.task_lbl = Label(self)
            self.task_lbl.SetProps(Parent=self, Text="Enter your TODO task")
            self.task_lbl.SetBounds(10, 10, 125, 25)
            
            self.task_text_box = Edit(self)
            self.task_text_box.Parent = self
            self.task_text_box.SetBounds(10, 30, 250, 20)

Above the first 3 lines of code will create the text - **Enter your TODO task** that you see on 
the GUI app. It does so by instantiating the :doc:`reference/delphifmx.Label` class of the 
``delphifmx`` library. Every component (:doc:`reference/delphifmx.Label` here) has a ``SetProps()`` 
method to set its properties. Every component will have a scope that is set using its ``Parent`` 
property/attribute, which is set to ``self`` here. The ``Text`` property sets the string of the 
text label. Similar to GUI app/Form, every component needs to be placed inside the GUI/Form using 
the ``SetBounds()`` method. For components, the top left corner of their parent (GUI window here) 
is considered as the origin - ``(0, 0).``

The next 3 lines of code create the edit box using the :doc:`reference/delphifmx.Edit` class. 
We can also set the properties/attributes directly without using the ``SetProps()`` method like 
we did here using the code ``self.task_text_box.Parent = self``. With the Form/GUI window as 
the parent of the Edit box, we can visualize its position and size as shown in the below figure. 
The width of the Edit box is automatically set to the default value.

.. image:: media/images/todo_quickstart_3.png

Let's look at next few lines of code:

.. code-block:: python

    ...
            self.add_task_btn = Button(self)
            self.add_task_btn.Parent = self
            self.add_task_btn.SetBounds(150, 75,100,30)
            self.add_task_btn.Text = "Add Task"
            self.add_task_btn.OnClick = self.__add_task_on_click

            self.del_task_btn = Button(self)
            self.del_task_btn.SetProps(Parent = self, Caption = "Delete Task")
            self.del_task_btn.SetBounds(150,120,100,30)
            self.del_task_btn.OnClick = self.__del_task_on_click

Above lines of code create 2 Buttons - ``Add Task`` and ``Delete Task`` using the 
:doc:`reference/delphifmx.Button` instance of the ``delphifmx`` package. For the buttons, one extra 
thing you'll find is an event handling using ``self.add_task_btn.OnClick = self.__add_task_on_click`` 
and ``self.del_task_btn.OnClick = self.__del_task_on_click`` for ``Add Task`` and ``Delete Task`` 
buttons respectively. We shall look after this in just a while.

Let's look at the next few lines of code:

.. code-block:: python

    ...
            self.list_of_tasks = ListBox(self)
            self.list_of_tasks.Parent = self
            self.list_of_tasks.SetBounds(300,50,300,350)

In the above lines of code, we created a list box using the :doc:`reference/delphifmx.ListBox` 
instance. Let's now look at the event handling methods to ``Add Task`` and ``Delete Task`` 
buttons:

.. code-block:: python

    ...
        def __add_task_on_click(self, Sender):
            self.list_of_tasks.Items.Add(self.task_text_box.Text)
            self.task_text_box.Text = ""

        def __del_task_on_click(self, Sender):
            self.list_of_tasks.Items.Delete(0)

For all the events other than ``OnClick``, the :doc:`reference/delphifmx.Form` automatically 
sends a single argument (``Sender`` here - this can be any name). We can add a task to the 
list box by typing anything into the text box and pressing on ``Add Task`` button. DelphiFMX 
library based GUIs support tab controls too, where you can also navigate from one component 
to another using the tab. So, you can press the Tab key on the keyboard, and as ``Add Task`` 
button gets highlighted, you can press Enter/Return key to fire its event. We add text from 
the text box to the list box using ``Add()`` method under ``Items`` under 
:doc:`reference/delphifmx.ListBox` instance. We delete the earlier added events on a first-come, 
first-serve basis by pressing the ``Delete Task`` button.



************
You're done!
************

With a working installation of DelphiFMX and these sample projects under your belt,
you're ready to start creating applications of your own.