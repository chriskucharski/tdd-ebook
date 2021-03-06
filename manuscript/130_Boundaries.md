Specifying Boundaries and Conditions
====================================

### A Disclaimer

Before I begin, I have to admit that this chapter is mostly based on the material from a series of posts by Scot Bain and Amir Kolsky from the blog Sustainable Test-Driven Development and their upcoming book by the same title. I like their idea of boundaries so much that I just follow the guidelines they outlined. This chapter is going to be a rephrase of these guidelines. I placed it here so that you have all the important topics covered in one place, but I encourage you to read the original blog posts on this subject on http://www.sustainabletdd.com/ (and the upcoming book).

Sometimes, anonymous value is not enough
----------------------------------------

When we specify a behavior, there are times when this behavior should be the same no matter what arguments we pass to the constructor or invoked methods. An example would be an addition of two numbers -- whatever numbers we would supply, the answer would always be a sum of those numbers:

```csharp
[Fact] public void
ShouldCalculateTheSumOfTwoNumbers()
{
  //GIVEN
  var a = Any.Integer();
  var b = Any,Integer();
  
  //WHEN
  var result = new Sum().Of(a, b);
  
  //THEN
  Assert.Equal(a + b, result);
}
```

In this case, the integer numbers can really be "any" -- the described relationship between input and output is independent of the actual values we use. As indicated in one of the previous chapters, this is the canonical case where Constrained Non-Determinism applies.

Sometimes, however, objects exhibit different behaviors based on what is passed to their constructor and to their methods (OK, we also have static methods and singletons. Longer discussion on those will be included in one of the next chapters -- for now we can safely ignore them). For example, in our application we may have a licensing policy where a feature is allowed to use only if the license is valid, and denied after it has expired. Another example would be that some shops are open from 10:00 to 18:00, so if we had a query in our application whether the shop is currently open, we would expect it to be answered differently based on what the current time is.

In such cases, Scott and Amir offer us other guidelines for choosing input values.

Exceptions to the rule
----------------------

There are times, when a Statement is true for every value except one (or more) explicitly specified. For example, in Poland, high school students are graded for exams between "mediocre" and "very good". Every grade means the exam is passed except for "mediocre" grade which means the exam is failed. If we imagine we have to specify an object that decides whether the exam is passed based on rating, we would have to write two Statements: one for the mediocre rating and other for all the others.

Here is the Statement for mediocre grade (let's imagine that all grades are members of an enum):

```csharp
[Fact] public void
ShouldNotPassTheExamWhenGradeIsMediocre()
{
  //GIVEN
  var decision = new PassOrNotDecision();

  //WHEN
  var examPassed = decision.MakeBasedOn(Grades.Mediocre);

  //THEN
  Assert.False(examPassed);
}
```

Note that here, we used the literal value of `Grades.Mediocre`. This is because no other value gives the same behavior as this one, so generating the value would not make any sense.

The second Statement is for all the other cases. Here, we are going to use another method of the `Any` class for generating any enum member other than specified:

```csharp
[Fact] public void
ShouldPassTheExamWhenGradeIsOtherThanMediocre()
{
  //GIVEN
  var decision = new PassOrNotDecision();

  //WHEN
  var examPassed 
    = decision.MakeBasedOn(Any.Besides(Grades.Mediocre));
  
  //THEN
  Assert.True(examPassed);
}
```

Here, `Any.Besides()` takes care of the enum value generation, producing a nice, readable code as a side effect.

The example shown above assumes there is only one exception to the rule (the mediocre grade). However, this concept can be scaled up to more values, as long as it is a finished, discrete set. If there are multiple exceptional values that produce the same behavior, a single Statement is sufficient to cover them all (using `Any` class and making the following call for exception Statement: `Any.Of(value1, value2)` and the following for the rest: `Any.OtherThan(value1, value2)`). However, when there are multiple exceptions to the rule and each one triggers a different behavior, each one deserves its own Statement.

Rules valid within boundaries
-----------------------------

Sometimes, a behavior varies around a numerical boundary. The simplest example would be a set of rules on how to calculate an absolute value of a number:

1.  for any X less than 0, the result is -X (e.g. absolute value of -1.5 is 1.5)
2.  for any X greater or equal to 0, the result is X (e.g. absolute value of 3 is 3).

As you can see, there is a boundary between the two behaviors and the right edge of the boundary is 0. Why do I say "right edge"? That is because the boundary always has two edges and there’s a length between them. If we assume we are talking about the mentioned absolute value calculation and that our numerical domain is that of integer numbers, we can as well use -1 as edge value and say that:

1.  for any X less or equal to -1, the result is -X (e.g. absolute value of -1.5 is 1.5)
2.  for any X greater than -1, the result is X (e.g. absolute value of 3 is 3).

So a boundary is not a single number -- it always has a length -- the length between last value of the previous behavior and the first value of the next behavior. In case of our example, the length between -1 (left edge -- last negated number) and 0 (right edge -- first non-negated) is 1.

Now, imagine that we are not talking about integer values anymore, but about floating point values. Then the right edge value would still be 0. But what about left edge? It would not be possible for it to stay -1, because the rule applies to e.g. -0.9 as well. So what is the correct right edge value and the correct length of the boundary? Would the length be 0.1? Or 0.001? Or maybe 0.00000001? This is harder to answer and depends heavily on the context, but it is something that must be answered for each particular case -- this way we know what kind of precision is expected of us. In our Specification, we have to document the boundary length somehow.

So the next topic is: how to describe the boundary length with Statements? To illustrate this, I want to show you two Statements assuming we’re implementing the mentioned absolute value calculation for integers. The first Statement is for values smaller than 0 and we want to use the left edge value here like this:

```csharp
[Fact] public void
ShouldNegateTheNumberWhenItIsLessThan0()
{
  //GIVEN
  var function = new AbsoluteValueCalculation();
  var lessThan0 = 0 - 1;

  //WHEN
  var result = function.PerformFor(lessThan0);

  //THEN
  Assert.Equal(-lessThan0, result);
}
```

And the next Statement for values at least 0 and we want to use the right edge value:

```csharp
[Fact] public void
ShouldNotNegateTheNumberWhenItIsGreaterOrEqualTo0()
{
  //GIVEN
  var function = new AbsoluteValueCalculation();
  var moreOrEqualTo0 = 0;

  //WHEN
  var result = function.PerformFor(moreOrEqualTo0);

  //THEN
  Assert.Equal(moreOrEqualTo0, result);
}
```

There are two things to note about these examples. The first one is that we don’t use any kind of `Any` methods. We explicitly take the edges, because they’re the numbers that most strictly define the boundary. This way we document the boundary length.

It is important to understand why we are not using methods like `Any.IntegerGreaterOrEqualTo(0)`, even though we do use `Any` in case when we have no boundary. This is because in the latter case, no value is better than the other in any particular way. In case of boundaries, however, the edge values are better in that they more strictly define the boundary and drive the right implementation.

The second thing to note is the usage of literal constant 0 in the above example. In one of the previous chapter, I showed you a technique called **Constant Specification**, where we write an explicit Statement about the value of the named constant and use the named constant everywhere else instead of its literal. So why did i not use this technique?

The only reason is that this might have looked a little bit silly with such extremely trivial example as calculating absolute value. In reality, I should have used the named constant in both Statements and it would show the boundary length even more clearly. Let's perform this exercise now and see what happens.

First, let's document the named constant with the following Statement:

```csharp
[Fact] public void
ShouldIncludeSmallestValueNotToNegateSetToZero()
{
  Assert.Equal(0, Constants.SmallestValueNotToNegate);
}
```

Now we have got everything we need to rewrite the two Statements we wrote earlier. The first would look like this:

```csharp
[Fact] public void
ShouldNegateTheNumberWhenItIsLessThanSmallestValueNotToNegate()
{
  //GIVEN
  var function = new AbsoluteValueCalculation();
  var lastNumberToNegate 
    = Constants.SmallestValueNotToNegate - 1;

  //WHEN
  var result = function.PerformFor(lastNumberToNegate);

  //THEN
  Assert.Equal(-lastNumberToNegate, result);
}
```

And the second Statement for values at least 0:

```csharp
[Fact] public void
ShouldNotNegateNumberWhenItIsGreaterOrEqualToSmallestValueNotToNegate()
{
  //GIVEN
  var function = new AbsoluteValueCalculation();

  //WHEN
  var result = function.PerformFor(
    Constants.SmallestValueNotToNegate
  );

  //THEN
  Assert.Equal(
    Constants.SmallestValueNotToNegate, result);
}
```

As you can see, the first Statement contains the following expression: `Constants.SmallestValueNotToNegate - 1`, where 1 is the exact length of the boundary. Like I said, the situation is so trivial that it may look silly and funny, however, in real life scenarios, this is a great technique to apply anytime, anywhere.

Boundaries may look like they apply only to integer input, but they occur at many other places. There are boundaries associated with date/time (e.g. an action is performed again when time from last action is at least 30 seconds -- the decision would need to be made whether we need precision in seconds or maybe in ticks), to strings (e.g. validation of user name where it must be at least 2 characters, or password that must contain at least 2 special characters) etc.

Combination of boundaries -- ranges
----------------------------------

So, what about a behavior that is valid in a range? Let's assume that we live in a country where a citizen can get a driving license only after their 17th birthday, but before 65th (the government decided that people after 65 have worse sight and it’s safer not to give them new driving licenses). Let's also assume that we try to develop a class that answers the question whether we can apply for driving license and the return values of this query are as follows:

1.  Age \< 17 -- returns enum value `QueryResults.TooYoung`
2.  17 \<= age \>= 65 -- returns enum value `QueryResults.AllowedToApply`
3.  Age \> 65 -- returns enum value `QueryResults.TooOld`

Now, remember I told you that we specify the behaviors near boundaries? This, however, when applied to the situation I just described, would give us the following Statements:

1.  Age = 17, should yield result `QueryResults.TooYoung`
2.  Age = 18, should yield result `QueryResults.AllowedToApply`
3.  Age = 65, should yield result `QueryResults.AllowedToApply`
4.  Age = 66, should yield result `QueryResults.TooOld`

thus, we would describe the behavior where the query should return `AllowedToApply` value twice, which effectively means that we would copy-paste the Statement and change just one value. How do we deal with this? Thankfully, we have a solution available:

Most xUnit frameworks provide some kind of data-driven test functionality, which we can use to write parameterized Statements. The functionality basically means that we can write the code of the Statement once, but make the xUnit framework invoke it twice with different sets of input values. Let’s see an example in XUnit.net:

```csharp
[Theory]
[InlineData(17, QueryResults.TooYoung)]
[InlineData(18, QueryResults.AllowedToApply)]
[InlineData(65, QueryResults.AllowedToApply)]
[InlineData(66, QueryResults.TooOld)]
public void ShouldYieldResultForAge(int age, QueryResults expectedResult)
{
  //GIVEN
  var query = new DrivingLicenseQuery();

  //WHEN
  var result = query.ExecuteFor(age);

  //THEN
  Assert.Equal(expectedResult, result);
}
```

This way, there is only one Statement executed four times. The case of `AllowedToApply` is still evaluated twice for both edge cases (so there is more time spent on executing it, which for small cases is not an issue), but the code maintenance issue is gone -- we don’t have to copy-paste the code to specify both edges of the behavior.

Note that we’re quite lucky because the specified logic is strictly functional (i.e. returns different results based on different inputs). Thanks to this, we could parameterize input values together with expected results. This is not always the case. For example, let's imagine that we have a clock class where we can set alarm time. The class allows us to set hour between 0 and 24, but otherwise throws an exception.

While some xUnit frameworks, like NUnit, allow us to handle both cases with one Statement by writing something like this:

```csharp
//NOTE: this is an example in NUnit framework!
[TestCase(Hours.Min, Result=Hours.Min)]
[TestCase(Hours.Max, Result=Hours.Max)]
[TestCase(Hours.Min-1, ExpectedException = typeof(OutOfRangeException))]
[TestCase(Hours.Max+1, ExpectedException = typeof(OutOfRangeException))]
public int 
ShouldBeAbleToSetHourBetweenMinAndMaxButNotOutsideThatRange(
  int inputHour)
{
  //GIVEN
  var clock = new Clock();
  clock.SetAlarmHour(inputHour);

  //WHEN
  var setHour = clock.GetAlarmHour();

  //THEN
  return setHour;
}
```

Others, like XUnit.NET, don’t (not that it’s a defect of the framework, it’s just that the philosophy behind those two is a little bit different and that some features have hidden price attached to their usage which some frameworks are willing to pay, while others aren’t). Thus, we have to solve it by writing two parameterized Statements -- one where a value is returned (for valid cases) and one where exception is thrown (for invalid cases). The first would look like this:

```csharp
[Theory]
[InlineData(Hours.Min)]
[InlineData(Hours.Max)]
public void 
ShouldBeAbleToSetHourBetweenMinAndMax(int inputHour)
{
  //GIVEN
  var clock = new Clock();
  clock.SetAlarmHour(inputHour);

  //WHEN
  var setHour = clock.GetAlarmHour();

  //THEN
  Assert.Equal(inputHour, setHour);
}
```

and the second:

```csharp
[Theory]
[InlineData(Hours.Min-1)]
[InlineData(Hours.Max+1)]
public void 
ShouldThrowOutOfRangeExceptionWhenTryingToSetAlarmHourOutsideValidRange(
  int inputHour)
{
  //GIVEN
  var clock = new Clock();

  //WHEN - THEN
  Assert.Throws<OutOfRangeException>( 
    ()=> clock.SetAlarmHour(inputHour)
  );
}
```

Summary
-------

In this chapter, we covered specifying numerical boundaries with a minimal amount of code, so that the Specification is more maintainable and runs fast. There is one more kind of situation left: when we have compound conditions (e.g. a password must be at least 10 characters and contain at least 2 special characters) -- we’ll get back to those when we introduce mocks.
