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