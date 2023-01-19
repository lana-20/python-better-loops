# ✏🔁🐍 Write Better For Loops in Python

    # Bathe the Pets Function

    for pet in ['🐕', '🐩', '🐈', '🐇']:
         print("%s is bathed" %pet, "🛁")

Do you have any ideas what this code might do? 
First of all, we have some square brackets with a bunch of strings inside it, separated by commas. These square brackets are one way of creating something called a Python <code>list</code>.
A list is just a collection of different values. And not just a collection, but an _ordered_ collection. Lists are extremely useful and we'll see them a lot. For now, what we're doing with this list is just grouping together the 4 pet emojis, in order.
Take a look at the <code>for</code> keyword at the beginning of the line. This is a special keyword that tells Python we want to start a loop, meaning we want to execute the same block of code potentially multiple times, based on some kind of input. In our case, we want to loop over the list of emojis.
Essentially, the loop will run for as many times as there are emojis, and each time through the loop, we'll get out a different emoji.  We'll be able to use that emoji with any name we specify; in our case, we just call it <code>pet</code>. The code block within the loop takes the emoji which is currently being provided, and prints it out.

<img width="490" alt="Screenshot 2023-01-18 at 7 21 40 PM" src="https://user-images.githubusercontent.com/70295997/213349787-716950e3-6a08-42cb-b753-9dd8e15b7ede.png">


There are several more examples of control flow in Python, and indeed several other kinds of loops. There are also several different ways to write better <code>for</code> loops.


### 1) Don't use loops at all
    # 1) Don't use loops at all

    numbers = [10, 20, 33, 40, 50, 60, 70, 80]
    result = 0
    for num in numbers:
      result += num

    print(result)

Output:

    323

Loops are typically very slow, avoid them when possible. Always looks for the built-in methods instead. the easiest example is when I want to calculate the sum of a list. 

I avoid the for loop and instead use the built-in _sum()_ method. It yields the same result in one line.

    result = sum(numbers)
    print(result)

The built-in methods are always optimized. This is the better choice than a raw for loop.

### 2) Use enumerate
    # 2) Use enumerate

    numbers = [10, 20, 33, 40]
    for idx in range(len(numbers)):
      print(idx, numbers[idx])

Output:

    0 10
    1 20
    2 33
    3 40

Use enumerate when I need both the index and the value. the above code works, but it's very error prone. A better way is to use _enumarate()_ and pass the numbers to it. It returns the index and the value as a tuple, so I can immediately unpack them and do what I need with them, eg, print.

    for idx, val in enumerate(numbers):
      print(idx, val)
  
I can optionally start at a different number with _start=n_. Then the counting should start at that value.

    for idx, val in enumerate(numbers, start=1):
        print(idx, val)

Output:

    1 10
    2 20
    3 33
    4 40

Use _enumerate()_ instead of _range(len())_.

### 3) Use zip
    # 3) Use zip

    a = [1, 2, 3]
    b = ["a", "b", "c"]

    for idx in range(len(a)):
        print(a[idx], b[])idx)

Output:

    1 a
    2 b
    3 c

Use the _zip()_ function to loop over multiple lists or iterables at once. 

The code above works, but again it is error prone. If one list is longer it raises the index out of range error. 

    a = [1, 2, 3, 4]
    b = ["a", "b", "c"]

    for idx in range(len(a)):
        print(a[idx], b[])idx)

Output:

    IndexError: List index out of range

A better way is to use the built-in _zip()_ function to zip a and b. _val1_ and _val2_ give me the value of the 1st list and the value of the 2nd list at once. The below code automatically stops at the the shorter of those lists.

    for val1, val2 in zip(a, b):
        print(val1, val2)

Output:

    1 a
    2 b
    3 c

Sometimes it's useful to have the error so that I can catch bugs in the app. In order to see the error, in this case I can use the argument _strict=True_. It's new since Python 3.10. This raises an error if one of those lists is longer.

    for val1, val2 in zip(a, b, strict=True):
        print(val1, val2)

Output:

    ValueError: zip() argument 2 is shorter than argument 1


### 4) Think lazy - use a generator
    # 4) Think lazy - use a generator
    events = [("learn", 5), ("learn", 10), ("relaxed", 20)]
    minutes_studied = 0
    for event in events:
        if event[0] == "learn":
            minutes_studied +=event[1]
    print(minutes_studied)

Output:

    15

This builds on top of approach # 1, where I want to avoid for loops and replace them with built-in methods. And now additionally I'm looking out for the generators I can use.

In this example I calculate how many minutes I studied and loop over different _events_, which contains tuples. 
_event[0]_ is the 1st item in a tuple, eg, _"learn"_, _event[1]_ is the 2nd, eg, _5_.

Remenber, again, if I want to calculate the sum, I can use the built-in _sum()_ method. And now I can additionally use a generator. I refactor the code, using a generator expression, which looks like a list comprehension but with parenthesis. Then I pass the generator expression into the _sum()_ function.

    study_times = (event[1] for event in events if event[0] == "learn")
    minutes_studied = sum(study_times)
    print(minutes_studied)

It prints the same output as before, but now this generator is evaluated lazily. So at the moment when I declare the _study_times_ variable, nothing happens. When I put into the _sum()_ function, the calculations begin. That's why this code is called 'lazy'. So the generator is an alternative approach that might optimize your code.

Be cautious when doing it the 2nd time. When printing the sum again, it's now zero. When using a generator, the objects get comsumed.

    study_times = (event[1] for event in events if event[0] == "learn")
    minutes_studied = sum(study_times)
    print(minutes_studied)

    study_times = (event[1] for event in events if event[0] == "learn")
    minutes_studied = sum(study_times)
    print(minutes_studied)

Output:

    0

Check this [talk](https://youtu.be/Wd7vcuiMhxU) on YouTube about a deeper look at iteration in Python.


### 5) Use itertools

Use the [itertools](docs.python.org/3/library/itertools.html) module more. It provides functions with iterators for efficient looping. There are a lot of functions. I use 3 examples that can be used to refactor the code.

The 1st one is the _islice_ function. Here, I want to iterate over lines. And if I reach line 5, then I break. I can refactor the for loop with the _islice_ method.

    # 5) Use itertools
    lines = ["line1", "line2", "line3", "line4", "line5", "line6", "line7", "line8", "line9", "line10"]

    for i, line in enumerate(lines):
        if i >= 5:
            break
        print(line)

    from itertools import islice

    first_five_lines = islice(lines, 5)
    for line in first_five_lines:
        print(line)

Output:

    line1
    line2
    line3
    line4
    line5

    line1
    line2
    line3
    line4
    line5

This makes the code more readable.

Here, again, be cautious if iterating a 2nd time. The output will be empty because it got consumed.

I can use the start and the stop index.

    for i, line in enumerate(lines):
        if i >= 5:
            break
        print(line)

    print()

    from itertools import islice

    first_five_lines = islice(lines, 1, 5)
    for line in first_five_lines:
        print(line)

Output:

    line1
    line2
    line3
    line4
    line5

    line2
    line3
    line4
    line5

The output starts at index 1.

I can also use a _step_ argument, eg, a step size of 2.

    first_five_lines = islice(lines, 1, 5, 2)
    for line in first_five_lines:
        print(line)

Output:

    line2
    line4

This is a good approach when I just need to iterate over slices.

The 2nd function is the _pairwise_ function. Per [documentation](https://docs.python.org/3/library/itertools.html#itertools.pairwise), _pairwise_ returns successive overlapping pairs taken from the input iterable.

If I do it manually, then I have to say _for i in range(len(data)-1)_. I use _-1_ on length, because I then access _data[1]_ and _data[i+1]_.

I can refactor it with _pairwise_ by saying _for pair in pairwise_, which gives me a tuple. So I can access index[0] and index[1]. It gives back the same result, but it's more optimized.

    # 5) Use itertools

    data = 'ABCDEFG'
    for i in range(len(data)-1):
        print(data[i], data[i+1])

    print()

    from itertools import pairwise
    for pair in pairwise('ABCDEFG'):
        print(pair[0], pair[1])

Output:

    A B
    B C
    C D
    D E
    E F
    F G

    A B
    B C
    C D
    D E
    E F
    F G

The 3rd function is the _takewhile_ function.

Often I want to iterate over an iterable. And, if I reach a certain condition, eg, if _i_ is negative, then I stop and break. I can easily refactor this with _takewhile_. It takes a predicate as the 1st param and an iterable as the 2nd one. This is a function where I can use _lambda_ in one line. It gives me the _items_ iterator. Now I can say _for item in items_ and get the same result.

    # 5) Use itertools
    for item in [1,2,4,-1,4,1]:
        if item >= 0:
            print(item)
        else:
            break

    print()

    from itertools import takewhile
    items = takewhile(lambda x: x => 0, [1,2,4,-1,4,1])
    for item in items:
        print(item)

Output:

    1
    2
    4

    1
    2
    4

### 6) Use numpy

If speed is really important, or when doing lots of matrix multiplications and math operations, use _numpy_.
_numpy_ provides methods for many linear algebra methods and matrix multiplications in a vectorized way.

Instead of calculating the dot-product with a for loop, I want to avoid the loop and simply call _numpy.dot()_. It gives me the same result and it's much faster.

    # 6) Use numpy
    import numpy as np

    vec_a = [1, 2, 3]
    vec_b = [4, 5, 6]

    result = 0
    for val1, val2 in zip(vec_a, vec_b):
        result += val1 * val2
    print(result)

    result = np.dot(vec_a, vec_b)
    print(result)


Output: 

    32

    32

_numpy_ also has many functions that I can find in Python, eg, the built-in _sum()_ function. 
The _numpy_ equivalent is _numpy.sum()_. The analog of the Python _range()_ function in _numpy_ is _numpy.arrange()_. It return the same result.

    N = 100_000_000
    def sum_python():
        return sum(range(N))

    def sum_numpy():
        return np.sum(np.arrange(N))

    print(sum_python())
    print(sum_numpy())

Output:

    4999999950000000

    4999999950000000

Now, let's time both versions. The _numpy_ version is much faster.

    import timeit
    print("sum python:", timeit.timeit(sum_python, number=1))
    print("sum numpy:", timeit.timeit(sum_numpy, number=1))

Output:

    sum python: 0.8761997090005025

    sum numpy: 0.16527225000027101

The one thing to be aware of is that in the case of _numpy_ I copy the whole array _np.arrange(N)_ into memory. So, the _sum_numpy()_ method requires more memory, but it's much faster.





















