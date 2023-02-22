Hooks
=====

In some cases it might be useful to perform some check between the evaluation of two or more selections, maybe you want to print 
some value to the screen, fill some histogram, check if some precondition is satisfied, etc...

But if you defined you entire Selection sequence in one go there is no way of inserting code between one check and the next one. 
On the other hand, keeping all selections separated only to be able to fill a histogram in between kinda defeats the purpose of
the whole design of this library.

For this exact application NSL allows to define for each selection a "hook", which is a snippet of code that can be run before or
after the evaluation of the selection. The former case is named a "pre-hook", while the latter is called a "post-hook" and can be
selectively run when the check succeeds, fails or in both cases.

The way you attach a hook to a Selection is by calling ``AddPreHook`` or ``AddPostHook``

.. code-block:: cpp
    
    auto my_selection_chain = MySelection1(...).AddPreHook([](NAIA::Event &ev){ /* your hook code here */ }) &&
                              MySelection2(...).AddPostHook([](NAIA::Event &ev){ /* ... */ }, PostHookCondition::OnSuccess) &&
                              /* All your selections ... */;

A hook is represented in code by any kind of function that takes a ``NAIA::Event &`` and returns ``void``. Also, for post-hooks
the ``PostHookCondition`` enum controls when the hook is actually executed.