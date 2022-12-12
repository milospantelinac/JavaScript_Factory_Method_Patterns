# JavaScript - Factory Method Patterns

Factory metoda stvara nove objekte prema instrukcijama klijenta. Jedan od načina stvaranja objekata u JavaScriptu je pozivanje funkcije konstruktora s operatorom **new**. Međutim, postoje situacije u kojima klijent ne zna ili ne bi trebao znati tj. treba enkapsulirati koje klase zapravo treba instancirati. Factory metoda omogućuje klijentu delegiranje kreiranja objekta dok sve vreme zadržava kontrolu nad tipom koji će instancirati.

## Upotreba Factory metode

Ključni cilj Factory metode je proširivost. Factory metode se često koriste u aplikacijama koje upravljaju, održavaju ili manipulišu kolekcijama objekata koji su različiti, ali istovremeno imaju mnoge zajedničke karakteristike (tj. metode i svojstva). Primer bi bila lista dokumenata s mix-om Xml dokumenata, PDF dokumenata i Docs dokumenata.

## Diagram

<img width="300" alt="factory_pattern" src="https://user-images.githubusercontent.com/21141150/207117228-524fec15-14eb-43c9-a480-fc330ddeb99e.png">

## Učesnici

Objekti koji učestvuju u ovom paternu su:

Creator -- U primeru: Factory
1. Factory objekat koji stvara nove proizvode
2. Implementira 'factoryMethod' koji kreira novo nastale proizvode

AbstractProduct -- ne koristi se u JavaScript-u
1. Deklariše interfejs za proizvode

ConcreteProduct -- U primeru: Employees
1. Proizvod koji se kreira
2. Svi proizvodi imaju isti interfejt (svojstva i metode)

## Primer

U ovom JavaScript primeru Factory objekat kreira četiri različita tipa zaposlenika. Svaki tip zaposlenika ima drugačiju satnicu. Metoda createEmployee() je  aktuelna Factory metoda. Klijent daje instrukciju Factory-u koji tip zaposlenika da kreira prosleđivanjem argumenta unutar Factory metode.

The AbstractProduct in the diagram is not implemented because Javascript does not support abstract classes or interfaces. However, we still need to ensure that all employee types have the same interface (properties and methods).

AbstractProduct u diagramu nije implementiran zato što JavaScript ne podržava abstraktne klase ili interfejs. Međutim, još uvijek moramo osigurati da svi tipovi zaposlenika imaju isti interfejs (svojstva i metode).

Four different employee types are created; all are stored in the same array. Each employee is asked to say what they are and their hourly rate.

Kreirana su četiri različita tipa zaposlenika. Svi se skladište u isti niz. Od svakog zaposlenika se traži da kaže ko je i kolika mu je satnica.

```
var Factory = function () {
    this.createEmployee = function (type) {
        var employee;

        if (type === "fulltime") {
            employee = new FullTime();
        } else if (type === "parttime") {
            employee = new PartTime();
        } else if (type === "temporary") {
            employee = new Temporary();
        } else if (type === "contractor") {
            employee = new Contractor();
        }

        employee.type = type;

        employee.say = function () {
            console.log(this.type + ": rate " + this.hourly + "/hour");
        }

        return employee;
    }
}

var FullTime = function () {
    this.hourly = "$12";
};

var PartTime = function () {
    this.hourly = "$11";
};

var Temporary = function () {
    this.hourly = "$10";
};

var Contractor = function () {
    this.hourly = "$15";
};

function run() {

    var employees = [];
    var factory = new Factory();

    employees.push(factory.createEmployee("fulltime"));
    employees.push(factory.createEmployee("parttime"));
    employees.push(factory.createEmployee("temporary"));
    employees.push(factory.createEmployee("contractor"));

    for (var i = 0, len = employees.length; i < len; i++) {
        employees[i].say();
    }
}

// → fulltime: rate $12/hour
// → parttime: rate $11/hour
// → temporary: rate $10/hour
// → contractor: rate $15/hour
```

