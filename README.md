# Conditionals

## If and Else

Now that we know how to convert values into booleans using conditionals, what can we do with that "true" or "false" state? We can use `conditionals` to check for a boolean value and go down one branch or another, depending on the value.

Let's say we have a function that checks if a number is even or odd. First, we need to check for an even or odd value and return that as a boolean, but then we need one branch of code for a true value, and another branch of code for a false value.

```py
def even_or_odd(number):
    if number % 2 == 0:
        return f"{number} is even"
    else:
        return f"{number} is odd"
        
print(even_or_odd(4)) # 4 is even

print(even_or_odd(1)) # 1 is odd
```

The modulo operator (%) works as division, but instead of converting any remainder to a decimal, it returns the smallest whole number remainder possible. For even numbers, dividing by two should always have no remainder, so `number % 2` will be converted to True or even numbers and False for odd numbers. Now that there's a boolean to check, we set up and `if` block, which will execute for any true values. If the value is not true, it skips the 'if' block entirely and goes to the `else` block. There is no check to execute the 'else' block, as you only reach it if the other blocks are bypassed. Think of 'else' as a way to catch all outcomes outside of the one you're specfically targetting. In fact, you can even have an implicit 'else' by returning your functions early and not including an else block at all:

```py
def even_or_odd(number):
    if number % 2 == 0:
        return f"{number} is even"
        
    return f"{number} is odd"
```

This function is identical to the first version, as it will end early if 'number' is converted to True, but return the default response if number is converted to False.

## Elif

Sometimes you want to check for more than one condition, such as with the FizzBuzz game. For FizzBuzz, the goal is to count from one to as high as you can go, but along the way, if a number is divisible by three, say "fizz" instead of the number, if it's divisible by five, say "buzz" instead of the number, and if it's divisible by both three and five, say "fizzbuzz". You could set up several if blocks within your function:

```py
def fizz_buzz(number):
    if number % 3 == 0:
        value = "fizz"
        
    if number % 5 == 0:
        value = "buzz"
        
    if number % 3 == 0 and number % 5 == 0:
        value = "fizzbuzz"
        
    if not number % 3 == 0 and not number % 5 == 0:
        value = number
    
    return value
```

If you pass 6 into fizz_buzz, it will check if six is divisible by 3, which is is, declaring `value` as "fizz". Then if checks if six is divisible by 5, which it isn't, so it moves on. Next, it checks if six is divisible by three and five, which again, it isn't, and finally, if checks if it's *not* divisible by three or five. This works perfectly fine, but it's extremely inefficient. Every number you pass through has to run through four conditional blocks, potentially changing the output value several times. For example, 30 would be converted into "fizz" in the first block, then converted to "buzz" in the second block, then converted yet again to "fizzbuzz" in the third block. In addition, 6 from the first example was already done in the first block, and all it did was waste time running three more checks just to remain "fizz" for the entire function. We can dramatically improve this performance by adding a new block to our conditionals, `elif`.

```py
def fizz_buzz(number):
    if number % 3 == 0 and number % 5 == 0:
        value = "fizzbuzz"
    elif number % 3 == 0:
        value = "fizz"
    elif number % 5 == 0:
        value = "buzz"
    else:
        value = number
        
    return value
```

Elif, short for "else if", allows you to connect all your conditional blocks into a single conditional statement. Meaning, once one of the conditions are met, the entire statement is finished. So for our previous examples, plugging in 6 would fail the first check, but pass the first elif block, and once it does, the second elif and the else block are completely ignored. For 30, it passes the first if block, and as a result, the function is immediately finished.

## Conditional Order

Note the different orders of the blocks in both of the functions, as well as the complete lack of an else block in the first. Depending on the process you want to use, you need to be careful with the order your check for your values.

For the first version, we have the "fizz" and "buzz" checks before the "fizzbuzz" check, because we can accidentally override the correct response with the wrong one if we don't.

```py
def fizz_buzz(number):
    if number % 3 == 0 and number % 5 == 0:
        value = "fizzbuzz"
        
    if number % 3 == 0:
        value = "fizz"
        
    if number % 5 == 0:
        value = "buzz"
        
    if not number % 3 == 0 and not number % 5 == 0:
        value = number
    
    return value
```

Now if we pass in, say, 15, it will pass the first check and become "fizzbuzz". But it also passes the check for "fizz", so it rewrites value to "fizz". Yet again, it passes the check for "buzz", becoming "buzz", and that's ultimately what it returns. Even though 15 should be converted to fizzbuzz, and it was initially, because we put the checks in the wrong order, we got the wrong response.

We also can't use an implicit else like we did with our second even_or_odd function, because that would just override all of the checks completely:

```py
def fizz_buzz(number):
    if number % 3 == 0:
        value = "fizz"
        
    if number % 5 == 0:
        value = "buzz"
        
    if number % 3 == 0 and number % 5 == 0:
        value = "fizzbuzz"
        
    value = number
    
    return value
```

No matter what checks are passed or failed, this will end every function call by declaring value as the input number, completely removing any actual functionality.

For the second version, once one check is passed, no other checks are executed, and as a result, we need to more specific checks at the beginning of the function rather than the end.

```py
def fizz_buzz(number):
    if number % 3 == 0:
        value = "fizz"
    elif number % 5 == 0:
        value = "buzz"
    elif number % 3 == 0 and number % 5 == 0:
        value = "fizzbuzz"
    else:
        value = number
        
    return value
```

If we pass 15 into this version of the function, it will pass the "fizz" check, but immediately exit the coniditional blocks instead of continuing until it gets to "fizzbuzz", also returning the wrong answer.

To maximize efficiency, you can include early `return` calls, like we did for even_or_odd. This bypasses the need to make multiple checks by ending the function as soon as the correct condition is met. Just be sure that the return call is properly scoped, or the function won't work properly:

```py
def check_conditions(value):
    if condition_one:
        value = resp_1
        
    return value
    
    if condition_two:
        value = resp_2
        
    return value

    value = resp_3
    
    return value
    
# Wrong, the "return" calls are outside the conditional's scope. If condition_one is true, it will return resp_1, but if it isn't, it will just return "value" instead of checking for resp_2

def check_conditions(value):
    if condition_one:
        return resp_1
    else if condition_two:
        return resp_2
    
    return resp_3
    
# Correct, each return call is scoped inside the conditional (note the indentation), save for the last implicit else block. This means that if condition_one is met, the function immediately ends, directly returning resp_1. If not, it checks for condition_two, directly returning resp_2, and finally, returns resp_3 if both conditions fail.
```

In the context of fizz_buzz, The condition that a number will not be divisible by three or five is actually the most common scenario, so we can make our function more efficient by placing that at the beginning of the function, and by including return calls in our conditional blocks.

```py
def fizz_buzz(number):
    if number % 3 != 0 and number % 5 != 0:
        return number
        
    if number % 3 == 0 and number % 5 == 0:
        return "fizzbuzz"
    elif number % 3 == 0:
        return "fizz"
    
    return "buzz"
```

If the number is divisible by neither, there's no need to do any more checks, so the number is returned as is. If the number is divisible by both, "fizzbuzz". Note that there's no explicit check for "buzz", as we've already implicitly checked for this. By failing the first conditional, we know that the number is divisible by either three or five. By failing the second check, we know it's divisible by one of them, but not both. And by failing the third conditional, we know the number it is divisible by isn't three. Understanding what your function is trying to do, as well as how conditionals work, can help you customize your conditional blocks to get the best possible performances.
