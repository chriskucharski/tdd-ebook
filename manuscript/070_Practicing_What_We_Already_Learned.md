Practicing what we have already learned
==================================

> And now, a taste of things to come!
>
>  -- Shang Tsung, Mortal Kombat The Movie

The above quote took place just before a fighting scene in which a nameless warrior jumped at Sub-Zero only to be frozen and broken into multiple pieces upon hitting the wall. The scene was not spectacular in terms of fighting technique or length. Also, the nameless guy did not even try hard -- the only thing he did was to jump only to be hit by a freezing ball, which, by the way, he actually saw coming. It looked a lot like the fight was set up only to showcase Sub-Zero’s freezing ability. Guess what? In this chapter, we are gonna do roughly the same thing -- set up a fake, easy scenario just to showcase some of the basic TDD elements!

The previous chapter was filled with a lot of theory and philosophy, don’t you think? I really hope you did not fall asleep while reading it. To tell you the truth, we need to grasp much more theory until we are really able to write real-world applications using TDD. To compensate for this somehow, I propose we make a side trip from the trail and try what we already learned on a quick and easy example. As we go through the example, you might wonder how on earth could you possibly write real applications the way we will write our simple program. Do not worry, I will not show you all the tricks yet, so treat it as a "taste of things to come". In other words, the example will be as close to real world problems as the fight between Sub-Zero and nameless ninja was to real martial arts fight, but will show you some of the elements of TDD process.

Let me tell you a story 
-----------------------

Meet Johnny and Benjamin, two developers from Buthig Company. Johnny is quite fluent in programming and Test-Driven Development, while Benjamin is an intern under Johnny’s mentorship and is eager to learn TDD. They are on their way to their customer, Jane, who requested their presence as she wants them to write a small program for her. Along with them, we will see how they interact with the customer and how Benjamin tries to understand the basics of TDD. Like you, Benjamin is a novice so his questions may reflect yours. However, if you find anything explained in not enough details, do not worry -- in the next chapters, we will be expanding on this material.

Act 1: The Car
--------------

**Johnny:** How do you feel about your first assignment?

**Benjamin:** I am pretty excited! I hope I can learn some of the TDD stuff you promised to teach me.

**Johnny:** Not only TDD, but we are also gonna use some of the practices associated with a process called Acceptance Test-Driven Development, albeit in a simplified form.

**Benjamin:** Acceptance Test-Driven Development? What is that?

**Johnny:** While TDD is usually referred to as a development technique, ATDD is something more of a collaboration method. Both ATDD and TDD have a bit of analysis in them and work very well together as both use the same underlying principles, just on different levels. We will need only a small subset of what ATDD has to offer, so do not get over-excited.

**Benjamin:** Sure.

Act 2: The Customer
-------------------

**Johnny:** Hi, Jane, how are you?

**Jane:** Thanks, I am fine, how about you?

**Johnny:** Same here, thanks. So, can you tell us a bit about the software you need us to write?

**Jane:** Sure. Recently, I bought a new smartphone as a replacement for my old one. The thing is, I am really used to the calculator application that was running on the previous phone and I cannot find it for my current device.

**Benjamin:** Can't you just use another calculator app? There are plenty of them available to download from the web.

**Jane:** That is right. I checked them all and none has exactly the same behavior as the one I was using for my tax calculations. You know, the program was like right hand to me and it had some really nice shortcuts that made my life easier.

**Johnny:** So you want us to reproduce the application to run on your new device?

**Jane:** Yes.

**Johnny:** Are you aware that apart from the fancy features that you were using we will have to allocate some effort to implement the basics that all the calculators have?

**Jane:** Sure, I am OK with that. I am so used to my calculator application that if I use something else for more than a few months, I will have to pay a psychotherapist instead of you guys. Apart from that, writing a calculator app seems like a simple task in my mind, so the cost isn’t going to be overwhelming, right?

**Johnny:** I think I get it. Let's get it going then. We will be implementing the functionality incrementally, starting with the most essential features. Which feature of the calculator would you consider the most essential?
 
**Jane:** That would be addition of numbers, I guess.

**Johnny:** Ok, that will be our target for the first iteration. After the iteration, we will deliver this part of the functionality for you to try out and give us some feedback. However, before we can even implement addition, we will have to implement displaying digits on the screen as you enter them. Is that correct?

**Jane:** Yes, I want the display stuff to work as well -- it is the most basic feature, so...

**Johnny:** Ok then, this is a simple functionality, so let me suggest some user stories as I understand what you already said and you will correct me where I am wrong. Here we go:

1.  **In order to** know that the calculator is turned on, **As a** tax payer **I want** to see "0" on the screen as soon as I turn it on.
2.  **In order to** see what numbers I am currently operating on, **As a** tax payer, **I want** the calculator to display the values I enter
3.  **In order to** calculate the sum of my different incomes, **As a** tax payer **I want** the calculator to enable addition of multiple numbers

What do you think?

**Jane**: The stories pretty much reflect what I want for the first iteration. I do not think I have any corrections to make.

**Johnny:** Now we will take each story and collect some examples of how it should work.

**Benjamin:** Johnny, don’t you think it is obvious enough to proceed with implementation straight away?

**Johnny:** Trust me, Benjamin, if there is one word I fear most in communication, it is "obvious". Miscommunication happens most often around things that people consider obvious, simply because other people do not.

**Jane:** Ok, I am in. What do I do?

**Johnny:** Let's go through stories one by one and see if we can find some key examples of how the features work. The first story is...

### **In order to** know that the calculator is turned on, **As a** tax payer **I want** to see "0" on the screen as soon as I turn it on.

**Jane:** I do not think there is much to talk about. If you display "0", I will be happy. That is all.

**Johnny:** Let's write down this example:

| key sequence | Displayed output | Notes                   |
|--------------|------------------|-------------------------|
| N/A          | 0                | Initial displayed value |

**Benjamin:** That makes me wonder... what should happen when I press "0" again at this stage?

**Johnny:** Good catch, that is what these examples are for -- they make our thinking concrete. As Ken Pugh says: "Often the complete understanding of a concept does not occur until someone tries to use the concept". We would normally put it on a TODO list, because it is part of a different story, but as we are done with this one, let's move straight to the story about displaying entered digits. How about it, Jane?

**Jane:** Agree.

### **In order to** see what numbers I am currently operating on, **As a** tax payer, **I want** the calculator to display the values I enter

**Johnny:** Let's begin with the case raised by Benjamin. What should happen when I input "0" multiple times after I have only "0" on the display?

**Jane:** Just one "0" should be displayed

**Johnny:** Do you mean this?

| key sequence | Displayed output | Notes                                              |
|--------------|------------------|----------------------------------------------------|
| 0,0,0        | 0                | Zero is a special case – it is displayed only once |

**Jane:** That is right. Other than this, the digits should just show on the screen, like this:

| key sequence | Displayed output | Notes                        |
|--------------|------------------|------------------------------|
| 1,2,3        | 123              | Entered digits are displayed |

**Benjamin:** How about this:

| key sequence               | Displayed output | Notes                         |
|----------------------------|------------------|-------------------------------|
| 1,2,3,4,5,6,7,1,2,3,4,5,6  | 1234567123456?   | Entered digits are displayed? |

**Jane:** Actually, no. My old calculator app has a limit of six digits that I can enter, so it should be:

| key sequence              | Displayed output  | Notes                         |
|---------------------------|-------------------|-------------------------------|
| 1,2,3,4,5,6,7,1,2,3,4,5,6 | 123456            | Display limited to six digits |

**Johnny:** Another good catch, Benjamin!

**Benjamin:** I think I am beginning to understand why you like working with examples!

**Johnny:** Good. Is there anything else, Jane?

**Jane:** No, that is pretty much it. Let's start working on another story.

### **In order to** calculate sum of my different incomes, **As a** tax payer **I want** the calculator to enable addition of multiple numbers

**Johnny:** Is the following all we have to support?

| key sequence | Displayed output | Notes                      |
|--------------|------------------|----------------------------|
| 2,+,3,+,4,=  | 9                | Simple addition of numbers |

**Jane:** This scenario is correct, however, there is also a case when I start with "+" without inputting any number before. This should be treated as adding to zero:

| key sequence | Displayed output | Notes                              |
|--------------|------------------|------------------------------------|
| +,1,=        | 1                | Addition shortcut – treated as 0+1 |

**Benjamin:** How about when the output is a number longer than six digits limit? Is it OK that we truncate it like this?

| key sequence                | Displayed output | Notes                                     |
|-----------------------------|------------------|-------------------------------------------|
| 9,9,9,9,9,9,+,9,9,9,9,9,9,= | 199999           | Our display is limited to six digits only |

**Jane:** Sure, I do not mind. I do not add such big numbers anyway.

**Johnny:** There is still one question we missed. Let's say that I input a number, then press "+" and then another number without asking for result with "=". What should I see?

**Jane:** Every time you press "+", the calculator should consider entering current number finished and overwrite it as soon as you press any other digit:

| key sequence | Displayed output | Notes                                                                                             |
|--------------|------------------|---------------------------------------------------------------------------------------------------|
| 2,+,3        | 3                | Digits entered after + operator are treated as digits of a new number, the previous one is stored |

**Jane:** Oh, and just asking for result after the calculator is turned on should result in "0".

| key sequence | Displayed output | Notes                             |
|--------------|------------------|-----------------------------------|
| =            | 0                | Result key in itself does nothing |

**Johnny:** Let's sum up our discoveries:

| key sequence                | Displayed output | Notes    |
|-----------------------------|------------------|---------------------------------------------------------------------------------------------------|
| N/A                         | 0                | Initial displayed value |
| 1,2,3                       | 123              | Entered digits are displayed |
| 0,0,0                       | 0                | Zero is a special case – it is displayed only once |
| 1,2,3,4,5,6,7               | 123456           | Our display is limited to six digits only |
| 2,+,3                       | 3                | Digits entered after + operator are treated as digits of a new number, the previous one is stored |
| =                           | 0                | Result key in itself does nothing |
| +,1,=                       | 1                | Addition shortcut – treated as 0+1 |
| 2,+,3,+,4,=                 | 9                | Simple addition of numbers |
| 9,9,9,9,9,9,+,9,9,9,9,9,9,= | 199999           | Our display is limited to six digits only |

**Johnny:** The limiting of digits displayed looks like a whole new feature, so I suggest we add it to the backlog and do it in another sprint. In this sprint, we will not handle such situation at all. How about that, Jane?

**Jane:** Fine with me. Looks like a lot of work. Nice that we discovered it up-front. For me, the limiting capability seemed so obvious that I did not even think it would be worth mentioning.

**Johnny:** See? That is why I do not like the word "obvious". Jane, we will get back to you if any more questions arise. For now, I think we know enough to implement these three stories for you.

**Jane:** good luck!

Act 3: Test-Driven Development
------------------------------

**Benjamin:** Wow, that was cool. Was that Acceptance Test-Driven Development?

**Johnny:** In a greatly simplified version, yes. The reason I took you with me was to show you the similarities between working with customer the way we did and working with the code using TDD process. They are both applying the same set of principles, just on different levels.

**Benjamin:** I am dying to see it with my own eyes. Shall we start?

**Johnny:** Sure. If we followed the ATDD process, we would start writing what we call acceptance-level specification. In our case, however, a unit-level specification will be enough. Let's take the first example:

### Statement 1: Calculator should display 0 on creation

| key sequence | Displayed output | Notes                   |
|--------------|------------------|-------------------------|
| N/A          | 0                | Initial displayed value |

**Johnny:** Benjamin, try to write the first Statement.

**Benjamin:** Boy, I do not know how to start.

**Johnny:** Start by writing the statement in plain English. What should the calculator do?

**Benjamin:** It should display "0" when I turn the application on.

**Johnny:** In our case, "turning on" is creating a calculator. Let's write it down as a method name:

```csharp
public class CalculatorSpecification
{

[Fact] public void
ShouldDisplay0WhenCreated()
{

}

}
```

**Benjamin:** Why is the name of the class `CalculatorSpecification` and the name of the method `ShouldDisplay0WhenCreated`?

**Johnny:** It is a naming convention. There are many others, but this is the one that I like. The rule is that when you take the name of the class without the `Specification` part followed by the name of the method, it should form a legit sentence. For instance, if I apply it to what we wrote, it would make a sentence: "Calculator should display 0 when created".

**Benjamin:** Ah, I see now. So it is a statement of behavior, isn’t it?

**Johnny:** That is right. Now, the second trick I can sell to you is that if you do not know what code to start with, start with the expected result. In our case, we are expecting that the behavior will end up as displaying "0", right? So let's just write it in the form of an assertion.

**Benjamin:** You mean something like this?

```csharp
public class CalculatorSpecification
{

[Fact] public void
ShouldDisplay0WhenCreated()
{
 Assert.Equal("0", displayedResult);
}

}
```

**Johnny:** Precisely.

**Benjamin:** But that does not even compile. What use is it?

**Johnny:** The code not compiling is the feedback that you needed to proceed. While previously you did not know where to start, now you have a clear goal -- make this code compile. Firstly, where do you get the displayed value from?

**Benjamin:** From the calculator display, of course!

**Johnny:** Then write down how you get the value form the display.

**Benjamin:** Like how?

**Johnny:** Like this:

```csharp
public class CalculatorSpecification
{

[Fact] public void
ShouldDisplay0WhenCreated()
{
 var displayedResult = calculator.Display();

 Assert.Equal("0", displayedResult);
}

}
```

**Benjamin:** I see. Now the calculator is not created anywhere. I need to create it somewhere now or it will not compile and this is my next step. Is this how it works?

**Johnny:** Yes, you are catching on quickly.

**Benjamin:** Ok then, here goes:

```csharp
public class CalculatorSpecification
{

[Fact] public void
ShouldDisplay0WhenCreated()
{
 var calculator = new Calculator();

 var displayedResult = calculator.Display();

 Assert.Equal("0", displayedResult);
}

}
```

**Johnny:** Bravo!

**Benjamin:** Now the code still does not compile, because I do not have the `Calculator` class at all...

**Johnny:** Sounds like a good reason to create it.

**Benjamin:** OK.

```csharp
public class Calculator
{
}
```

**Benjamin:** Looks like the `Display()` method is missing too. I will add it.

```csharp
public class Calculator
{
  public string Display()
  {
    return "0";
  } 
}
```

**Johnny:** Hey hey, not so fast!

**Benjamin:** What?

**Johnny:** You already provided an implementation that will make our current Statement true. Remember its name? `ShouldDisplay0WhenCreated` -- and that is exactly what the code you wrote does. Before we arrive at this point, let's make sure this Statement can ever be evaluated as false. You won't achieve this by providing a correct implementation out of the box. So for now, let's change it to this:

```csharp
public class Calculator
{
  public string Display()
  {
    return "Once upon a time in Africa";
  } 
}
```

**Johnny:** Look, now we can run the Specification and watch that Statement evaluate to false, because it expects "0", but gets "Once upon a time in Africa".

**Benjamin:** Running... Ok, it is false. By the way, do you always use such silly values to make Statements false?

**Johnny:** Hahaha, no, I just did it to emphasize the point. Normally, I would write `return "";` or something similarly simple. Now we can evaluate the Statement and see it evaluate to false. Now we are sure that we have not yet implemented what is required for the Statement to be true.

**Benjamin:** I think I get it. For now, the Statement shows that we do not have something we need and gives us a reason to add this "thing". When we do so, this Statement will show that we do have what we need. So what do we do now?

**Johnny:** Write the simplest thing that makes this Statement true.

**Benjamin:** like this?

```csharp
public class Calculator
{
  public string Display()
  {
    return "0";
  } 
}
```

**Johnny:** Yes.

**Benjamin:** But that is not a real implementation. What is the value behind putting in a hardcoded string? The final implementation is not going to be like this for sure!

**Johnny:** You’re right. The final implementation is most probably going to be different. What we did, however, is still valuable because:

1.  You’re one step closer to implementing the final solution
2.  This feeling that this is not the final implementation points you towards writing more Statements. When there is enough Statements to make your implementation complete, it usually means that you have a complete Specification of class behaviors as well.
3.  If you treat making every Statement true as an achievement, this practice allows you to evolve your code without losing what you already achieved. If by accident you break any of the behaviors you’ve already implemented, the Specification is going to tell you because one of the existing Statements that were previously true will then evaluate to false. You can then either fix it or undo your changes using version control and start over from the point where all existing Statements were true.

**Benjamin:** Looks like there are quite a few benefits. Still, I will have to get used to this kind of working.

**Johnny:** Do not worry, it is central to TDD, so you will grasp it in no time. Now, before we proceed to the next Statement, let's look at what we already achieved. First, we wrote a Statement that turned out false. Then, we wrote just enough code to make the Statement true. Time for a step called Refactoring. In this step, we will take a look at the Statement and the code and remove duplication. Can you see what is duplicated between the Statement and the code?

**Benjamin:** both of them contain the literal "0". The Statement has it here:

```csharp
Assert.Equal("0", displayedResult);
```

and the implementation here:

```csharp
return "0";
```

**Johnny:** Good, let's eliminate this duplication by introducing a constant called `InitialValue`. The Statement will now look like this:

```csharp
[Fact] public void
ShouldDisplayInitialValueWhenCreated()
{
 var calculator = new Calculator();

 var displayedResult = calculator.Display();

 Assert.Equal(Calculator.InitialValue, displayedResult);
}
```

and the implementation:

```csharp
public class Calculator
{
  public const string InitialValue = "0";
  public string Display()
  {
    return InitialValue;
  } 
}
```

**Benjamin:** The code looks better and having the constant in one place will make it more maintainable, but I think the Statement in its current form is weaker than before. We could change the `InitialValue` to anything and the Statement would still be true, since it does not force this constant to be "0".

That is right. We need to add it to our TODO list to handle this case. Can you write it down?

**Benjamin:** Sure. I will write it as "TODO: 0 should be used as an initial value."

**Johnny:** Ok. We should handle it now, especially since it is part of the story we are currently implementing, but I will leave it for later just to show you the power of TODO list in TDD -- whatever is on the list, we can forget and get back to when we have nothing better to do. Our next item from the list is this:

### Statement 2: Calculator should display entered digits

| key sequence | Displayed output | Notes                        |
|--------------|------------------|------------------------------|
| 1,2,3        | 123              | Entered digits are displayed |

**Johnny:** Benjamin, can you come up with a Statement for this behavior?

**Benjamin:** I will try. Here goes:

```csharp
[Fact] public void 
ShouldDisplayEnteredDigits()
{
  var calculator = new Calculator();
  
  calculator.Enter(1); 
  calculator.Enter(2);
  calculator.Enter(3);
  var displayedValue = calculator.Display();

  Assert.Equal("123", displayedValue);
}
```

**Johnny:** I am glad that you got the part about naming and writing a Statement. There is one thing we will have to work on here though.

**Benjamin:** What is it?

**Johnny:** When we talked to Jane, we used examples with real values. These real values were extremely helpful in pinning down the corner cases and uncovering missing scenarios. They were easier to imagine as well, so they were a perfect suit for conversation. If we were automating these examples on acceptance level, we would use those real values as well. When we write unit-level Statements, however, we use a different technique to get this kind of specification more abstract. First of all, let me enumerate the weaknesses of the approach you just used:

1.  Making a method `Enter()` accept an integer value suggests that one can enter more than one digit at once, e.g. `calculator.Enter(123)`, which is not what we want. We could detect such cases and throw exceptions if the value is outside the 0-9 range, but there are better ways when we know we will only be supporting ten digits (0,1,2,3,4,5,6,7,8,9).
2.  The Statement does not clearly show the relationship between input and output. Of course, in this simple case it is pretty self-evident that the sum is a concatenation of entered digits, we do not want anyone who will be reading this Specification in the future to have to guess.
3.  The Statement suggests that what you wrote is sufficient for any value, which isn’t true, since the behavior for "0" is different (no matter how many times we enter "0", the result is just "0")

Hence, I propose the following:

```csharp
[Fact] public void 
ShouldDisplayAllEnteredDigitsThatAreNotLeadingZeroes()
{
 //GIVEN
 var calculator = new Calculator();
 var nonZeroDigit = Any.Besides(DigitKeys.Zero);
 var anyDigit1 = Any.Of<DigitKeys>();
 var anyDigit2 = Any.Of<DigitKeys>();

 //WHEN
 calculator.Enter(nonZeroDigit);
 calculator.Enter(anyDigit1);
 calculator.Enter(anyDigit2);
		  
 //THEN
 Assert.Equal(
  string.Format("{0}{1}{2}", 
   (int)nonZeroDigit, 
   (int)anyDigit1, 
   (int)anyDigit2
  ),
  calculator.Display()
 );
}
```

**Benjamin:** Johnny, I am lost! Can you explain what is going on here?

**Johnny:** Sure, what do you want to know?

**Benjamin:** For instance, what is this `DigitKeys` type doing here?

**Johnny:** It is supposed to be an enumeration (note that it does not exist yet, we just assume that we have it) to hold all the possible digits a user can enter, which are 0-9. This is to ensure that the user will not write `calculator.Enter(123)`. Instead of allowing our users to enter any number and then detecting errors, we are giving them a choice from among only the valid values.

**Benjamin:** Now I get it. So how about the `Any.Besides()` and `Any.Of()`? What do they do?

**Johnny:** They are methods from a small utility library I am using when writing unit-level Specifications. `Any.Besides()` returns any value from enumeration besides the one supplied as an argument. Hence, the call `Any.Besides(DigitKeys.Zero)` means "any of the values contained in DigitKeys enumeration, but not DigitKeys.Zero".

The `Any.Of()` is simpler -- it just returns any value in an enumeration. Note that when I create the values this way, I state explicitly that this behavior occurs when first digit is non-zero. This technique of using generated values instead of literals has its own principles, but let's leave it for later. I promise to give you a detailed lecture on it. Agree?

**Benjamin:** You better do, because for now, I feel a bit uneasy with generating the values -- it seems like the Statement we are writing is getting less deterministic this way. The last question -- what about those weird comments you put in the code? Given? When? Then?

**Johnny:** Yes, this is a convention that I use, not only in writing, but in thinking as well. I like to think about every behavior in terms of three elements: assumptions (given), trigger (when) and expected result (then). Using the words, we can summarize the Statement we are writing in the following way: "**Given** a calculator, **when** I enter some digits, the first one being non-zero, **then** they should all be displayed in the order they were entered". This is also something that I will tell you more about later.

**Benjamin:** Sure, for now I need just enough detail to understand what is going on -- we can talk about the principles, pros and cons later. By the way, the following sequence of casts looks a little bit ugly:

```csharp
string.Format("{0}{1}{2}", 
 (int)nonZeroDigit, 
 (int)anyDigit1, 
 (int)anyDigit2
)
```

**Johnny:** We will get back to it and make it "smarter" in a second after we make this statement true. For now, we need something obvious. Something we know works. let's evaluate this Statement. What is the result?

**Benjamin:** Failed, expected "331", but was "0".

**Johnny:** Good, now let's write some code to make this Statement true. First, we're going to introduce an enumeration of digits:

```csharp
public enum DigitKeys
{
 Zero = 0,
 TODO1, //TODO
 TODO2, //TODO
 TODO3, //TODO
 TODO4, //TODO
}
```

**Benjamin:** What is with all those bogus values? Should we not just enter all the digits we support?

**Johnny:** Nope, not yet. We still do not have a Statement which would say what digits are supported and thus make us add them, right?

**Benjamin:** You say you need a Statement for an element to be in an enum?

**Johnny:** This is a specification we are writing, remember? It should say somewhere which digits we support, should it not?

**Benjamin:** It is difficult to agree with, especially since I am used to writing unit tests, not Statements, and, in unit tests, I wanted to verify what I was unsure of.

**Johnny:** I will try to give you more arguments later. For now, just bear with me and note that adding such Statement will be almost effortless.

**Benjamin:** OK.

**Johnny:** Now for the implementation. Just to remind you -- up to now, it looked like this:

```csharp
public class Calculator
{
 public const string InitialValue = "0";
 public string Display()
 {
  return InitialValue;
 } 
}
```

This is not enough to support displaying multiple digits (as we saw, because the Statement saying they should be supported was evaluated to false). So let's evolve the code to handle this case:

```csharp
public class Calculator
{
 private int _result = 0;
	  
 public void Enter(DigitKeys digit)
 {
  _result *= 10;
  _result += (int)digit; 
 }

 public string Display()
 {
  return _result.ToString();
 }
}
```

**Johnny:** Now the Statement is true so we can go back to it and make it a little bit prettier. Let's take a second look at it:

```csharp
[Fact] public void 
ShouldDisplayAllEnteredDigitsThatAreNotLeadingZeroes()
{
 //GIVEN
 var calculator = new Calculator();
 var nonZeroDigit = Any.Besides(DigitKeys.Zero);
 var anyDigit1 = Any.Of<DigitKeys>();
 var anyDigit2 = Any.Of<DigitKeys>();
		  
 //WHEN
 calculator.Enter(nonZeroDigit);
 calculator.Enter(anyDigit1);
 calculator.Enter(anyDigit2);
		  
 //THEN
 Assert.Equal(
  string.Format("{0}{1}{2}", 
   (int)nonZeroDigit, 
   (int)anyDigit1, 
   (int)anyDigit2
  ),
  calculator.Display()
 );
}
```

**Johnny:** Remember you said that you do not like the part where `string.Format()` is used?

**Benjamin:** Yeah, it seems a bit unreadable.

**Johnny:** Let's extract this part into a utility method and make it more general -- we will need a way of constructing expected displayed output in many of our future Statements. Here is my go at this helper method:

```csharp
string StringConsistingOf(params DigitKeys[] digits)
{
 var result = string.Empty;
	  
 foreach(var digit in digits) 
 {
  result += (int)digit;
 }
 return result;
}
```

Note that this is more general as it supports any number of parameters. And the Statement after this extraction looks like this:

```csharp
[Fact] public void 
ShouldDisplayAllEnteredDigitsThatAreNotLeadingZeroes()
{
 //GIVEN
 var calculator = new Calculator();
 var nonZeroDigit = Any.Besides(DigitKeys.Zero);
 var anyDigit1 = Any.Of<DigitKeys>();
 var anyDigit2 = Any.Of<DigitKeys>();
		  
 //WHEN
 calculator.Enter(nonZeroDigit);
 calculator.Enter(anyDigit1);
 calculator.Enter(anyDigit2);
		  
 //THEN
 Assert.Equal(
  StringConsistingOf(nonZeroDigit, anyDigit1, anyDigit2),
  calculator.Display()
 );
}
```

**Benjamin:** Looks better to me. The Statement is still evaluated as true, which means we got it right, did we not?

**Johnny:** Not exactly. With moves such as this one, I like to be extra careful. Let's comment out the body of the `Enter()` method and see if this Statement can still be made false by the implementation:

```csharp
public void Enter(DigitKeys digit)
{
 //_result *= 10;
 //_result += (int)digit; 
}
```

**Benjamin:** Running... Ok, it is false now. Expected "243", got "0".

**Johnny:** Good, now we are pretty sure that it works OK. Let's uncomment the lines we just commented out and move forward.

### Statement 3: Calculator should display only one zero digit if it is the only entered digit even if it is entered multiple times 

**Johnny:** Benjamin, this should be easy for you, so go ahead and try it. It is really a variation of previous Statement.

**Benjamin:** Let me try... ok, here it is:

```csharp
[Fact] public void 
ShouldDisplayOnlyOneZeroDigitWhenItIsTheOnlyEnteredDigitEvenIfItIsEnteredMultipleTimes()
{
 var zero = DigitKeys.Zero;
 var calculator = new Calculator();
	  
 calculator.Enter(zero);
 calculator.Enter(zero);      
 calculator.Enter(zero);
	  
 Assert.Equal(
  StringConsistingOf(DigitKeys.Zero), 
  calculator.Display()
 );
}
```

**Johnny:** Good, you are learning fast! Let's evaluate this Statement.

**Benjamin:** It seems that our current code already fulfills the Statement. Should I try to comment some code to make sure this Statement can fail just like you did in the previous Statement?

**Johnny:** That would be wise thing to do. When a Statement is true without requiring you to change any production code, it is always suspicious. Just like you said, we have to change production code for a second to force this Statement to be false, then undo this modification to make it true again. This isn’t as obvious as previously, so let me do it. I will mark all the added lines with `//+` comment so that you can see them easily:

```csharp
public class Calculator
{
 int _result = 0;
 string _fakeResult = "0"; //+
	  
 public void Enter(DigitKeys digit)
 {
  _result *= 10;
  _result += (int)digit; 
  if(digit == DigitKeys.Zero) //+
  {  //+
   _fakeResult += "0";  //+
  } //+
 }

 public string Display()
 {
  if(_result == 0)  //+
  {  //+
   return _fakeResult;  //+
  }  //+
  return _result.ToString();
 }
}
```

**Benjamin:** Wow, looks like a lot of code just to make the Statement false! Is it worth the hassle? We will undo this whole change in a second anyway...

**Johnny:** Depends on how confident you want to feel. I would say that it is usually worth it -- at least you know that you got everything right. It might seem like a lot of work, but it only took me about a minute to add this code and imagine you got it wrong and had to debug it on a production environment. Now *that* would be a waste of time.

**Benjamin:** Ok, I think I get it. Since we saw this Statement turn false, I will undo this change to make it true again.

**Johnny:** Sure.

Epilogue
--------

Time to leave Johnny and Benjamin, at least for now. I actually planned to make this chapter longer, and cover all the other operations, but I fear I would make this too long and bore you. You should have a feel of how the TDD cycle looks like, especially since Johnny and Benjamin had a lot of conversations on many other topics in the meantime. I will be revisiting these topics later in the book. For now, if you felt lost or unconvinced on any of the topics mentioned by Johnny, do not worry -- I do not expect you to be proficient with any of the techniques shown in this chapter just yet. The time will come for that.
