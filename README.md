[readme.txt](https://github.com/Amen229/amen229/files/8864311/readme.txt)

Exercice 1:

 Site 1: 207.180.211.231
 Site 2: 149.102.155.141

1- Démarrer sur site1 un nœud serveur et sur site2 un noeud client.
 D'abord il faut entrer dans l'emplacement du dossier apache-ignite-slim-2.13.0-bin en faisant:
 Site 1:
 cd C:\apache-ignite-slim-2.13.0-bin
 cd apache-ignite-slim-2.13.0-bin
 cd bin
 ssh root@207.180.211.231
--------------------------------------------------
Microsoft Windows [Version 10.0.19043.1586]
(c) Microsoft Corporation. All rights reserved.

C:\Users\user>cd C:\apache-ignite-slim-2.13.0-bin

C:\apache-ignite-slim-2.13.0-bin>cd apache-ignite-slim-2.13.0-bin

C:\apache-ignite-slim-2.13.0-bin\apache-ignite-slim-2.13.0-bin>cd bin

C:\apache-ignite-slim-2.13.0-bin\apache-ignite-slim-2.13.0-bin\bin>ssh root@207.180.211.231
root@207.180.211.231's password:
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-110-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
  _____
 / ___/___  _  _ _____ _   ___  ___
| |   / _ \| \| |_   _/ \ | _ )/ _ \
| |__| (_) | .` | | |/ _ \| _ \ (_) |
 \____\___/|_|\_| |_/_/ \_|___/\___/

Welcome!
--------------------------------------------------------------------
Site 2:
 cd C:\apache-ignite-slim-2.13.0-bin
 cd apache-ignite-slim-2.13.0-bin
 cd bin
 ssh root@149.102.155.141
-------------------------------------------------------------------
Microsoft Windows [Version 10.0.19043.1586]
(c) Microsoft Corporation. All rights reserved.

C:\Users\user>cd C:\apache-ignite-slim-2.13.0-bin

C:\apache-ignite-slim-2.13.0-bin>cd apache-ignite-slim-2.13.0-bin

C:\apache-ignite-slim-2.13.0-bin\apache-ignite-slim-2.13.0-bin>cd bin

C:\apache-ignite-slim-2.13.0-bin\apache-ignite-slim-2.13.0-bin\bin>ssh root@149.102.155.141
root@149.102.155.141's password:
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-105-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
  _____
 / ___/___  _  _ _____ _   ___  ___
| |   / _ \| \| |_   _/ \ | _ )/ _ \
| |__| (_) | .` | | |/ _ \| _ \ (_) |
 \____\___/|_|\_| |_/_/ \_|___/\___/

Welcome!
 
2- Connectez-vous au site2 depuis votre machine en utilisant l’outils sqlline et exécuter les
requêtes suivantes:
CREATE TABLE City (id LONG PRIMARY KEY, name VARCHAR) WITH
"template=replicated";
INSERT INTO City (id, name) VALUES (1, 'Porto Novo');
INSERT INTO City (id, name) VALUES (2, 'Come');
INSERT INTO City (id, name) VALUES (3, 'Ouidah');
SELECT * FROM City;
Dire ce que vous constatez.

Sur le site 1 il faut faire:

  cd C:\apache-ignite-slim-2.13.0-bin
 cd apache-ignite-slim-2.13.0-bin
 cd bin
 ignite.bat ..\examples\config\example-ignite.xml
 sqlline -u jdbc:ignite:thin://149.102.155.141
 username:--- 
 password: ---
 Les requetes: CREATE TABLE City (id LONG PRIMARY KEY, name VARCHAR) WITH
"template=replicated";
INSERT INTO City (id, name) VALUES (1, 'Porto Novo');
INSERT INTO City (id, name) VALUES (2, 'Come');
INSERT INTO City (id, name) VALUES (3, 'Ouidah');
SELECT * FROM City;

+----+------------+
| ID |    NAME    |
+----+------------+
| 1  | Porto Novo |
| 2  | Come       |
| 3  | Ouidah     |
+----+------------+

constat: On constatte que la table City est cree avec les 3 valeurs.

3- Stopper le noeud serveur du site1 (CTRL+C) et exécuter la requête depuis l’interface de
sqlline:
SELECT * FROM City;
Dire ce que vous constatez.

a- Aller au niveau de l'interface de la creation du nooeud serveur (site 1) et faire <<!exit>> puis <<ctrl C>> 
b- Aller au niveau du cmd de la question 3 puis inserrer ceci:
 SELECT * FROM City;

+----+------------+
| ID |    NAME    |
+----+------------+
| 1  | Porto Novo |
| 2  | Come       |
| 3  | Ouidah     |
+----+------------+

constat: Il y a transfert de donnes(replication) du site 2 vers le site 1

4- Redémarrer le noeud serveur sur le site1, puis exécuter de nouveau depuis l’interface de
sqlline la requête suivante:
SELECT * FROM City;
Dire ce que vous constater.

Apres redemarrage on constate que la table reussit toujours que la repliquation fonctionne.


5- Depuis l’interface sqlline exécuter les requêtes suivantes:
CREATE TABLE Person (id LONG, name VARCHAR, city_id LONG, PRIMARY KEY (id,
city_id)) WITH "backups=1, affinityKey=city_id";
INSERT INTO Person (id, name, city_id) VALUES (1, 'John Koffi', 3);
INSERT INTO Person (id, name, city_id) VALUES (2, 'Jane GBOBA', 2);
INSERT INTO Person (id, name, city_id) VALUES (3, 'Mary AFFI', 1);
INSERT INTO Person (id, name, city_id) VALUES (4, 'Richard TOGODO', 2);
SELECT * FROM PERSON;

Toujours dans l'interface sqlline executer ces requetes :

CREATE TABLE Person (id LONG, name VARCHAR, city_id LONG, PRIMARY KEY (id,
city_id)) WITH "backups=1, affinityKey=city_id";
INSERT INTO Person (id, name, city_id) VALUES (1, 'John Koffi', 3);
INSERT INTO Person (id, name, city_id) VALUES (2, 'Jane GBOBA', 2);
INSERT INTO Person (id, name, city_id) VALUES (3, 'Mary AFFI', 1);
INSERT INTO Person (id, name, city_id) VALUES (4, 'Richard TOGODO', 2);
SELECT * FROM PERSON;

+----+----------------+---------+
| ID |      NAME      | CITY_ID |
+----+----------------+---------+
| 1  | John Koffi     | 3       |
| 2  | Jane GBOBA     | 2       |
| 3  | Mary AFFI      | 1       |
| 4  | Richard TOGODO | 2       |
+----+----------------+---------+


6- Stopper le noeud client du site2 (CTRL+C) et exécuter la requête depuis l’interface de
sqlline:
SELECT * FROM PERSON;
Dire ce que vous constater.
Tirer une conclusion.

a- Aller au niveau de l'interface de la creation du noeud client (site 2) et faire <<!exit>> puis <<ctrl C>> 
b- Aller au niveau du cmd de la question 3 puis inserrer ceci:
SELECT * FROM PERSON;

constat: la table a ete replique

conclusion: il s'agit d'un systeme de base de donnes distribues.

