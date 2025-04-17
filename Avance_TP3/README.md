# TP 3 Avancé SQL: Création d'un programme en PL/SQL

Le but est ici de réaliser un programme en PL/SQL qui permette de gérer le personnel d'une
entreprise, notamment au niveau salarial.

## Question 1

créer la table PERSONNEL:

``` SQL
CREATE TABLE personnel (
    nom        VARCHAR2(50),
    emploi     VARCHAR2(20) CHECK (emploi IN ('Vendeur', 'Salarie')),
    sal_hor    NUMBER(6,2),
    nb_heure   NUMBER(5),
    comm       NUMBER(6,2),
    nb_vente   NUMBER(5)
);
```

## Question 2

Créez une procédure PL/SQL pour insérer une personne avec vérification

``` SQL
CREATE OR REPLACE PROCEDURE inserer_personne (
    p_nom      IN VARCHAR2,
    p_emploi   IN VARCHAR2,
    p_sal_hor  IN NUMBER,
    p_nb_heure IN NUMBER,
    p_comm     IN NUMBER,
    p_nb_vente IN NUMBER
) AS
BEGIN
    -- Vérifications selon le type d'emploi
    IF p_emploi = 'Salarie' AND (p_comm <> 0 OR p_nb_vente <> 0) THEN
        RAISE_APPLICATION_ERROR(-20001, 'Un salarié ne peut pas avoir une commission ou des ventes.');
    
    ELSIF p_emploi = 'Vendeur' AND p_comm = 0 THEN
        RAISE_APPLICATION_ERROR(-20002, 'Un vendeur doit avoir une commission non nulle.');
    
    ELSE
        INSERT INTO personnel (nom, emploi, sal_hor, nb_heure, comm, nb_vente)
        VALUES (p_nom, p_emploi, p_sal_hor, p_nb_heure, p_comm, p_nb_vente);
        
        DBMS_OUTPUT.PUT_LINE('Insertion réussie pour ' || p_nom);
    END IF;
END;
/
```

## Question 3

Créez une Fonction PL/SQL pour calculer le salaire d’une personne donnée

``` SQL
CREATE OR REPLACE FUNCTION calcul_salaire (
    p_nom IN VARCHAR2
) RETURN NUMBER IS
    v_salaire NUMBER;
    v_emploi  VARCHAR2(20);
    v_sal_hor NUMBER;
    v_nb_heure NUMBER;
    v_comm   NUMBER;
    v_nb_vente NUMBER;
BEGIN
    SELECT emploi, sal_hor, nb_heure, comm, nb_vente
    INTO v_emploi, v_sal_hor, v_nb_heure, v_comm, v_nb_vente
    FROM personnel
    WHERE nom = p_nom;

    IF v_emploi = 'Salarie' THEN
        v_salaire := v_sal_hor * v_nb_heure;
    ELSIF v_emploi = 'Vendeur' THEN
        v_salaire := v_sal_hor * v_nb_heure + v_comm * v_nb_vente;
    END IF;

    RETURN v_salaire;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RAISE_APPLICATION_ERROR(-20003, 'Personne non trouvée.');
END;
/
```

## Question 4

Affichez les salaires de toutes les personnes

``` SQL
BEGIN
    FOR rec IN (SELECT nom FROM personnel) LOOP
        DBMS_OUTPUT.PUT_LINE('Nom : ' || rec.nom || ' - Salaire : ' || calcul_salaire(rec.nom));
    END LOOP;
END;
/
```

## Utilisation 

``` SQL
-- Activer l'affichage
SET SERVEROUTPUT ON;

-- Insertion correcte
BEGIN
    inserer_personne('Alice', 'Salarie', 20, 35, 0, 0);
    inserer_personne('Bob', 'Vendeur', 15, 30, 10, 5);
END;
/

-- Insertion incorrecte (déclenche une erreur)
BEGIN
    inserer_personne('Charlie', 'Salarie', 25, 35, 5, 2); -- ERREUR
END;
/

-- Calcul d’un salaire
BEGIN
    DBMS_OUTPUT.PUT_LINE('Salaire de Alice : ' || calcul_salaire('Alice'));
END;
/

-- Liste de tous les salaires
BEGIN
    FOR rec IN (SELECT nom FROM personnel) LOOP
        DBMS_OUTPUT.PUT_LINE(rec.nom || ' : ' || calcul_salaire(rec.nom));
    END LOOP;
END;
/
```