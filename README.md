# SOLID Principles
## Nishaant Dixit, FA Flutter Team



SOLID Principles are the designing principles that can be applied when working with various Object Oriented Programming languages. They reduce the time needed to read the legacy code and let us delve into the development process faster.

In this article, we will see each principle with an analogy to an example for better understanding.

## Principles

- S:- Single Responsibilty Principle
- O:- Open Closed Principle
- L:- Liskov Substitution Principle
- I:- Interface Segregation Principle
- D:- Dependency Inversion Principle

## Single Responsibility Principle

This principle states that a class should have only one job. For this analogy we can think of classes as hired freelancers in your startup. The more load you give them in various fields, the more are the chances of error. It's better to give them specific tasks.

Code Example:-

This is the wrong approach.
```dart
class Payment {
  intiatePayment(String type)
  {
    if(type=="Paypal")
    {
      //paypal code
    }
    else if(type=="UPI")
    {
      //UPI Code
    }
  }
}
```

The reason is straight forward. Now the class has to juggle with both PayPal and UPI payment methods along with initiating payment.

This can be solved as
```dart
class Payment {
  intiatePayment(String type)
  {
    if(type=="Paypal")
    {
      PayPalPayment();
    }
    else if(type=="UPI")
    {
      UPIPayment();
    }
  }
}
```

Now we have two other class responsible for their respective actions.
## Open Closed Principal

The principle states that our code should be open for extension but closed for modification.Or in simpler words, make extensions of the parent class. Modify specifically for child classes that extend it.

Code Example:- 
Wrong Approach
```dart
class Payment {
  UPIPayment(){

  }
  PayPalPayment(){
    
  }
}
```

The reason this is bad approach is because anytime we have to change something in UPIPayment or PayPalPayment, we will have to change the parent class which will affect every instance of it.

Better Approach would be:-
``` dart
abstract class Payment{
  //does something
}

class UPIPayment extends Payment{
  //Does UPI Payment
}

class PayPalPayment extends Payment{
  // Does PayPal Payment
}
```

Now whenever we need to do chages in either of the UPI or Paypal, we can just change their respective classes while ensuring parent class doesn't change.

## Liskov Substitution Principle

The architecture guarantee that the subclass will maintain the logic correctness of code.Basically prefer composition (with interfaces) over inheritance. 
Or in pretty simple terms, check your abstraction until it has no errors.

Code Example:-
Bad Approach:-
```dart
abstract class Payment {
  showAmount();
  upiPaymentcompletion();
}

class UPIPayment extends Payment {
  @override
  upiPaymentcompletion() {
    // implement upiPaymentcompletion
  }

  @override
  showAmount() {
    // implement showAmount
  }
}

class PayPalPayment extends Payment {
  @override
  showAmount() {
    // implement showAmount
  }

  @override
  upiPaymentcompletion() {
    //doesn't need it and yet it is here
  }
}
```

Now the reason this is a bad abstraction is because PayPalPayment has no use for the function upiPaymentCompletion and yet has to override it.

A better approach would be:-
```dart
abstract class Payment {
  showAmount();
}

abstract class upiPaymentClass {
  upiPaymentCompletion();
}

class UPIPayment implements Payment, upiPaymentClass {
  @override
  showAmount() {
    // implement showAmount
  }

  @override
  upiPaymentCompletion() {
    // implement upiPaymentCompletion
  }
}

class PayPalPayment extends Payment {
  @override
  showAmount() {
    // implement showAmount
  }
}
```
This is the correct approach as PayPalPayment class avoids inheriting the upiPaymentCompletion function.

## Interface Segregation Principle
It states that no client should be forced to depend on methods it does not use.
Or in simpler terms, instead of making big interfaces, make smaller and precise ones.

Code Example:-
Bad Approach:-

```dart
abstract class Payment {
  showAmount();
  upiPayment();
  paypalPaymnet();
  wireTransfer();
}
```
Now remember the last principle. Anytime a class will implement Payment, it will have to override some non useful functions. And not only that, this also makes the abstract class bulky. It'd be better if we divide it into separate abstract classes.

```dart
abstract class Payment {
  showAmount();
}

abstract class UPIPayment {
  UPIPayment();
}

abstract class PaypalPayment {
  paypalPayment();
}

abstract class WireTransfer {
  WireTransfer();
}
```

## Dependency Inversion Principle
Abstractions should not depend on details(concrete implementations). They should depend on abstractions.
Or what it means to say is, don't extend concrete classes. Instead extend abstractions.

Code Example:
Bad Approach:-
```dart
class Payment {
  payment() {
    //does something
  }
}

class UPI extends Payment {
  @override
  payment() {
    // implement payment
    //plus do something else too
    return super.payment();
  }
}
```

Now the problem with this, whenever we change something in Payment we have to reflect these changes in UPI too. This will take time to manually check everything. Instead we can just make an abstract class of Payment and do things specifically in UPI. 

```dart
abstract class Payment {
  payment();
}

class UPI extends Payment {
  @override
  payment() {
    // implement payment
    //plus do something else too
  }
}
```
