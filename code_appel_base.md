##### **1. Utilisateurs (users)**

###### 

###### **Créer un compte standard :**



INSERT INTO users (username, password) VALUES (?, ?);

###### 

###### **Créer un compte administrateur (CLI uniquement) :**



INSERT INTO users (username, password, is\_admin) VALUES (?, ?, 1);

###### 

###### **Récupérer un utilisateur à la connexion :**



SELECT id, username, password, is\_admin, is\_blocked FROM users WHERE username = ?;

###### 

###### **Changer son mot de passe :**



UPDATE users SET password = ? WHERE id = ?;

###### 

###### **Bloquer ou débloquer un utilisateur (admin) :**



UPDATE users SET is\_blocked = ? WHERE id = ?;

###### 

###### **Lister tous les utilisateurs (admin) :**



SELECT id, username, is\_admin, is\_blocked FROM users;

##### 

##### **2. Produits (products)**

###### 

###### **Ajouter un produit (admin) :**



INSERT INTO products (name, price, stock, is\_available) VALUES (?, ?, ?, ?);

###### 

###### **Afficher les produits disponibles (visiteurs / utilisateurs) :**



SELECT id, name, price, stock FROM products WHERE is\_available = 1 AND stock > 0;

###### 

###### **Lister tous les produits (admin) :**



SELECT \* FROM products;

###### 

###### **Modifier un produit ou son stock (admin) :**



UPDATE products SET name = ?, price = ?, stock = ?, is\_available = ? WHERE id = ?;

###### 

###### **Décrémenter le stock lors d'une commande :**



UPDATE products SET stock = stock - ? WHERE id = ? AND stock >= ?;

###### 

###### **Supprimer un produit (admin) :**



DELETE FROM products WHERE id = ?;

##### 

##### **3. Menus (menus et menu\_products)**

###### 

###### **Créer un menu (admin) :**



INSERT INTO menus (name, price, available\_from, available\_to) VALUES (?, ?, ?, ?);

###### 

###### **Associer un produit à un menu (admin) :**



INSERT INTO menu\_products (menu\_id, product\_id, quantity) VALUES (?, ?, ?);

###### 

###### **Afficher les menus valides à une date donnée :**



SELECT \* FROM menus WHERE (available\_from IS NULL OR available\_from <= ?) AND (available\_to IS NULL OR available\_to >= ?);

###### 

###### **Récupérer la composition d'un menu :**



SELECT p.id, p.name, p.stock, p.is\_available, mp.quantity FROM products p JOIN menu\_products mp ON p.id = mp.product\_id WHERE mp.menu\_id = ?;

###### 

###### **Supprimer un menu (admin) :**



DELETE FROM menus WHERE id = ?;

##### 

##### **4. Jours de fermeture (closing\_days)**

###### 

###### **Ajouter un jour de fermeture (admin) :**



INSERT INTO closing\_days (date) VALUES (?);

###### 

###### **Vérifier si une date de retrait est fermée :**



SELECT 1 FROM closing\_days WHERE date = ?;

###### 

###### **Lister tous les jours de fermeture :**



SELECT \* FROM closing\_days ORDER BY date ASC;

###### 

###### **Supprimer un jour de fermeture (admin) :**



DELETE FROM closing\_days WHERE id = ?;

##### 

##### **5. Commandes (orders et order\_items)**

###### 

###### **Créer l'en-tête de commande :**



INSERT INTO orders (user\_id, pickup\_date) VALUES (?, ?);

###### 

###### **Ajouter un produit unitaire à la commande :**



INSERT INTO order\_items (order\_id, product\_id, quantity) VALUES (?, ?, ?);

###### 

###### **Ajouter un menu à la commande :**



INSERT INTO order\_items (order\_id, menu\_id, quantity) VALUES (?, ?, ?);

###### 

###### **Voir les commandes d'un utilisateur :**



SELECT \* FROM orders WHERE user\_id = ? ORDER BY pickup\_date DESC;

###### 

###### **Voir toutes les commandes avec le nom du client (admin) :**



SELECT o.id, u.username, o.pickup\_date, o.status FROM orders o JOIN users u ON o.user\_id = u.id ORDER BY o.pickup\_date ASC;

###### 

###### **Voir le détail des articles d'une commande :**



SELECT oi.id, oi.quantity, p.name AS product\_name, m.name AS menu\_name FROM order\_items oi LEFT JOIN products p ON oi.product\_id = p.id LEFT JOIN menus m ON oi.menu\_id = m.id WHERE oi.order\_id = ?;

###### 

###### **Changer le statut d'une commande (admin) :**



UPDATE orders SET status = ? WHERE id = ?;

