---
title: "Creating object in the base class?"
date: 2020-06-20
read_time: true
categories:
  - post
tags:
  - design pattern
  - c#
---

I always got confused about why creating an object in the base class. Recently, I finally figured out why.

Let's first take a took at an example. There is a base class called `BaseCustomer`, with fields `Mobile` and `Name`, and methods `AddCustomer()` and `DisplayInfo()`. `PrimeCustomer` inherits from `BaseCusomer` with extra fields `ValidTo` and overrides the methods.

When creating a prime customer instance using `BaseCustomer c = new PrimeCustomer()`.

#### Question 1. We create an instance by executing which one's constructor❓

🗝 The constructor of `BaseCustomer` will be first executed and then followed by `PrimeCustomer`'s custructor.

#### Question 2. Which method will be called when executing `c.AddCustomer()`❓

🗝 Since the method is overriden in class `PrimeCustomer`, then the `AddCustomer()` in `PrimeCustomer` class will be called. In other words, the most derived version will be called.

#### Question 3. Then what about other methods in `PrimeCustomer`❓

🗝 Be aware of `c` is an object of `BaseCustomer` not `PrimeCustomer`. Right? Then `c` will not have access to them.

#### Question 4. If two methods implemented in two classes have same signature, how to call the one in the `BaseCustomer`❓

🗝 In this case, we could hide the method, _method hiding_. For example, in c# modifying the method using `new` keyword in `PrimeCustomer`. Call `c.Welcome()`, you will get `Welcome from base customer class...`.

```c#
class BaseCustomer
{
    public void Welcome()
    {
        Console.WriteLine("Welcome from base customer class...");
    }
}

class PrimeCustomer : BaseCustomer
{
    public new void Welcome()
    {
        Console.WriteLIne("Welcome from prime customer class....");
    }
}
```

### Why we use this method instead of creating the object in the derived class❓

Let's extend the previous example, adding new customers for the website, `Visitor` and `Customer`.

```c#
BaseCustomer v = new Visitor();
BaseCustomer c = new Customer();
BaseCustomer p = new PrimeCustomer();
```

It comes a requirement: write a function to generate object based upon the customer type, regular customer, prime customer and visitor.

```c#
public Visitor GenerateObject() {}
public Customer GenerateObject() {}
public PrimeCustomer GenerateObject() {}
```

Then there is another requirement: write a function to calculate the special discount given the object.

```c#
public double SpecialDiscount(Visitor visitor) {}
public double SpecialDiscount(Customer regular) {}
public double SpecialDiscount(PrimeCustomer prime) {}
```

In this scenerio, I come up two 👎🏻:

1. It's a pain to duplicate the code instead of _reusing_ the logic.
2. Things are not _loosely couple_. What I mean is, at first the website only have customer. Then it adds visitor, you need to add support for visitors because currently the code is couple with type customer.

👍🏻 How to optimize it?

```c#
// Be aware of the type is hard-coded, but it's not a good practice.
public BaseCustomer GenerateObject(int type)
{
    switch(type)
    {
        case 1:
            return new Customer();
        case 2:
            return new PrimeCustomer();
        case 3:
            return new Visitor();
    }

    return null;
}
```

In this way, using a method to create objects in the base class. This is how I understand **factory design pattern**, a creational desing pattern. If a service is required for the customers later, there is no need to support every type of derived classes.

### Reference Code

The code below is written 💻 in `c#`.

```c#
public class BaseCustomer
    {
        public String Mobile { get; set; }
            public String Name { get; set; }

        public BaseCustomer()
        {
            Console.WriteLine("Constructor of BaseCustomer...");
        }

        public void PrintWelcome()
        {
            Console.WriteLine("Hello from base customer!");
        }

        public virtual void AddCustomer()
        {
            Console.WriteLine("Please enter your name:");
            Name = Console.ReadLine();
            Console.WriteLine("Please enter your mobile");
            Mobile = Console.ReadLine();
        }

        public virtual void DisplayInfo()
        {
            Console.WriteLine("****Info about the Base Account****");
            Console.WriteLine("Name is: {0}", Name);
            Console.WriteLine("Mobile is: {0}", Mobile);
        }
    }

public class PrimeCustomer : BaseCustomer
    {
        public DateTime ValidTo { get; set; }

        public PrimeCustomer()
        {
            Console.WriteLine("Constructor of Prime Customer...");
        }

        public override void AddCustomer()
        {
            Console.WriteLine("Executing AddCustomer from PrimeCustomer class....");
            base.AddCustomer();
            ValidTo = DateTime.Now.AddDays(365);
        }

        public new void PrintWelcome()
        {
            Console.WriteLine("Hello from prime customer");
        }

        public override void DisplayInfo()
        {
            Console.WriteLine("****Info about the Prime Account****");
            Console.WriteLine("Name is: {0}", base.Name);
            Console.WriteLine("Mobile is: {0}", base.Mobile);
            Console.WriteLine("Valid to: {0}", ValidTo.Date);
        }
    }
```
