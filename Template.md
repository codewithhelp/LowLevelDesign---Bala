

Aim of Low Level Design
1. Extensible
2. Readable
3. Testable
4. Maintainable


Solid Principles:

1. Single Responsibility

Each funciton/class/module should have a single, well defined responsibility.
There should be only one reason to change the code


class Marker{
    String name;
    Int price;
}
class Invoice{
    private Marker marker;
    int quantity;
    void calculatePrice(){}
    void saveToDB(){}
    void printInvoice(){}
}

In the above code, we need to tiuch the invoice class in below use cases
    to modify the calculation 
    to save to db 
    to print invoice
as the code is voilaing the SRP(single responsibility principle)


// save to db
class InvoiceDao{
    InvoiceDao(Invoice invoice){ }
    void saveToDB() {}
}

// print invoice
class InvoicePrinter{
    InvoiceDao(Invoice invoice){ }
    void print() {}
}


Now the each class has it's own well defined single responsibility





2. Open Close Principle: Open for extension & closed for modifications


class InvoiceDao{
    void saveInDB(){
        ...
    }
}

Suppose we have requirement to introduce saveToFile

class InvoiceDao{
    void saveInDB(){
        ...
    }
    void saveToFile(){
        ...
    }
}

But the above code is not following the Solid (Open for extension & closed for modifications), we are modifing the class on adding new functionality


interface InvoiceDao{
    public void save(Invoice invoice);
}
class DatabaseInvoiceDao impletements InvoiceDao{
    @override
    public void save(Invoice invoice){
        // save to db
    }
}
class FileInvoiceDao impletements InvoiceDao{
    @override
    public void save(Invoice invoice){
        // save to files
    }
}

In future, if new requirement of save to drive comes, we extend by create class DriveInvoiceDao implements InvoiceDao{} ...etc.,
we do not required to modify the existing code.


3. Liskov Substitution 
Any functionality works for parent class, should also work for child class
Means, any extension to the code should NOT break the existing features

interface Bike{
    void turnOnEngine(){
    }
    void accelerate(){

    }
}

class Motorcycle extends Bike{
    void turnOnEngine(){
        // on enginer
    }
    void accelerate(){
        // do accelerate
    }
}

class Bicycle extends Bike{ 
    void turnOnEngine(){
        exception throws error <<<<<<<<=====
    }
    void accelerate(){
        // do accelerate
    }
}

to fix the above code, keep the generic functionality in the common interface.

interface Vehicle{
    void accelerate(){}
}
interface EngineVehicle extends Vehicle{
    void turnOnEngine(){}
}



4. Interface Segregation

Keep your interfaces minimal, No code should be forced to implement a method that it does not need


interface RestaurentEmployee{
    washDishes()
    serveFood()
    cookFood()
}
class Waiter implements RestaurentEmployee{
    washDishes(){
        // not my job
    }
    serveFood(){
        // i'll do
    }
    cookFood(){
        // not my job
    }
}


Now change the above code, as most of the forced methods from interface are not required. "Segment the interfaces to small interfaces"

interface WaiterInterface{
    serveFood()
    takeOrders()
}

interface ChefInterface{
    cookFood()
    decideMenu()
}

5. Dependency Inversion

Class should depend on Interfaces, rather than concrete classes

class Macbook{
    BluetoothKeyboard keyboard;
    Macbook(BluetoothKeyboard keyboard){

    }
}


the above code is restricted to BluetoothKeyboard object, below code is flexible and extensible to choose BluetoothKeyboard/WirelessKeyboard...etc 


class Macbook{
    Keyboard keyboard;
    Macbook(Keyboard keyboard){

    }
}




### Design Patterns

Reusable solutions to commonly occuring problems in software design.


## Creational : Deals with object creation

# Singleton : Restricts to create one and only one object

Example: java.lang.Runtime

# Builder : Construct/Create complext objects

Example: java.lang.StringBuilder

Advantage: Reduce number of constructors, understanable and maintainable
Disadvantages: Code increases to maintain builder code


# Factory : Creates object without exposing the logic, and referes the newly created objects using common interface

Example: getInstance() in java.util.calender

Advantage: Loose coupling & Hide implementation logic
Disadvantages: It creates too many sub clasees with minor differences


* Abstract Factory : Super factory, which creates other factories. Facroty of Factories, Car Factory at multion regions | Useful at Frameworks
* Prototype : Creates new object, by copying existing object | Difficult to implement, Clone method in java


## Structural : deals with relationship between object

# Adapter :  Bridge b/w two incompatible interfaces

# Bridge : Seperates the abstraction from its implementaion, so that the two can vary independently

# Decorator : Adding new functionality to the existing object, without altering its strcture

# Composite : Compose objects in to tree strcture to represent part-whoe hierachies

# Facede : Noting but face of a building, means provides simple interface to client instead of complex sub system. It hides complexity, not reduce complexity.

* Fly weight: Reuses existing objects by storing them,  creates new object only when no matching object found
* Proxy pattern: Class represents substitue functionality of another class.


## Behavioral : deals with communication between objects

# Observer : Pub/Sub pattern, it is for one-to-many relationship b/w objects. If one object is modified, all the dependent objects get notified automatically. 
Advantage: Loose coupling

# Strategy : class behaviour or it's algorithm can be changed at runtime
Example: java.util.comparator, has compare() to order the elements
Advantages: Based on Open/Close principle. Allow clinet to choose any algorithms from set of defined related algorithms.

# Template Method :  Defining the skeleton of an algorithm, which shall not be overriden
Examples: java.util.AbstractList/java.util.AbstractSet/java.util.AbstractMap