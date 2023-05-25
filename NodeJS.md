# Singleton : Restricts to create one and only one object

let instance = null

class Singleton{
    constructor(){
        this.value = Math.random(100)
    }
    static getInstance(){
        if(!instance)
            instance = new Singleton();
        return instance;
    }

    printValue(){
        console.log(this.value)
    }
}
// Singleton.getInstance()


# Builder : Construct/Create complext objects

class Phone{
    constructor(builder){
        this.name = name;
        this.os = os;
    }
}

class PhoneBuilder{
    constructor(){
        this.name = "";
        this.os = "";
    }
    setName(){
        this.name = name;
        return this;
    }

    setOs(){
        this.os = os;
        return this;
    }

    build(){
        return new Phone(this);
    }
}

// new PhoneBuilder().setName("iPhone").setOs("Mac");

# Factory : Creates object without exposing the logic, and referes the newly created objects using common interface

class Phone{
    constructor(){}
    printName() { console.log("I am a Phone")}
}

class iPhone extends Phone{
    constructor(){super()}
    printName() { console.log("I am a iPhone")}
}

class NokiePhone extends Phone{
    constructor(){super()}
    printName() { console.log("I am a Nokia")}
}

class PhoneFactory{
    static createPhone(name){
        if(name=='iPhone')
            return new iPhone()
        else if(name=='Nokia')
            return new NokiePhone();
        else return null;
    }
}

PhoneFactory.createPhone('iPhone').printName();
PhoneFactory.createPhone('Nokia').printName();



# Adapter :  Bridge b/w two incompatible interfaces

Bird - Fly
Rat - Walk


class Bird{
    fly(){
        console.log('I Fly')
    }
}

class Rat{
    walk(){
        console.log('I Walk')
    }
}

class BirdAdapter extends Rat{
    constructor(bird){
      super()
      this.bird = bird
    }
    walk(){
        this.bird.fly()
    }
}


new Bird().fly()
new Rat().walk()


new BirdAdapter(new Bird()).walk(); // with the bird adaptor i am able to make walk call on bird.


# Bridge : Seperates the abstraction from its implementaion, so that the two can vary independently

class Shape{
  constructor(){}
  draw(){}
  
}

class Rectangle extends Shape{
  constructor(color){
    super();
    this.color = color;
  }
  draw(){
    console.log('I am a rectangle with color '+this.color.getName());
  }
}

class Circle extends Shape{
  constructor(color){
    super();
    this.color = color;
  }
  draw(){
    console.log('I am a rectangle with'+this.color.getName());
  }
}


class Color{
  constructor(){
    this.color = ""
  }
}
class RedColor extends Color{
  constructor(){super(); this.color = 'Red';}
  getName(){ return this.color;}
}
class BlueColor extends Color{
  constructor(){super(); this.color = 'Blue';}
  getName(){ return this.color;}
}


new Rectangle(new RedColor()).draw()
new Rectangle(new BlueColor()).draw()


# Decorator : Adding new functionality to the existing object, without altering its strcture

class Pizza{
  constructor(){}
  getCost(){ return 0; }
  getDescription(){return "Pizza";}
}

class Decorator extends Pizza{
  constructor(){super()}
  getDescription(){return "Pizza";}
}

class Paneer extends Decorator{
  constructor(pizza){
    super()
    this.pizza = pizza;
  }
  
  getDescription(){return "Paneer "+ this.pizza.getDescription();}
  getCost(){ return 10+this.pizza.getCost();}
}

class Chicken extends Decorator{
  constructor(pizza){
    super()
    this.pizza = pizza;
  }
  
  getDescription(){return "Chicken "+ this.pizza.getDescription();}
  getCost(){ return 50+this.pizza.getCost();}
}


new Chicken(new Paneer(new Pizza())).getDescription()

# Composite : Compose objects in to tree strcture to represent part-whoe hierachies
Example: directory contains directories or files, file is a leaf node, directory is a composite
Every node will have add/remove/getChild functions

class Node{
  constructor(name){
    this.children = [];
    this.name = name;
  }
  
  add(child){
      this.children.push(child)
  }
  remove(child){
    var length = this.children.length;
    for (var i = 0; i < length; i++) {
      if (this.children[i] === child) {
        this.children.splice(i, 1);
        return;
      }
    }
  }
  getChild(ind){
    return this.children[ind]
  } 
}


# Facede : Noting but face of a building, means provides simple interface to client instead of complex sub system. It hides complexity, not reduce complexity.

class HotelKeeperFacede{
  getVegMenu(){
    new VegHotel().getMenu();
  }
  getNonVegMenu(){
    new NonVegHotel().getMenu();
  }
}

class Hotel{
  getMenu(){
    
  }
}

class VegHotel extends Hotel{
  getMenu(){
    console.log("Veg Menu...")
  }
}

class NonVegHotel extends Hotel{
  getMenu(){
    console.log("NonVeg Menu...")
  }
}


new HotelKeeper().getVegMenu()



# Observer : Pub/Sub pattern, it is for one-to-many relationship b/w objects. If one object is modified, all the dependent objects get notified automatically. 
Advantage: Loose coupling

publisher => add/remove/notify
observer => update


class Observer {
  constructor(name){this.name = name}
  update(m){ console.log(m+" "+this.name) }
}

class Publisher{
  constructor(){
    this.observeres = []
  }
  add(observer){
    this.observeres.push(observer)
  }
  remove(observer){
    this.observeres.remove(observer)
  }
  notify(text){
    for (let observer of this.observeres)
				observer.update(text);
  }
}

let observer1 = new Observer("observer1");
let observer2 = new Observer("observer2");
let observer3 = new Observer("observer3");

let publisher = new Publisher();
publisher.add(observer1);
publisher.add(observer2);
publisher.add(observer3);

publisher.notify("Hey...texting to you observer...")


# Strategy : class behaviour or it's algorithm can be changed at runtime
Example: java.util.comparator, has compare() to order the elements
Advantages: Based on Open/Close principle. Allow clinet to choose any algorithms from set of defined related algorithms.


class SocialMedia{
  constructor(strategy){this.strategy = strategy}
  login(){this.strategy.login()}
}

class Google{
  login(){ console.log('i am google')}
}

class Facebook{
  login(){ console.log('i am facebook')}
}

new SocialMedia(new Google()).login()
new SocialMedia(new Facebook()).login()


# Template Method :  Defining the skeleton of an algorithm, which shall not be overriden

class House {
  buildHouse() {
    constructBase();
		constructRoof();
		constructWalls();
		decorateHouse();
  }
  constructRoof() {
	  console.log("Roof has been constructed.");
	}
  constructBase() {
	  console.log("Base has been constructed.");
	}
  constructWalls(){}
  decorateHouse(){}
}

class MyHouse extends House {
  constructor(){super()}
  buildHouse() {
    this.constructBase();
		this.constructRoof();
		this.constructWalls();
		this.decorateHouse();
  }
  constructWalls(){ console.log("MyHouse...constructWalls")}
  decorateHouse(){ console.log("MyHouse...decorateHouse")}
}

new MyHouse().buildHouse()