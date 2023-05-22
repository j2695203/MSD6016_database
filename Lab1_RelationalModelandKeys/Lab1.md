### Part 1 - English to Schema

1. Grocery Store
    
    `AllProduct [__SKU(string)__, name(string), type(string), quantity(integer), price(real)]`
    
    | SKU | Name | Type | Quantity | Price |
    | --- | --- | --- | --- | --- |
    | apple-13-256g-white | iPhone13 | mobile | 5000 | 700 |
    | apple-13-256g-black | iPhone13 | mobile | 5000 | 700 |
    | google-6-256g-black | pixel6 | mobile | 3000 | 500 |
    | 29394582 | type-C charge | charger | 10000 | 10 |
    | 42892430 | USB charge | charger | 10000 | 10 |
    
    `InventoryByType [__type(string)__, quantity(integer)]`
    
    | Type | Quantity |
    | --- | --- |
    | mobile | 13000 |
    | charger | 20000 |
2. Grocery store + Aisle
    
    `ProductAisle [__SKU(string)__, __aisle(integer)__]`
    
    | SKU | Aisle |
    | --- | --- |
    | apple-13-256g-white | 1 |
    | apple-13-256g-white | 2 |
    | apple-13-256g-black | 1 |
    | google-6-256g-black | 3 |
    | cc2939 | 3 |
    | cc2939 | 4 |
    | bb4289 | 1 |
    | bb4289 | 2 |
3. Car dealership
    
    `Car [__VIN(integer)__, make(string), model(string), year(integer), color(string)]`
    
    | VIN | make | model | year | color |
    | --- | --- | --- | --- | --- |
    | 10001 | Toyota | Camry | 1999 | black |
    | 10002 | Toyota | Rav4 | 2022 | black |
    | 20001 | Mazda | 3 | 2000 | grey |
    | 20002 | Mazda | cx5 | 2004 | red |
    | 30001 | Honda | civic | 2020 | silver |
    
    `Salesperson [__SSN(integer)__, name(string)]`
    
    | SSN | name |
    | --- | --- |
    | 123456789 | Alice |
    | 223123123 | Bob |
    | 333984945 | Chase |
    | 448593782 | Dan |
    | 538756398 | Elyse |
    
    `AssignedCars [__salesSSN(integer)__, __carVIN(integer)__]`
    
    | SSN | VIN |
    | --- | --- |
    | 123456789 | 10001 |
    | 123456789 | 10002 |
    | 333984945 | 10001 |
    | 538756398 | 10001 |
    | 538756398 | 30001 |

### Part 2 - SQL Table Declarations

```sql
CREATE TABLE Patrons (
    Name char(20),
    CardNum int,
    PRIMARY KEY (CardNum)
)

CREATE TABLE Phones (
    CardNum int,
    Phone int,
    PRIMARY KEY (CardNum, Phone),
    FOREIGN KEY (CardNum) REFERENCES Patrons(CardNum)
)

CREATE TABLE Titles (
    ISBN int,
    Title char(50),
    Author char(20),
    PRIMARY KEY (ISBN),
    UNIQUE (Title, Author)
)

CREATE TABLE Inventory (
    SerialNum int,
    ISBN int,
    PRIMARY KEY (SerialNum)
    FOREIGN KEY (ISBN) REFERENCES Titles(ISBN)
)

CREATE TABLE CheckedOut (
    CardNum int,
    SerialNum int,
    PRIMARY KEY (SerialNum),
    FOREIGN KEY (CardNum) REFERENCES Patrons(CardNum),
    FOREIGN KEY (SerialNum) REFERENCES Inventory(SerialNum)
)
```

### Part 3 - Fill in Tables (Car Dealership)

`Car [__VIN(integer)__, make(string), model(string), year(integer), color(string)]`

| VIN | make | model | year | color |
| --- | --- | --- | --- | --- |
| 10001 | Toyota | Tacoma | 2008 | Red |
| 10002 | Toyota | Tacoma | 1999 | Green |
| 20001 | Tesla | Model 3 | 2018 | White |
| 30001 | Subaru | WRX | 2016 | Blue |
| 40001 | Ford | F150 | 2004 | Red |

`Salesperson [__SSN(integer)__, name(string)]`

| SSN | name |
| --- | --- |
| 111223333 | Arnold |
| 222334444 | Hannah |
| 333445555 | Steve |

`AssignedCars [__salesSSN(integer)__, __carVIN(integer)__]`

| SSN | VIN |
| --- | --- |
| 111223333 | 10001 |
| 111223333 | 10002 |
| 222334444 | 10001 |
| 222334444 | 40001 |
| 333445555 | 20001 |

### Part 4 - Keys and Superkeys

| Attribute Sets | Superkey? | Proper Subsets | Key? |
| --- | --- | --- | --- |
| {A1} | No | N/A | No |
| {A2} | No | N/A | No |
| {A3} | No | N/A | No |
| {A1, A2} | Yes | {A1}, {A2} | Yes |
| {A1, A3} | No | {A1}, {A3} | No |
| {A2, A3} | No | {A2}, {A3} | No |
| {A1, A2, A3} | Yes | {A1}, {A2}, {A3}, {A1, A2}, {A1, A3}, {A2, A3} | No |

### Part 5 - Abstract Reasoning

1. If {x} is a superkey, then any set containing x is also a superkey.
    
    Yes. If x has no duplicated value, then any set containing x also has no duplicated value because x has unique value.
    
2. If {x} is a key, then any set containing x is also a key.
    
    No. A key must be a superkey, so x is a superkey. However, a set can’t be a key if any subsets of its field is a superkey. 
    
3. If {x} is a key, then {x} is also a superkey.
    
    Yes. A key must be a superkey be definition.
    
4. If {x, y, z} is a superkey, then one of {x}, {y}, or {z} must also be a superkey.
    
    No. {x, y, z} has no duplicated value doesn’t mean subsets of its field also has no duplicated value.
    
5. If an entire schema consists of the set {x, y, z}, and if none of the proper subsets of {x, y, z} are keys, then {x, y, z} must be a key.
    
    No. {x, y, z} might has duplicated value.
