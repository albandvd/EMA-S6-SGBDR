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

-- Gestion des droits de l'utilisateur (à réaliser sur C##DAVID)
GRANT SELECT, UPDATE ON SITE TO C##ALBAN

-- Suppression des droits
REVOKE SELECT, UPDATE ON site FROM C##ALBAN;
```

Vérifiez les mécanismes transactionnels et la pose des verrous par Oracle

``` SQL
-- User 1 
UPDATE site SET nom_site = 'GoogleFR' WHERE id_site = 1;

-- User 2
UPDATE site SET nom_site = 'Ggl' WHERE id_site = 1;

-- User 1
COMMIT;
```

Créez un interblocage et vérifiez qu'Oracle le détecte et le résout

``` SQL
-- User 1
UPDATE site SET nom_site = 'GoogleX' WHERE id_site = 1;

-- User 2
UPDATE site SET nom_site = 'WikiX' WHERE id_site = 3;

-- User 1
UPDATE site SET nom_site = 'WikipediaFromA' WHERE id_site = 3;

-- User 2
UPDATE site SET nom_site = 'GoogleFromB' WHERE id_site = 1;
```

Oracle détecte l’interblocage et renvoi un message d'erreur "deadlock detected while waiting for resource"