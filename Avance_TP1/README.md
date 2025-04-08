# TP1 Avancé SQL: Mécanismes transactionnels et privilèges

Le but est ici de mettre en valeur les mécanismes transactionnels d'Oracle

## Question 1

Créez une table quelconque et insérez quelques données dans cette table

``` SQL
-- Création de la table
CREATE TABLE site(
    id_site NUMBER(10,0) PRIMARY KEY,
    nom_site VARCHAR(10),
    url_site VARCHAR(30),
    status NUMBER(2)
);

-- Insertion des données dans la table
INSERT INTO site (id_site, nom_site, url_site, status) VALUES (1, 'google', 'https://www.google.com', 1);
INSERT INTO site (id_site, nom_site, url_site, status) VALUES (2, 'chatgpt', 'https://chatgpt.com', 1);
INSERT INTO site (id_site, nom_site, url_site, status) VALUES (3, 'wikipedia', 'https://www.wikipedia.com', 1);
INSERT INTO site (id_site, nom_site, url_site, status) VALUES (4, 'aaaa', 'https://www.aaaaaaaa.com', 2);

-- Création du nouvel utilisateur

-- Gestion des droits de l'utilisateur
```


