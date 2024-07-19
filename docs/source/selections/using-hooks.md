# Hooks

In some cases it might be useful to perform some check between the evaluation of two or more selections, maybe you want to print 
some value to the screen, fill some histogram, check if some precondition is satisfied, etc...

But if you defined you entire Selection sequence in one go there is no way of inserting code between one check and the next one. 
On the other hand, keeping all selections separated only to be able to fill a histogram in between kinda defeats the purpose of
the whole design of this library.

For this exact application NSL allows to define for each selection a "hook", which is a snippet of code that can be run before or
after the evaluation of the selection. The former case is named a "pre-hook", while the latter is called a "post-hook" and can be
selectively run when the check succeeds, fails or in both cases.

The way you attach a hook to a Selection is by calling `AddPreHook` or `AddPostHook`

```cpp
// NSL namespace omitted for clarity
auto my_selection_chain = MySelection1(...).AddPreHook([](NAIA::Event &ev){ /* your hook code here */ }) &&
                          MySelection2(...).AddPostHook([](NAIA::Event &ev){ /* ... */ }, PostHookCondition::OnSuccess) &&
                          /* All your selections ... */;
```

A hook is represented in code by any kind of function that takes a `NAIA::Event &` and returns `void`. Also, for post-hooks
the `PostHookCondition` enum controls when the hook is actually executed. In the last example we added a "pre-hook" to 
`MySelection1` which will always be executed before the selection is checked, and a "post-hook" to `MySelection2` which will
only be executed when the selection is passed.

As a further example, let's say you want to fill a histogram with the inner tracker charge distribution of all events with physical 
trigger and positive beta. You could do that with the following code:

```cpp
// create a chain and add a rootfile to it
NAIA::NAIAChain chain;
chain.Add("some_root_file.root");

// define your selections
auto my_selections = NSL::Trigger::HasPhysicsTrigger() && 
                     NSL::Tof::BetaInRange(0.3, 1.5, NAIA::Tof::BetaType::BetaH);

// create a histogram for the charge distribution plot
TH1D charge_distribution{"charge", ";Q;Counts", 100, 0, 15};

// create the action that fills the charge histogram
// NOTE: since NSL requires that hooks take an event and return void, we need to use lambda capture-by-reference to 
//       have the histogram available inside our hook function
auto fill_charge_distribution = [&charge_distribution](NAIA::Event &ev){
    charge_distribution.Fill(ev.trTrackBase->InnerCharge[NAIA::TrTrack::ChargeRecoType::YJ]);
};

// add our function to our selectinos as a post-hook, to be run when the selections pass
my_selections.AddPostHook(fill_charge_distribution, NSL::PostHookCondition::OnSuccess);

// loop over the events and evaluate the selection(s). This will trigger 
for (NAIA::Event &event : chain){
    if (!my_selections(event)){
        continue;
    }
}

charge_distribution.Draw();
```