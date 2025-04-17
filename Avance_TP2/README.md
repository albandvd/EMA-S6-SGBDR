# TP2 Avancé SQL: Optimisation des requêtes

Le but est ici de réaliser une requête en étoile (jointure) sur des tables existantes.
Il faudra répondre à cette question en utilisant 2 méthodes :
  - Les opérateurs ensemblistes (conjointement avec des jointures si nécessaire)
  - La combinatoire (jointures uniquement)

## Question 1

Importer le schéma relationnel depuis le fichier expdat_video.dmp

```bash
#sur la machine hôte
scp -P 2222 expdat_video.dmp adavid@localhost:~

#sur la machine virtuelle
docker cp expdat_video.dmp 23cfree:/tmp/expdat_video.dmp
docker exec -it 23cfree bash
cd /tmp
imp # attention la commande imp pue la merde et ne comprends pas les caractères è ou , par exemple 
# répondre yes à "Import entire export file"
```

## Question 2 

Créez les requêtes permettant de renvoyer toutes les vidéos qualifiées à la fois par le mot clé intitulé oracle et celui intitulé java, ces 2 mots étant dans la langue fr.

Méthode 1 
``` SQL
-- Opérateurs ensemblistes

SELECT * FROM video
WHERE id_video IN (
    SELECT vm.id_video
    FROM video_motcle vm
    JOIN motcle mc ON vm.id_motcle = mc.id_motcle
    JOIN langue l ON mc.id_langue = l.id_langue
    WHERE mc.intitule = 'oracle' AND l.code_langue = 'fr'

    INTERSECT

    SELECT vm.id_video
    FROM video_motcle vm
    JOIN motcle mc ON vm.id_motcle = mc.id_motcle
    JOIN langue l ON mc.id_langue = l.id_langue
    WHERE mc.intitule = 'java' AND l.code_langue = 'fr';
);
```

Méthode 2

```SQL
-- Jointure
SELECT * FROM video
WHERE id_video IN (
    SELECT v.*
    FROM video v
    JOIN video_motcle vm1 ON v.id_video = vm1.id_video
    JOIN motcle mc1 ON vm1.id_motcle = mc1.id_motcle
    JOIN langue l1 ON mc1.id_langue = l1.id_langue

    JOIN video_motcle vm2 ON v.id_video = vm2.id_video
    JOIN motcle mc2 ON vm2.id_motcle = mc2.id_motcle
    JOIN langue l2 ON mc2.id_langue = l2.id_langue

    WHERE mc1.intitule = 'oracle' AND l1.code_langue = 'fr'
    AND mc2.intitule = 'java' AND l2.code_langue = 'fr';
);
```

## Question 3 

Optimisez les rêquetes à l'aide des indexes 

``` SQL
CREATE INDEX idx_mc_intitule ON motcle(intitule);
CREATE INDEX idx_mc_id_langue ON motcle(id_langue);
CREATE INDEX idx_vm_id_video ON video_motcle(id_video);
CREATE INDEX idx_vm_id_motcle ON video_motcle(id_motcle);
```