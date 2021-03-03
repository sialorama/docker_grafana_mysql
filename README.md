# docker_grafana_mysql
Un conteneur pour Grafana - COVID-19 World Vaccination Progress 


{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <center>Un conteneur pour grafana</center>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## <center>COVID-19 World Vaccination Progress</center>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Données sur la vaccination:"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Source : https://www.kaggle.com/gpreda/covid-world-vaccination-progress"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Tutoriel Grafana : http://localhost:3000/tutorial"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Docker documentation : https://docs.docker.com/network/overlay/"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 1. Installation du réseau mynet:"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "docker network create mynet"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 2. Installation du container mysql_server:"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "docker run --net mynet --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=password -d mysql:5.7"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 3. Installation du container grafana/grafana:"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "docker run --net mynet --name grafana -p 3000:3000 grafana/grafana:latest"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 4. Lancer le container:"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "docker run --name=mysql -d mysql/mysql-server:5.7"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[\"'!!' n'est pas reconnu en tant que commande interne\",\n",
       " 'ou externe, un programme ex‚cutable ou un fichier de commandes.']"
      ]
     },
     "execution_count": 1,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "!!!! Noter le mot de pass généré par défaut"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Remplacer le mot de pass root du container mysql par 'admin':"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "ALTER USER 'root'@'localhost' IDENTIFIED BY 'admin';"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Afficher les containers existants : "
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "PS C:\\ia\\vaccination> docker ps\n",
    "    \n",
    "CONTAINER ID   IMAGE             COMMAND                  CREATED       STATUS       PORTS                    NAMES\n",
    "db3ebb67fd3d   mysql:5.7         \"docker-entrypoint.s…\"   3 hours ago   Up 3 hours   3306/tcp, 33060/tcp      mysql_server\n",
    "b3711af3cfe8   grafana/grafana   \"/run.sh\"                4 hours ago   Up 3 hours   0.0.0.0:3000->3000/tcp   grafana"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Interagir avec le shell du container mysql_server avec la commande exec:"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "docker exec -it mysql_server bash"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### 5. Se connecter au container mysql_server et charger la base de donnée:"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "docker exec -it mysql_server mysql -u root -p\n",
    "#mot de passe : admin"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Copier le fichier sql à la racine du container mysql_server:"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "powershell:> docker cp c:/ia/vaccination/vaccination.sql mysql_server:/"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Charger le fichier vaccination.sql dans le container mysql_server créé :"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "source vaccination.sql"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "Etape 1 : docker exec -it mysql_server bin/bash\n",
    "Etape 2 : mysql -u root -p\n",
    "    >>>\n",
    "Etape 3 : mysql> use vaccin\n",
    "    >>> Database changed\n",
    "        Etape 4 : mysql> source table.sql\n",
    "        Query OK, 0 rows affected, 1 warning (0.00 sec)\n",
    "\n",
    "        Query OK, 0 rows affected (0.00 sec)\n",
    "\n",
    "        Query OK, 0 rows affected (0.00 sec)\n",
    "\n",
    "        Query OK, 0 rows affected (0.00 sec)\n",
    "\n",
    "        Query OK, 0 rows affected (0.00 sec)\n",
    "\n",
    "        Query OK, 0 rows affected (0.00 sec)\n",
    "\n",
    "        Query OK, 0 rows affected (0.00 sec)\n",
    "\n",
    "        Query OK, 0 rows affected (0.01 sec)\n",
    "\n",
    "        Query OK, 0 rows affected (0.02 sec)\n",
    "\n",
    "        Query OK, 180 rows affected (0.02 sec)\n",
    "        Records: 180  Duplicates: 0  Warnings: 0\n",
    "        "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Gérer le stockage de données:\n",
    "Par défaut, Docker enregistre les données dans un volume interne.\n",
    "Pour vérifier l'emplacement du volume on utilise la commande:"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "docker inspect mysql_server"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Afficher les bases de données existantes : "
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "show databases;"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Créer une base de données (vaccin) :"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "CREATE DATABASE ma_base_de_données;"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Importer notre DB .sql (vaccination.sql)"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "source notre_DB.sql"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Utiliser la base de données : "
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "USE la_base_de_données"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Afficher les tables : "
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "show tables;"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Importer le ficher sql : "
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "source notre_DB.sql"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##### Afficher le port qu'utilise mysql : "
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "SHOW VARIABLES LIKE 'port';"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Lancer Grafana"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "lancer grafana dans le navigateur : localhost:3000/login\n",
    "id : admin et mot de passe : admin par défaut."
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "docker run -d --name mysql_server --network mynet -p 3306:3306 -e MYSQL_ROOT_PASSWORD=admin mysql:5.7"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "docker run -d --name grafana --network mynet -p 3000:3000 -v grafana_config:/etc/grafana grafana/grafana"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Etape 2 : Mettre le .sql dans le container"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "powershell:> docker cp c:/ia/vaccination/vaccination.sql mysql_server:/ #pour le mettre à la racine"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Snapshot du graphic"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "http://localhost:3000/d/yzO5besMzO/vaccinations?from=1609459200000&to=1614556800000&orgId=1"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "#### Sauvegarde du ficher json à importer pour restaurer les graphiques:"
   ]
  },
  {
   "cell_type": "raw",
   "metadata": {},
   "source": [
    "{\n",
    "  \"annotations\": {\n",
    "    \"list\": [\n",
    "      {\n",
    "        \"builtIn\": 1,\n",
    "        \"datasource\": \"-- Grafana --\",\n",
    "        \"enable\": true,\n",
    "        \"hide\": true,\n",
    "        \"iconColor\": \"rgba(0, 211, 255, 1)\",\n",
    "        \"name\": \"Annotations & Alerts\",\n",
    "        \"type\": \"dashboard\"\n",
    "      }\n",
    "    ]\n",
    "  },\n",
    "  \"editable\": true,\n",
    "  \"gnetId\": null,\n",
    "  \"graphTooltip\": 0,\n",
    "  \"id\": 2,\n",
    "  \"links\": [],\n",
    "  \"panels\": [\n",
    "    {\n",
    "      \"datasource\": null,\n",
    "      \"description\": \"\",\n",
    "      \"fieldConfig\": {\n",
    "        \"defaults\": {\n",
    "          \"color\": {\n",
    "            \"mode\": \"continuous-RdYlGr\"\n",
    "          },\n",
    "          \"custom\": {},\n",
    "          \"mappings\": [],\n",
    "          \"thresholds\": {\n",
    "            \"mode\": \"absolute\",\n",
    "            \"steps\": [\n",
    "              {\n",
    "                \"color\": \"green\",\n",
    "                \"value\": null\n",
    "              },\n",
    "              {\n",
    "                \"color\": \"red\",\n",
    "                \"value\": 80\n",
    "              }\n",
    "            ]\n",
    "          },\n",
    "          \"unit\": \"short\"\n",
    "        },\n",
    "        \"overrides\": []\n",
    "      },\n",
    "      \"gridPos\": {\n",
    "        \"h\": 9,\n",
    "        \"w\": 17,\n",
    "        \"x\": 4,\n",
    "        \"y\": 0\n",
    "      },\n",
    "      \"id\": 4,\n",
    "      \"options\": {\n",
    "        \"displayMode\": \"gradient\",\n",
    "        \"orientation\": \"horizontal\",\n",
    "        \"reduceOptions\": {\n",
    "          \"calcs\": [\n",
    "            \"lastNotNull\"\n",
    "          ],\n",
    "          \"fields\": \"\",\n",
    "          \"values\": false\n",
    "        },\n",
    "        \"showUnfilled\": true,\n",
    "        \"text\": {}\n",
    "      },\n",
    "      \"pluginVersion\": \"7.4.3\",\n",
    "      \"targets\": [\n",
    "        {\n",
    "          \"format\": \"time_series\",\n",
    "          \"group\": [],\n",
    "          \"metricColumn\": \"vaccines\",\n",
    "          \"rawQuery\": false,\n",
    "          \"rawSql\": \"SELECT\\n  date AS \\\"time\\\",\\n  vaccines AS metric,\\n  total_vaccinations\\nFROM country_vaccinations\\nORDER BY date\",\n",
    "          \"refId\": \"A\",\n",
    "          \"select\": [\n",
    "            [\n",
    "              {\n",
    "                \"params\": [\n",
    "                  \"total_vaccinations\"\n",
    "                ],\n",
    "                \"type\": \"column\"\n",
    "              }\n",
    "            ]\n",
    "          ],\n",
    "          \"table\": \"country_vaccinations\",\n",
    "          \"timeColumn\": \"date\",\n",
    "          \"timeColumnType\": \"date\",\n",
    "          \"where\": []\n",
    "        }\n",
    "      ],\n",
    "      \"timeFrom\": null,\n",
    "      \"timeShift\": null,\n",
    "      \"title\": \"Utilisation des vaccins disponibles\",\n",
    "      \"type\": \"bargauge\"\n",
    "    },\n",
    "    {\n",
    "      \"datasource\": null,\n",
    "      \"fieldConfig\": {\n",
    "        \"defaults\": {\n",
    "          \"color\": {\n",
    "            \"mode\": \"thresholds\"\n",
    "          },\n",
    "          \"custom\": {},\n",
    "          \"mappings\": [],\n",
    "          \"thresholds\": {\n",
    "            \"mode\": \"percentage\",\n",
    "            \"steps\": [\n",
    "              {\n",
    "                \"color\": \"green\",\n",
    "                \"value\": null\n",
    "              },\n",
    "              {\n",
    "                \"color\": \"#EAB839\",\n",
    "                \"value\": 0\n",
    "              },\n",
    "              {\n",
    "                \"color\": \"#6ED0E0\",\n",
    "                \"value\": 10\n",
    "              },\n",
    "              {\n",
    "                \"color\": \"#EF843C\",\n",
    "                \"value\": 20\n",
    "              },\n",
    "              {\n",
    "                \"color\": \"#E24D42\",\n",
    "                \"value\": 30\n",
    "              }\n",
    "            ]\n",
    "          }\n",
    "        },\n",
    "        \"overrides\": []\n",
    "      },\n",
    "      \"gridPos\": {\n",
    "        \"h\": 12,\n",
    "        \"w\": 12,\n",
    "        \"x\": 0,\n",
    "        \"y\": 9\n",
    "      },\n",
    "      \"id\": 8,\n",
    "      \"options\": {\n",
    "        \"displayMode\": \"lcd\",\n",
    "        \"orientation\": \"horizontal\",\n",
    "        \"reduceOptions\": {\n",
    "          \"calcs\": [\n",
    "            \"lastNotNull\"\n",
    "          ],\n",
    "          \"fields\": \"\",\n",
    "          \"values\": false\n",
    "        },\n",
    "        \"showUnfilled\": true,\n",
    "        \"text\": {}\n",
    "      },\n",
    "      \"pluginVersion\": \"7.4.3\",\n",
    "      \"targets\": [\n",
    "        {\n",
    "          \"format\": \"time_series\",\n",
    "          \"group\": [],\n",
    "          \"metricColumn\": \"country\",\n",
    "          \"rawQuery\": false,\n",
    "          \"rawSql\": \"SELECT\\n  date AS \\\"time\\\",\\n  country AS metric,\\n  people_fully_vaccinated_per_hundred\\nFROM country_vaccinations\\nORDER BY date\",\n",
    "          \"refId\": \"A\",\n",
    "          \"select\": [\n",
    "            [\n",
    "              {\n",
    "                \"params\": [\n",
    "                  \"people_fully_vaccinated_per_hundred\"\n",
    "                ],\n",
    "                \"type\": \"column\"\n",
    "              }\n",
    "            ]\n",
    "          ],\n",
    "          \"table\": \"country_vaccinations\",\n",
    "          \"timeColumn\": \"date\",\n",
    "          \"timeColumnType\": \"date\",\n",
    "          \"where\": []\n",
    "        }\n",
    "      ],\n",
    "      \"title\": \"Taux de vaccination de la population\",\n",
    "      \"transparent\": true,\n",
    "      \"type\": \"bargauge\"\n",
    "    },\n",
    "    {\n",
    "      \"aliasColors\": {},\n",
    "      \"bars\": false,\n",
    "      \"dashLength\": 10,\n",
    "      \"dashes\": false,\n",
    "      \"datasource\": null,\n",
    "      \"description\": \"\",\n",
    "      \"fieldConfig\": {\n",
    "        \"defaults\": {\n",
    "          \"color\": {},\n",
    "          \"custom\": {},\n",
    "          \"thresholds\": {\n",
    "            \"mode\": \"absolute\",\n",
    "            \"steps\": []\n",
    "          },\n",
    "          \"unit\": \"short\"\n",
    "        },\n",
    "        \"overrides\": []\n",
    "      },\n",
    "      \"fill\": 1,\n",
    "      \"fillGradient\": 0,\n",
    "      \"gridPos\": {\n",
    "        \"h\": 12,\n",
    "        \"w\": 12,\n",
    "        \"x\": 12,\n",
    "        \"y\": 9\n",
    "      },\n",
    "      \"hiddenSeries\": false,\n",
    "      \"id\": 6,\n",
    "      \"legend\": {\n",
    "        \"alignAsTable\": false,\n",
    "        \"avg\": false,\n",
    "        \"current\": false,\n",
    "        \"max\": false,\n",
    "        \"min\": false,\n",
    "        \"show\": true,\n",
    "        \"total\": false,\n",
    "        \"values\": false\n",
    "      },\n",
    "      \"lines\": true,\n",
    "      \"linewidth\": 1,\n",
    "      \"nullPointMode\": \"null\",\n",
    "      \"options\": {\n",
    "        \"alertThreshold\": true\n",
    "      },\n",
    "      \"percentage\": false,\n",
    "      \"pluginVersion\": \"7.4.3\",\n",
    "      \"pointradius\": 2,\n",
    "      \"points\": false,\n",
    "      \"renderer\": \"flot\",\n",
    "      \"seriesOverrides\": [],\n",
    "      \"spaceLength\": 10,\n",
    "      \"stack\": false,\n",
    "      \"steppedLine\": false,\n",
    "      \"targets\": [\n",
    "        {\n",
    "          \"format\": \"time_series\",\n",
    "          \"group\": [],\n",
    "          \"metricColumn\": \"country\",\n",
    "          \"rawQuery\": false,\n",
    "          \"rawSql\": \"SELECT\\n  date AS \\\"time\\\",\\n  country AS metric,\\n  people_fully_vaccinated\\nFROM country_vaccinations\\nORDER BY date\",\n",
    "          \"refId\": \"A\",\n",
    "          \"select\": [\n",
    "            [\n",
    "              {\n",
    "                \"params\": [\n",
    "                  \"people_fully_vaccinated\"\n",
    "                ],\n",
    "                \"type\": \"column\"\n",
    "              }\n",
    "            ]\n",
    "          ],\n",
    "          \"table\": \"country_vaccinations\",\n",
    "          \"timeColumn\": \"date\",\n",
    "          \"timeColumnType\": \"date\",\n",
    "          \"where\": []\n",
    "        }\n",
    "      ],\n",
    "      \"thresholds\": [],\n",
    "      \"timeFrom\": null,\n",
    "      \"timeRegions\": [],\n",
    "      \"timeShift\": null,\n",
    "      \"title\": \"Nombre de personnes vaccinées par pays\",\n",
    "      \"tooltip\": {\n",
    "        \"shared\": true,\n",
    "        \"sort\": 0,\n",
    "        \"value_type\": \"individual\"\n",
    "      },\n",
    "      \"type\": \"graph\",\n",
    "      \"xaxis\": {\n",
    "        \"buckets\": null,\n",
    "        \"mode\": \"time\",\n",
    "        \"name\": null,\n",
    "        \"show\": true,\n",
    "        \"values\": []\n",
    "      },\n",
    "      \"yaxes\": [\n",
    "        {\n",
    "          \"format\": \"short\",\n",
    "          \"label\": null,\n",
    "          \"logBase\": 1,\n",
    "          \"max\": null,\n",
    "          \"min\": null,\n",
    "          \"show\": true\n",
    "        },\n",
    "        {\n",
    "          \"format\": \"short\",\n",
    "          \"label\": null,\n",
    "          \"logBase\": 1,\n",
    "          \"max\": null,\n",
    "          \"min\": null,\n",
    "          \"show\": true\n",
    "        }\n",
    "      ],\n",
    "      \"yaxis\": {\n",
    "        \"align\": false,\n",
    "        \"alignLevel\": null\n",
    "      }\n",
    "    }\n",
    "  ],\n",
    "  \"refresh\": false,\n",
    "  \"schemaVersion\": 27,\n",
    "  \"style\": \"dark\",\n",
    "  \"tags\": [],\n",
    "  \"templating\": {\n",
    "    \"list\": []\n",
    "  },\n",
    "  \"time\": {\n",
    "    \"from\": \"2021-01-01T00:00:00.000Z\",\n",
    "    \"to\": \"2021-03-01T00:00:00.000Z\"\n",
    "  },\n",
    "  \"timepicker\": {},\n",
    "  \"timezone\": \"\",\n",
    "  \"title\": \"Vaccinations\",\n",
    "  \"uid\": \"yzO5besMzO\",\n",
    "  \"version\": 3\n",
    "}"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.8.8"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}
