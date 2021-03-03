# <center>Un conteneur pour grafana</center>

## <center>COVID-19 World Vaccination Progress</center>

#### Données sur la vaccination:

Source : https://www.kaggle.com/gpreda/covid-world-vaccination-progress

Tutoriel Grafana : http://localhost:3000/tutorial

Docker documentation : https://docs.docker.com/network/overlay/

#### 1. Installation du réseau mynet:
docker network create mynet
#### 2. Installation du container mysql_server:
docker run --net mynet --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password -d mysql:5.7
#### 3. Installation du container grafana/grafana:
docker run --net mynet --name grafana -p 3000:3000 grafana/grafana:latest
#### 4. Lancer le container:
docker run --name=mysql -d mysql/mysql-server:5.7

```python
!!!! Noter le mot de pass généré par défaut
```




    ["'!!' n'est pas reconnu en tant que commande interne",
     'ou externe, un programme ex‚cutable ou un fichier de commandes.']



##### Remplacer le mot de pass root du container mysql par 'admin':
ALTER USER 'root'@'localhost' IDENTIFIED BY 'admin';
##### Afficher les containers existants : 
PS C:\ia\vaccination> docker ps
    
CONTAINER ID   IMAGE             COMMAND                  CREATED       STATUS       PORTS                    NAMES
db3ebb67fd3d   mysql:5.7         "docker-entrypoint.s…"   3 hours ago   Up 3 hours   3306/tcp, 33060/tcp      mysql_server
b3711af3cfe8   grafana/grafana   "/run.sh"                4 hours ago   Up 3 hours   0.0.0.0:3000->3000/tcp   grafana
#### Interagir avec le shell du container mysql_server avec la commande exec:
docker exec -it mysql_server bash
#### 5. Se connecter au container mysql_server et charger la base de donnée:
docker exec -it mysql_server mysql -u root -p
#mot de passe : admin
##### Copier le fichier sql à la racine du container mysql_server:

powershell:> docker cp c:/ia/vaccination/vaccination.sql mysql_server:/

##### Charger le fichier vaccination.sql dans le container mysql_server créé :

source vaccination.sqlEtape 1 : docker exec -it mysql_server bin/bash
Etape 2 : mysql -u root -p
    >>>
Etape 3 : mysql> use vaccin
    >>> Database changed
        Etape 4 : mysql> source table.sql
        Query OK, 0 rows affected, 1 warning (0.00 sec)

        Query OK, 0 rows affected (0.00 sec)

        Query OK, 0 rows affected (0.00 sec)

        Query OK, 0 rows affected (0.00 sec)

        Query OK, 0 rows affected (0.00 sec)

        Query OK, 0 rows affected (0.00 sec)

        Query OK, 0 rows affected (0.00 sec)

        Query OK, 0 rows affected (0.01 sec)

        Query OK, 0 rows affected (0.02 sec)

        Query OK, 180 rows affected (0.02 sec)
        Records: 180  Duplicates: 0  Warnings: 0
        
##### Gérer le stockage de données:
Par défaut, Docker enregistre les données dans un volume interne.
Pour vérifier l'emplacement du volume on utilise la commande:
docker inspect mysql_server
##### Afficher les bases de données existantes : 
show databases;
##### Créer une base de données (vaccin) :
CREATE DATABASE ma_base_de_données;
##### Importer notre DB .sql (vaccination.sql)
source notre_DB.sql
##### Utiliser la base de données : 
USE la_base_de_données
##### Afficher les tables : 
show tables;
##### Importer le ficher sql : 
source notre_DB.sql
##### Afficher le port qu'utilise mysql : 
SHOW VARIABLES LIKE 'port';
## Lancer Grafana
lancer grafana dans le navigateur : 
localhost:3000/login

id : admin et mot de passe : admin par défaut.

docker run -d --name mysql_server --network mynet -p 3306:3306 -e MYSQL_ROOT_PASSWORD=admin mysql:5.7docker run -d --name grafana --network mynet -p 3000:3000 -v grafana_config:/etc/grafana grafana/grafana

#### Etape 2 : Mettre le .sql dans le container
powershell:> docker cp c:/ia/vaccination/vaccination.sql mysql_server:/ #pour le mettre à la racine
#### Snapshot du graphic
http://localhost:3000/d/yzO5besMzO/vaccinations?from=1609459200000&to=1614556800000&orgId=1
#### Sauvegarde du ficher json à importer pour restaurer les graphiques:
{
  "annotations": {
    "list": [
      {
        "builtIn": 1,
        "datasource": "-- Grafana --",
        "enable": true,
        "hide": true,
        "iconColor": "rgba(0, 211, 255, 1)",
        "name": "Annotations & Alerts",
        "type": "dashboard"
      }
    ]
  },
  "editable": true,
  "gnetId": null,
  "graphTooltip": 0,
  "id": 2,
  "links": [],
  "panels": [
    {
      "datasource": null,
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "continuous-RdYlGr"
          },
          "custom": {},
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": null
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "short"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 9,
        "w": 17,
        "x": 4,
        "y": 0
      },
      "id": 4,
      "options": {
        "displayMode": "gradient",
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showUnfilled": true,
        "text": {}
      },
      "pluginVersion": "7.4.3",
      "targets": [
        {
          "format": "time_series",
          "group": [],
          "metricColumn": "vaccines",
          "rawQuery": false,
          "rawSql": "SELECT\n  date AS \"time\",\n  vaccines AS metric,\n  total_vaccinations\nFROM country_vaccinations\nORDER BY date",
          "refId": "A",
          "select": [
            [
              {
                "params": [
                  "total_vaccinations"
                ],
                "type": "column"
              }
            ]
          ],
          "table": "country_vaccinations",
          "timeColumn": "date",
          "timeColumnType": "date",
          "where": []
        }
      ],
      "timeFrom": null,
      "timeShift": null,
      "title": "Utilisation des vaccins disponibles",
      "type": "bargauge"
    },
    {
      "datasource": null,
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "custom": {},
          "mappings": [],
          "thresholds": {
            "mode": "percentage",
            "steps": [
              {
                "color": "green",
                "value": null
              },
              {
                "color": "#EAB839",
                "value": 0
              },
              {
                "color": "#6ED0E0",
                "value": 10
              },
              {
                "color": "#EF843C",
                "value": 20
              },
              {
                "color": "#E24D42",
                "value": 30
              }
            ]
          }
        },
        "overrides": []
      },
      "gridPos": {
        "h": 12,
        "w": 12,
        "x": 0,
        "y": 9
      },
      "id": 8,
      "options": {
        "displayMode": "lcd",
        "orientation": "horizontal",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showUnfilled": true,
        "text": {}
      },
      "pluginVersion": "7.4.3",
      "targets": [
        {
          "format": "time_series",
          "group": [],
          "metricColumn": "country",
          "rawQuery": false,
          "rawSql": "SELECT\n  date AS \"time\",\n  country AS metric,\n  people_fully_vaccinated_per_hundred\nFROM country_vaccinations\nORDER BY date",
          "refId": "A",
          "select": [
            [
              {
                "params": [
                  "people_fully_vaccinated_per_hundred"
                ],
                "type": "column"
              }
            ]
          ],
          "table": "country_vaccinations",
          "timeColumn": "date",
          "timeColumnType": "date",
          "where": []
        }
      ],
      "title": "Taux de vaccination de la population",
      "transparent": true,
      "type": "bargauge"
    },
    {
      "aliasColors": {},
      "bars": false,
      "dashLength": 10,
      "dashes": false,
      "datasource": null,
      "description": "",
      "fieldConfig": {
        "defaults": {
          "color": {},
          "custom": {},
          "thresholds": {
            "mode": "absolute",
            "steps": []
          },
          "unit": "short"
        },
        "overrides": []
      },
      "fill": 1,
      "fillGradient": 0,
      "gridPos": {
        "h": 12,
        "w": 12,
        "x": 12,
        "y": 9
      },
      "hiddenSeries": false,
      "id": 6,
      "legend": {
        "alignAsTable": false,
        "avg": false,
        "current": false,
        "max": false,
        "min": false,
        "show": true,
        "total": false,
        "values": false
      },
      "lines": true,
      "linewidth": 1,
      "nullPointMode": "null",
      "options": {
        "alertThreshold": true
      },
      "percentage": false,
      "pluginVersion": "7.4.3",
      "pointradius": 2,
      "points": false,
      "renderer": "flot",
      "seriesOverrides": [],
      "spaceLength": 10,
      "stack": false,
      "steppedLine": false,
      "targets": [
        {
          "format": "time_series",
          "group": [],
          "metricColumn": "country",
          "rawQuery": false,
          "rawSql": "SELECT\n  date AS \"time\",\n  country AS metric,\n  people_fully_vaccinated\nFROM country_vaccinations\nORDER BY date",
          "refId": "A",
          "select": [
            [
              {
                "params": [
                  "people_fully_vaccinated"
                ],
                "type": "column"
              }
            ]
          ],
          "table": "country_vaccinations",
          "timeColumn": "date",
          "timeColumnType": "date",
          "where": []
        }
      ],
      "thresholds": [],
      "timeFrom": null,
      "timeRegions": [],
      "timeShift": null,
      "title": "Nombre de personnes vaccinées par pays",
      "tooltip": {
        "shared": true,
        "sort": 0,
        "value_type": "individual"
      },
      "type": "graph",
      "xaxis": {
        "buckets": null,
        "mode": "time",
        "name": null,
        "show": true,
        "values": []
      },
      "yaxes": [
        {
          "format": "short",
          "label": null,
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        },
        {
          "format": "short",
          "label": null,
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        }
      ],
      "yaxis": {
        "align": false,
        "alignLevel": null
      }
    }
  ],
  "refresh": false,
  "schemaVersion": 27,
  "style": "dark",
  "tags": [],
  "templating": {
    "list": []
  },
  "time": {
    "from": "2021-01-01T00:00:00.000Z",
    "to": "2021-03-01T00:00:00.000Z"
  },
  "timepicker": {},
  "timezone": "",
  "title": "Vaccinations",
  "uid": "yzO5besMzO",
  "version": 3
}
