---
title: "What I learnt today 18/01/23"
tags:

---
💡 This post introduces what I learnt today 18/01/23.
{: .notice--warning}
# Python Object-Oriented Programming
e.g. How can we create a course registration program?
- objects: professor, student, admin
- action: register course, input course
- data: the list of course registration, the list of courses
<br>
<br>
Objects: some kind of stuff in real life, which has `attribute` and `action`. In OOP, they are represented as `variable` and `method`, respectively.
<br>
<br>
Object consists of `class` and `instance`.
 `attribute` is added with `__init__` and `self`.
 `__init__` is the constructor of the class that initialise the object's state.
 ```
class SoccerPlayer(object):
    def __init__(self, name, position, back_number):
      self.name = name
      self.postion = position
      self.back_number = back_number
```
Adding method(action) is same as original function but `self` should be added to be `class function`.
Input the initial value with Object name declaration. `self` means the created instance itself.
```
choycoy = SoccerPlayer("choycoy", "MF", 10)
```
Above `choycoy` is the name of object, `SoccerPlayer` is the name of class, and `("choycoy", "MF", 10)` is the interface of __init__ function or initialisation
- OOP characteristics
<br>
(1) Inheritance: use the properties and methods from the parent class to create inherited child classes
<br>
<br>
(2) Polymorphism:
<br>
<br>
(3) Visibility
<br>
<br>
