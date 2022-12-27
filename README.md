# Write Better For Loops in Python

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














