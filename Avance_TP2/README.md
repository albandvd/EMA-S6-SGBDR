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

```

```