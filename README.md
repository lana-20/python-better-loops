# Write Better For Loops in Python

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

If I do it manually, the I have to say _for i in range(len(data)-1)_. I use _-1_ on length, because I then access _data[1]_ and _data[i+1]_.

I can refactor it with _pairwise_ by saying _for pair in pairwise_, which gives me a tuple. So I can access index[0] and index[1]. It gives back the same result, but it's more optimized.

    # 5) Use itertools

    data = 'ABCDEFG'
    for i in range(len(data)-1):
        print(data[i], data[i+1])

    print()

    from itertools import pairwise
    for pair in pairwise('ABCDEFG'):
        print(pair[0], pair[1])

The 3rd function is the _takewhile_ function.












