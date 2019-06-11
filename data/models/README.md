Performance, Performance, Performance. DataClass vs NamedTuple vs Object for performance optimization in Python. 

> Life is too short, you need Python. - Bruce Eckel

Since its introduction in Python 3.7, `data class` presents a exciting and new way of storing data.  Why is this exciting? It boils down to performance. 

I've been programming in an OOP paradigm from my first day as a software engineer, and I'm recently learning Python 3.7 for the purpose of Algo-Trading and Quant Research. Coming from a Application Development realm, one key distinction immediately becomes abundantly clear: When Coding for Trading, **optimization** is heavily valued, and coding for **performance** needs to become a second nature.

It is for this reason, majority of Financial Trading systems are written in C, C++, Python or Java. 

In this post, I am going to perform a performance tests to compare the different *size* and *speed* when **creating**, **reading** and executing functions for `Object`, `NamedTuple` and the new `DataClass` introduced in Python 3.7.  
 
 # A day in trading
 
 For the tests, I am going to create an objects representing a trade day and its prices.
 
 ## TradeDay
 
 The TradeDay object will have the following properties.
 
| Property 	| Type   	| Description                                     	|
|----------	|--------	|-------------------------------------------------	|
| symbol   	| str    	| The ticker symbol.                              	|
| date     	| date   	| A datetime.date object that describes the date. 	|
| prices   	| Prices 	| A Prices object.                                	|
| change   	| float  	| A float representing the change for the price.  	|

## Prices

The Prices object will have the following properties.

| Property 	| Type  	| Description        	|
|----------	|-------	|--------------------	|
| open     	| float 	| The opening price. 	|
| high     	| float 	| The highest price. 	|
| low      	| float 	| The lowest price.  	|
| close    	| float 	| The closing price. 	|

 To perform tests, I've created the TradeDay and Prices objects using different data types. 
 
 1. TradeDay & Prices object [written using Object](https://gist.github.com/jacktator/29de16f85b0371d820d5b464eb685e1f)
 2. TradeDay & Prices object [written using NamedTuple](https://gist.github.com/jacktator/8ccd3cee2f192d0c09c3e581529b64ee)
 3. TradeDay & Prices object [written using DataClass](https://gist.github.com/jacktator/46782d6aa62c65f22f7c61334e884abb)
 
 ## Creating tradeday object

To create a tradeday object, the following code is ran.

```python
import datetime

day: TradeDay = TradeDay("MA", 
                         datetime.date.today(), 
                         Prices(10.0, 
                                30.0, 
                                5.0, 
                                20.0))

print(day)
```
We're all ready, Let's run some tests!

# Performance Test

In order to unbiased perform tests, the following code is used to generate a Performance Report. 

```python
#%% Performance Test

import datetime
import sys

day: TradeDay = TradeDay("MA", datetime.date.today(), Prices(10.0, 30.0, 5.0, 20.0))

print("====== Performance Report ======\n")
print("This report shows the performance of storing and accessing a data object as a class.")
print(day)

print("\n------ Speed Report -----\n")
print("Time it takes to create 'day' object is: ")
%timeit day: TradeDay = TradeDay("MA", datetime.date.today(), Prices(10.0, 30.0, 5.0, 20.0))
print("Time it takes to read 'day' object is: ")
%timeit day
print("Time it takes to read 'day.prices' nested attribute is: ")
%timeit day.prices
print("Time it takes to execute 'day.change()' function is: ")
%timeit day.change()

print("\n------ Size Report ------\n")
print("The Size of 'day' object: {} bytes".format(sys.getsizeof(day)))
print("The Size of 'day' dict: {} bytes".format(sys.getsizeof(vars(day))))
print("\n======== End of Report =========")

#%%
```

This performance report will report the result of the following common actions:

1. Speed of
    1. Creating an object
    2. Reading an object
    3. Reading a property of an object
    4. Executing a function
2. Size of
    1. Class TradeDay object
    2. NamedTuple TradeDay object
    3. DateClass TradeDay object


Let's run it! 

## Object

Let's start with the traditional Object.

```python
====== Performance Report ======

This report shows the performance of storing and accessing a data object as a class(object).
TradeDay(symbol: MA, date: 2019-06-01, prices: Prices(open: 10.0, high: 30.0, low: 5.0, close: 20.0), change: <function TradeDay.__init__.<locals>.<lambda> at 0x10c9eb620>)

------ Speed Report -----

Time it takes to create 'day' object is: 
2.94 µs ± 41.4 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
Time it takes to read 'day' object is: 
24.7 ns ± 0.638 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
Time it takes to read 'day.prices' nested attribute is: 
48.1 ns ± 0.993 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
Time it takes to execute 'day.change()' function is: 
829 ns ± 7.51 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

------ Size Report ------

The Size of 'day' object: 56 bytes
The Size of 'day' dict: 112 bytes

======== End of Report =========
```

## NamedTuple

Before the release of 3.7, NamedTuple was a popular choice for storing data object. 

```python
====== Performance Report ======

This report shows the performance of storing and accessing a data object as a namedtuple.
TradeDay(symbol='MA', date=datetime.date(2019, 6, 1), prices=Prices(open=10.0, high=30.0, low=5.0, close=20.0))

------ Speed Report -----

Time it takes to create 'day' object is: 
2.01 µs ± 17.9 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
Time it takes to read 'day' object is: 
26.9 ns ± 0.307 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
Time it takes to read 'day.prices' dict attribute is: 
75.8 ns ± 0.749 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
Time it takes to execute 'day.change()' function is: 
946 ns ± 15 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

------ Size Report ------

The Size of 'day' object: 80 bytes

======== End of Report =========
```

## DataClass

Data classes are just regular classes that are geared towards storing state, more than contain a lot of logic. Every time you create a class that mostly consists of attributes you made a data class. 

```python
====== Performance Report ======

This report shows the performance of storing and accessing a data object as a dataclasses.
TradeDay(symbol='MA', date=datetime.date(2019, 6, 1), prices=Prices(open=10.0, high=30.0, low=5.0, close=20.0))

------ Speed Report -----

Time it takes to create 'day' object is: 
2.34 µs ± 19.3 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
Time it takes to read 'day' object is: 
24.7 ns ± 0.162 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
Time it takes to read 'day.prices' nested attribute is: 
52.1 ns ± 0.611 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
Time it takes to execute 'day.change()' function is: 
821 ns ± 12.3 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

------ Size Report ------

The Size of 'day' object: 56 bytes
The Size of 'day' dict: 112 bytes

======== End of Report =========
```

# Comparison

Thanks to the above reports, we can make some comparisons.

|                  	| Object  	| NamedTuple 	| DataClass 	|
|------------------	|---------	|------------	|-----------	|
| create           	| 2.94 µs 	| **2.01 µs**   | 2.34 µs   	|
| read property    	| **24.7 ns**| 26.9 ns      | **24.7 ns**   |
| nested property  	| **48.1 ns**| 75.8 ns    	| 52.1 ns   	|
| execute function 	| 829 ns  	| 946 ns     	| **821 ns**    |
| size 	            | **56 bytes**| 80 bytes   	| **56 bytes**  |

## DataClass vs Object

While DataClass is just regular Class, it is indeed geared towards storing state. 

- **DataClass** is faster at **creating** and **reading** data objects. 
- **Object** is faster at reading **nested properties** and **executing functions**.

## DataClass vs NamedTuple

NamedTuple is also DataClass, but is **immutable** by default (as well as being sequences).

- **DataClass** is faster at **reading** the object, **nested properties** and **executing functions**.
- **NamedTuple** is faster at only **creating** object.

## For Data-Only Objects

For the faster performance on **newer projects**, **DataClass** is **8.18% faster** to create objects than NamedTuple to create and store objects.

However, if working on *legacy* software with Python 2.*, *NameTuple* delivers the best performance in creating data objects, while *Object* is faster at reading properties.

## For Objects with Functions

Again, for **newer projects**, **DataClass** performs **13.21% faster than NamedTuple** and **0.97% faster then Object** to execute functions.

However, for legacy software, it is faster to use *Object* to execute functions.  

# Conclusion

While `DataClass` is all shiny and sexy, it does come with its pitfall: It's only available from Python 3.7 onwards. If external environment allows you to use Python 3.7, then by all means, use DataClass.

Alternatively, consider sticking with the`Object` or `NamedTuple`, depending on your data use case.



