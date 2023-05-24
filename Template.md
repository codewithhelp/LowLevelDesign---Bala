

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

Example: java.lang.Runtime in java, Examples like DB Connection

class Singleton{

    // private methods

    private Singleton(){
        sysout("singleton init..")
    }

    private static class Lazy {
        private static final Single INSTANCE = new Single();
    }
    
    // public methods

    public static Single getInstance() {
		return Lazy.INSTANCE;
	}

}


# Builder : Construct/Create complext objects

Usecase1: complex object with lots of optional parameters.
Usecase2: complex object with step by step approach.

Advantage: Reduce number of constructors, understanable and maintainable
Disadvantages: Code increases to maintain builder code

Example: java.lang.StringBuilder


class Phone{
    String name, os;

    Phone(String name, String os){
        this.name = name;
        this.os = os;
    }
}

class PhoneBuilder{
    String name, os;

    PhoneBuilder setName(String name){
        this.name = name;
        return this;
    }

    PhoneBuilder setOS(String os){
        this.os = os;
        return this;
    }

    Phone buildPhone(){
        return new Phone(name, os);
    }
}

class Client{
    psv main(){
        Phone phone = new PhoneBuilder().setName("Nokia").setOS("Symbion").buildPhone();
    }
}


# Factory : Creates object without exposing the logic, and referes the newly created objects using common interface

Example: getInstance() in java.util.calender

Advantage: Loose coupling & Hide implementation logic
Disadvantages: It creates too many sub clasees with minor differences

Usecase1: Creating Objects Based on Runtime Conditions

class CarFactory {
	public static Car buidCar(CarType type) {
		switch (type) {
            case SEDEN:
                return new SedenCar();
            case HATCHBACK:
                return new HatchbackCar();
        }
        return null;
	}
}

public static enum CarType {
	SEDEN, HATCHBACK
}

public static abstract class Car {
    private CarType type = null;
	Car(CarType type) {
		this.type = type;
    }

	public void setType(CarType type) {
		this.type = type;
	}

	public CarType getType() {
		return this.type;
	}

	protected abstract void construct();
}

public static class SedenCar extends Car {

	SedenCar() {
		super(CarType.SEDEN);
	}

	@Override
	protected void construct() {
		System.out.println("Constructed SEDEN");
	}
}

public static class HatchbackCar extends Car {
	public HatchbackCar() {
		super(CarType.HATCHBACK);
	}

	@Override
    protected void construct() {
		System.out.println("Constructed HATCHBACK");
	}
}


* Abstract Factory : Super factory, which creates other factories. Facroty of Factories, Car Factory at multion regions | Useful at Frameworks
* Prototype : Creates new object, by copying existing object | Difficult to implement, Clone method in java


## Structural : deals with relationship between object

# Adapter :  Bridge b/w two incompatible interfaces

class Client {

	public static void main(String[] args) {

		Bird sparrow = new Sparrow();
		ToyDuck toyDuck = new PlasticToyDuck();

		ToyDuck birdAdapter = new BirdAdapter(sparrow);

		System.out.println("Sparrow");
		sparrow.fly();
		sparrow.makeSound();

		System.out.println("Toyduck");
		toyDuck.squeak();

		// bird behaves like toyduck
		System.out.println("bird adapter");
		birdAdapter.squeak();
	}


	interface Bird {
		public void fly();
		public void makeSound();
	}

	static class Sparrow implements Bird {
		@Override
		public void fly() {
			System.out.println("Flying");
		}
		@Override
		public void makeSound() {
			System.out.println("Chirp Chirp");
		}
	}

	interface ToyDuck { // it'll not fly, it'll make squeak sound
		public void squeak();
	}

	static class PlasticToyDuck implements ToyDuck {
		@Override
		public void squeak() {
			System.out.println("Squeak");
		}
	}

    // adapter
	static class BirdAdapter implements ToyDuck {
		Bird bird;
		public BirdAdapter(Bird bird) {
			this.bird = bird;
		}
		@Override
		public void squeak() {
			bird.makeSound();
		}
	}

}

# Bridge : Seperates the abstraction from its implementaion, so that the two can vary independently

class Client{
    public static void main(String[] args) {

		Shape rectangle = new Rectangle(new Red());
		rectangle.colorIt();

		Shape circle = new Circle(new Blue());
		circle.colorIt();

	}
}

abstract static class Shape {
	protected Color color;
	abstract public void colorIt();
}

static class Rectangle extends Shape {
	public Rectangle(Color color) {
        this.color = color;
	}
	@Override
	public void colorIt() {
		color.color();
		System.out.println(" rectangle");
    }
}

static class Circle extends Shape {
	public Circle(Color color) {
		this.color = color;
	}
	@Override
	public void colorIt() {
		color.color();
		System.out.println(" circle");
	}
}

interface Color {
	abstract public void color();
}

static class Blue implements Color {
	@Override
	public void color() {
		System.out.print("Blue color");
	}
}

static class Red implements Color {
	@Override
	public void color() {
		System.out.print("Red color");
	}
}


# Decorator : Adding new functionality to the existing object, without altering its strcture

class Client {
	public static void main(String[] args) {

		Pizza pizza = new SimplePizza();
		System.out.println(pizza.getDescription());
		System.out.println(pizza.getCost());

		Pizza pannerPizza = new Paneer(pizza);
		System.out.println(pannerPizza.getDescription());
		System.out.println(pannerPizza.getCost());

	}

	static abstract class Pizza {
		String desc = "Unknown";

		abstract int getCost();

		public String getDescription() {
			return desc;
		}
	}

	static abstract class ToppingsDecorator extends Pizza {
		abstract public String getDescription();
	}

	static class SimplePizza extends Pizza {
		SimplePizza() {
			this.desc = "SimplePizza";
		}

		@Override
		int getCost() {
			return 10;
		}
	}

	static class Paneer extends ToppingsDecorator {
		Pizza pizza;

		Paneer(Pizza pizza) {
			this.pizza = pizza;
		}

		@Override
		public String getDescription() {
			return this.pizza.getDescription() + " decorated with paneer";
		}

		@Override
		int getCost() {
			return 25 + this.pizza.getCost();
		}

	}
}

# Composite : Compose objects in to tree strcture to represent part-whoe hierachies
Example: directory contains directories or files, file is a leaf node, directory is a composite

class Client {
	public static void main(String[] args) {

		Developer dev1 = new Developer("Bala", "Senior");
		Developer dev2 = new Developer("Satish", "Senior 2");
		CompanyDirectory devDirectory = new CompanyDirectory();
		devDirectory.addEmployee(dev1);
		devDirectory.addEmployee(dev2);

		Manager man1 = new Manager("Bala", "Senior");
		Manager man2 = new Manager("Satish", "Senior 2");
		CompanyDirectory manDirectory = new CompanyDirectory();
		manDirectory.addEmployee(man1);
		manDirectory.addEmployee(man2);

		CompanyDirectory root = new CompanyDirectory();
		root.addEmployee(devDirectory);
		root.addEmployee(manDirectory);

		root.showEmployeeDetails();
	}

	static interface Employee {
		public void showEmployeeDetails();
	}

	// leaf
	static class Developer implements Employee {
		String name, position;

		public Developer(String name, String position) {
			this.name = name;
			this.position = position;
		}

		@Override
		public void showEmployeeDetails() {
			System.out.println("Developer Name: " + name + ", Position: " + position);
		}
	}

	// leaf
	static class Manager implements Employee {
		String name, position;

		public Manager(String name, String position) {
			this.name = name;
			this.position = position;
		}

		@Override
		public void showEmployeeDetails() {
			System.out.println("Manager Name: " + name + ", Position: " + position);
		}
	}

	// non-leaf/composite node
	static class CompanyDirectory implements Employee {

		List<Employee> employees = new ArrayList<Employee>();

		@Override
		public void showEmployeeDetails() {
			for (Employee employee : employees)
				employee.showEmployeeDetails();
		}

		public void addEmployee(Employee emp) {
			employees.add(emp);
		}

		public void removeEmployee(Employee emp) {
			employees.remove(emp);
		}
	}
}
 

# Facede : Noting but face of a building, means provides simple interface to client instead of complex sub system. It hides complexity, not reduce complexity.

class Client {
	public static void main(String[] args) {

		HotelKeeperFacade hotelKeeper = new HotelKeeperFacade();
		hotelKeeper.getNonVegMenu();
		hotelKeeper.getVegMenu();

	}

	interface Hotel {
		public void getMenus();
	}

	static class VegRest implements Hotel {
		@Override
		public void getMenus() {
			System.out.println("Veg Rest");
		}
	}

	static class NonVegRest implements Hotel {
		@Override
		public void getMenus() {
			System.out.println("NonVeg Rest");
		}
	}

	static class HotelKeeperFacade {
		public void getVegMenu() {
			VegRest vegRest = new VegRest();
			vegRest.getMenus();
		}

		public void getNonVegMenu() {
			NonVegRest nonVegRest = new NonVegRest();
			nonVegRest.getMenus();
		}
	}
}

* Fly weight: Reuses existing objects by storing them,  creates new object only when no matching object found
* Proxy pattern: Class represents substitue functionality of another class.


## Behavioral : deals with communication between objects

# Observer : Pub/Sub pattern, it is for one-to-many relationship b/w objects. If one object is modified, all the dependent objects get notified automatically. 
Advantage: Loose coupling

class Client {
	public static void main(String[] args) {

		MessageSubOne sub1 = new MessageSubOne();
		MessageSubTwo sub2 = new MessageSubTwo();
		MessageSubThree sub3 = new MessageSubThree();

		MessagePublisher pub = new MessagePublisher();

		pub.attach(sub1);
		pub.attach(sub2);

		pub.notifyUpdate(new Message("Message 1"));

		pub.detach(sub2);
		pub.attach(sub3);

		pub.notifyUpdate(new Message("Message 2"));
	}

	interface Subject {
		void attach(Observer o);

		void detach(Observer o);

		void notifyUpdate(Message m);
	}

	interface Observer {
		public void update(Message m);
	}

	static class Message {
		String message;

		public Message(String message) {
			this.message = message;
		}

		public String getMessage() {
			return this.message;
		}
	}

	static class MessagePublisher implements Subject {

		private List<Observer> observers = new ArrayList<Observer>();

		@Override
		public void attach(Observer o) {
			observers.add(o);
		}

		@Override
		public void detach(Observer o) {
			observers.remove(o);
		}

		@Override
		public void notifyUpdate(Message m) {
			for (Observer o : observers)
				o.update(m);
		}

	}

	static class MessageSubOne implements Observer {

		@Override
		public void update(Message m) {
			System.out.println("Message subscriber one : " + m.getMessage());
		}

	}

	static class MessageSubTwo implements Observer {

		@Override
		public void update(Message m) {
			System.out.println("Message subscriber two : " + m.getMessage());
		}

	}

	static class MessageSubThree implements Observer {

		@Override
		public void update(Message m) {
			System.out.println("Message subscriber three : " + m.getMessage());
		}

	}
}


# Strategy : class behaviour or it's algorithm can be changed at runtime
Example: java.util.comparator, has compare() to order the elements
Advantages: Based on Open/Close principle. Allow clinet to choose any algorithms from set of defined related algorithms.

class Client {
	public static void main(String[] args) {
		// Creating social Media Connect Object for connecting with friend by
		// any social media strategy.
		SocialMediaContext context = new SocialMediaContext();

		// Setting Facebook strategy.
		context.setSocialmediaStrategy(new FacebookStrategy());
		context.connect("Bala");

		System.out.println("====================");

		// Setting Twitter strategy.
		context.setSocialmediaStrategy(new GooglePlusStrategy());
		context.connect("Bala");

		System.out.println("====================");
	}

	interface ISocialMediaStrategy {
		public void connectTo(String friendName);
	}

	static class SocialMediaContext {
		ISocialMediaStrategy smStrategy;

		public void setSocialmediaStrategy(ISocialMediaStrategy smStrategy) {
			this.smStrategy = smStrategy;
		}

		public void connect(String name) {
			smStrategy.connectTo(name);
		}
	}

	static class FacebookStrategy implements ISocialMediaStrategy {

		public void connectTo(String friendName) {
			System.out.println("Connecting with " + friendName + " through Facebook");
		}
	}

	static class GooglePlusStrategy implements ISocialMediaStrategy {

		public void connectTo(String friendName) {
			System.out.println("Connecting with " + friendName + " through GooglePlus");
		}
	}
}

# Template Method :  Defining the skeleton of an algorithm, which shall not be overriden
Examples: java.util.AbstractList/java.util.AbstractSet/java.util.AbstractMap

class Client {
	public static void main(String[] args) {
		System.out.println("Going to build Concrete Wall House");

		House house = new ConcreteWallHouse();
		house.buildhouse();

		System.out.println("Concrete Wall House constructed successfully");

		System.out.println("********************");

		System.out.println("Going to build Glass Wall House");

		house = new GlassWallHouse();
		house.buildhouse();

		System.out.println("Glass Wall House constructed successfully");
	}

	static abstract class House {
		/**
		 * This is the template method we are discussing. This method should be final so
		 * that other class can't re-implement and change the order of the steps.
		 */
		public final void buildhouse() {
			constructBase();
			constructRoof();
			constructWalls();
			constructWindows();
			constructDoors();
			paintHouse();
			decorateHouse();
		}

		public abstract void decorateHouse();

		public abstract void paintHouse();

		public abstract void constructDoors();

		public abstract void constructWindows();

		public abstract void constructWalls();

		/**
		 * final implementation of constructing roof - final as all type of house Should
		 * build roof in same manner.
		 */
		private final void constructRoof() {
			System.out.println("Roof has been constructed.");
		}

		/**
		 * final implementation of constructing base - final as all type of house Should
		 * build base/foundation in same manner.
		 */
		private final void constructBase() {
			System.out.println("Base has been constructed.");
		}
	}

	static class ConcreteWallHouse extends House {
		@Override
		public void decorateHouse() {
			System.out.println("Decorating Concrete Wall House");
		}

		@Override
		public void paintHouse() {
			System.out.println("Painting Concrete Wall House");
		}

		@Override
		public void constructDoors() {
			System.out.println("Constructing Doors for Concrete Wall House");
		}

		@Override
		public void constructWindows() {
			System.out.println("Constructing Windows for Concrete Wall House");
		}

		@Override
		public void constructWalls() {
			System.out.println("Constructing Concrete Wall for my House");
		}
	}

	static class GlassWallHouse extends House {
		@Override
		public void decorateHouse() {
			System.out.println("Decorating Glass Wall House");
		}

		@Override
		public void paintHouse() {
			System.out.println("Painting Glass Wall House");
		}

		@Override
		public void constructDoors() {
			System.out.println("Constructing Doors for Glass Wall House");
		}

		@Override
		public void constructWindows() {
			System.out.println("Constructing Windows for Glass Wall House");
		}

		@Override
		public void constructWalls() {
			System.out.println("Constructing Glass Wall for my House");
		}
	}
}