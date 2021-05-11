# Java-Exceptions-Lab-Section-18

Exception handling is a normal and required part of Java coding. In this lab, you
retrofit parts of your application to use Java exceptions where they just use
System.out to alert right now. System.out is not something that the application or a
user can effectively use to take action when a problem occurs.  

Specifically, in this lab you will:
* Create an exception class and use it to create an exception object
* See how to throw the exception object when the application detects a problem
or potential problem
* Learn how to use try-catch blocks to capture and react to problems
* Learn how to rethrow an exception as another way to handle an exception

## Scenario

There are several places in the Acme Order System that check for valid data. For
example, in the MyDate class, the valid( ) method checks to ensure proper day,
month, and year data are supplied when creating or updating a MyDate instance. In
the Order class, orders are not allowed to be created with order dates of holidays.
However, in both of these examples, when bad data is provided, the application does
not stop or otherwise return an indication of problem to the offending method
caller. Instead, the data is just not accepted. This might be a worse bug than
actually having the application fail! In this lab, you create a new Exception class and
have an instance of this exception thrown to the caller if an Order is created with a
holiday date as its order date.

## Step 1: Create the HolidayOrdersNotAllowedException

1.1 Create HolidayOrdersNotAllowedException. Right-click on the
com.acme.utils package and select New > Java Class.

<img src="./src/main/resources/newJavaClass.png" width="400px">

In the resulting New Java Class window, enter
HolidayOrdersNotAllowedException, and press Enter. 

<img src="./src/main/resources/HolidayOrders.png" width="400px">

Type "extends Exception" to ensure that the Superclass is java.lang.Exception. 

<img src="./src/main/resources/extendsException.png" width="600px">

1.2 Add a constructor to the new exception class. In the
HolidayOrdersNotAllowedException Java editor that opens after creating
the class, add a constructor to the class. The constructor should be passed
the offending date and use a call to the super constructor to create the
appropriate message.

```java
public HolidayOrdersNotAllowedException(MyDate date){
  super("Orders are not allowed to be created on: " + date);
}
```

## Step 2: Use the new HolidayOrdersNotAllowedException

2.1 Modify the setOrderDate( ) method in Order. In the Package Explorer view,
double-click on the Order.java file in the com.acme.domain package to open
the file in a Java editor.

Modify the setOrderDate( ) method to throw a
HolidayOrdersNotAllowedException when the suggested date is a holiday.

2.1.1 This requires that the method declaration have a “throws
HolidayOrdersNotAllowedException”.

```java
public void setOrderDate(MyDate orderDate) throws HolidayOrdersNotAllowedException {
```

2.1.2 Throwing the exception will require that the Order class imports
com.acme.utils.HolidayOrdersNotAllowedException.

```java
import com.acme.utils.HolidayOrdersNotAllowedException;
```

2.1.3 Modify the body of the if-conditional to create and throw a new
HolidayOrdersNotAllowedException when the new orderDate is determined to be a
holiday.

```java
if (isHoliday(orderDate)) {
   System.out.println("Order date, " + orderDate + ", cannot be set to a holiday!");
   throw new HolidayOrdersNotAllowedException(orderDate);
} else {
    this.orderDate = orderDate;
}
```

2.1.4 Save the new HolidayOrdersNotAllowedException and the Order classes.
This should result in a compiler error in the Order constructor. As
HolidayOrdersNotAllowedException is a checked exception, it must be handled by
all callers of setOrderDate( ). Currently, the constructor does not handle this
exception.

<img src="./src/main/resources/UnhandledException.png" width="600px">

2.2 Update the constructor. The constructor calls on a method that may throw
a checked exception, and so it must either rethrow or try-catch and handle
the exception. Surround the offending part of the constructor with a try-catch block. In the catch block, display an appropriate error and exit the
system with a call to System.exit(0).

```java
public Order(MyDate d, double amt, String c, Product p, int q) {
   try {
       setOrderDate(d);
   } catch (HolidayOrdersNotAllowedException e){
       System.out.println("The order date for an order cannot be a holiday! Application closing.");
       System.exit(0);
   }
   orderAmount = amt;
   customer = c;
   product = p;
   quantity = q;
}
```

Note: Calling System.exit(0) is rarely an appropriate way to handle an exception.
Handling an exception usually requires the exception to be logged, informing the user
of the issue, and then seeking appropriate action from the user (in this case perhaps
picking a new order date).

2.3 Save your changes to the Order class. Make sure there are no compiler
errors in any class. Run TestOrders. The application should inform you
when an illegal order is created and then stop.

```java
...
The tax for this order is: 60.0
The total bill for: 125 ea. Acme Balloon-1401 that is 375.0
CUBIC_FEET in size for Bugs Bunny is 1040.0
Order date, 1/1/2012, cannot be set to a holiday!
The order date for an order cannot be a holiday! Application
closing.
```

## Step 3: Submission

Commit and push all your changes to your GitHub repository.
