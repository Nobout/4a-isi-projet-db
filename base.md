##### **Tables et colonnes simplifiées**



###### **users :**



\- id : entier, clé primaire.

\- username : texte, unique, non nul.

\- password : texte, non nul (stocké haché par le back-end).

\- is\_admin : booléen (0 ou 1), défaut 0.

\- is\_blocked : booléen (0 ou 1), défaut 0.



###### **products :**



\- id : entier, clé primaire.

\- name : texte, non nul.price : réel, non nul.

\- stock : entier, défaut 0.

\- is\_available : booléen (0 ou 1), défaut 1.  

&#x20;

###### **menus :**



\- id : entier, clé primaire.

\- name : texte, non nul.price : réel, non nul.

\- available\_from : date, optionnelle (début de validité).   

\- available\_to : date, optionnelle (fin de validité ; nulles toutes les deux = menu permanent).   



###### **menu\_products (association N:N entre menus et produits) :**



\- menu\_id : entier, clé primaire + clé étrangère vers menus.id.product\_id : entier, clé primaire + clé étrangère vers products.id.

\- quantity : entier, défaut 1.



###### **orders :**



\- id : entier, clé primaire.

\- user\_id : entier, clé étrangère vers users.id, non nul.

\- pickup\_date : date, non nulle (jour de récupération).

\- status : texte, défaut 'pending' (ex. : 'pending', 'completed', 'no\_show' pour justifier un blocage).



###### **order\_items (contenu de la commande) :**



\- id : entier, clé primaire.

\- order\_id : entier, clé étrangère vers orders.id, non nul.

\- product\_id : entier, clé étrangère vers products.id, optionnel.

\- menu\_id : entier, clé étrangère vers menus.id, optionnel.

\- quantity : entier, défaut 1.



###### **closing\_days :**



\- id : entier, clé primaire.

\- date : date, unique, non nulle.

