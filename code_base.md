##### **1. Activer les clés étrangères**



PRAGMA foreign\_keys = ON;



##### **2. Créer les tables de base (sans dépendances)**



###### **Table users :**



CREATE TABLE IF NOT EXISTS users (

&#x20;   id INTEGER PRIMARY KEY AUTOINCREMENT,

&#x20;   username TEXT NOT NULL UNIQUE,

&#x20;   password TEXT NOT NULL,

&#x20;   is\_admin INTEGER NOT NULL DEFAULT 0 CHECK (is\_admin IN (0, 1)),

&#x20;   is\_blocked INTEGER NOT NULL DEFAULT 0 CHECK (is\_blocked IN (0, 1))

);



###### **Table products :**



CREATE TABLE IF NOT EXISTS products (

&#x20;   id INTEGER PRIMARY KEY AUTOINCREMENT,

&#x20;   name TEXT NOT NULL,

&#x20;   price REAL NOT NULL CHECK (price >= 0),

&#x20;   stock INTEGER NOT NULL DEFAULT 0 CHECK (stock >= 0),

&#x20;   is\_available INTEGER NOT NULL DEFAULT 1 CHECK (is\_available IN (0, 1))

);



###### **Table menus :**



CREATE TABLE IF NOT EXISTS menus (

&#x20;   id INTEGER PRIMARY KEY AUTOINCREMENT,

&#x20;   name TEXT NOT NULL,

&#x20;   price REAL NOT NULL CHECK (price >= 0),

&#x20;   available\_from TEXT,

&#x20;   available\_to TEXT

);



###### **Table closing\_days :**



CREATE TABLE IF NOT EXISTS closing\_days (

&#x20;   id INTEGER PRIMARY KEY AUTOINCREMENT,

&#x20;   date TEXT NOT NULL UNIQUE

);



##### **3. Créer les tables avec clés étrangères**



###### **Table menu\_products :**



CREATE TABLE IF NOT EXISTS menu\_products (

&#x20;   menu\_id INTEGER NOT NULL,

&#x20;   product\_id INTEGER NOT NULL,

&#x20;   quantity INTEGER NOT NULL DEFAULT 1 CHECK (quantity > 0),

&#x20;   PRIMARY KEY (menu\_id, product\_id),

&#x20;   FOREIGN KEY (menu\_id) REFERENCES menus(id) ON DELETE CASCADE,

&#x20;   FOREIGN KEY (product\_id) REFERENCES products(id) ON DELETE CASCADE

);



###### **Table order\_items :**



CREATE TABLE IF NOT EXISTS order\_items (

&#x20;   id INTEGER PRIMARY KEY AUTOINCREMENT,

&#x20;   order\_id INTEGER NOT NULL,

&#x20;   product\_id INTEGER,

&#x20;   menu\_id INTEGER,

&#x20;   quantity INTEGER NOT NULL DEFAULT 1 CHECK (quantity > 0),

&#x20;   FOREIGN KEY (order\_id) REFERENCES orders(id) ON DELETE CASCADE,

&#x20;   FOREIGN KEY (product\_id) REFERENCES products(id) ON DELETE RESTRICT,

&#x20;   FOREIGN KEY (menu\_id) REFERENCES menus(id) ON DELETE RESTRICT,

&#x20;   CHECK (

&#x20;       (product\_id IS NOT NULL AND menu\_id IS NULL) OR

&#x20;       (product\_id IS NULL AND menu\_id IS NOT NULL)

&#x20;   )

);

