.. _docker_documentation:

====================
Docker documentation
====================

.. contents:: Table des matières
   :depth: 3
   :local:
   :backlinks: entry

----

.. _introduction:

1. Introduction
===============

L'isolation est un principe essentiel pour séparer les processus d'une machine et résoudre les conflits de dépendances. Elle garantit que les applications fonctionnent de manière identique entre les environnements de développement et de production.

.. _isolation-virtualisation:

1.1 Isolation par virtualisation
---------------------------------

La virtualisation est une technique permettant de créer un environnement indépendant qui masque la machine hôte. On distingue trois grandes catégories :

- **Machines virtuelles** : émulent une machine complète, mais sont gourmandes en ressources.
- **Conteneurs (ex. Docker)** : plus légers, ils isolent les processus sans embarquer de système d'exploitation.
- **Environnements virtuels Python** : isolent les dépendances pour différentes applications sur une même machine.

La virtualisation permet non seulement d'isoler les processus, mais aussi de reproduire facilement des environnements identiques (ex. images Docker, fichiers ``requirements.txt``). Elle est donc essentielle pour la cohérence et l'efficacité en développement et en production.

----

.. _containerisation:

2. Containerisation
===================

Docker est un outil majeur de containerisation, permettant d'isoler des processus dans des conteneurs légers, sans système d'exploitation embarqué, ce qui optimise les ressources. À la différence des machines virtuelles, les conteneurs utilisent le noyau du système hôte, réduisant ainsi la consommation de ressources.

.. _conteneurs:

2.1 Les conteneurs
------------------

- Les conteneurs contiennent des applications, des bibliothèques et les exécutables nécessaires, tout en isolant leur fonctionnement.
- Ils limitent l'accès aux ressources système (fichiers, réseau, utilisateurs) et assignent des ressources spécifiques (CPU, mémoire).
- Les conteneurs fonctionnent nativement sous Linux, mais nécessitent une couche de virtualisation sur Windows.

.. _images-conteneurs:

2.2 Images de conteneurs
-------------------------

- Une image est un modèle préconfiguré pour créer des conteneurs.
- Les images sont construites de manière hiérarchique pour mutualiser les ressources (ex. : distribution Linux commune).
- Elles peuvent être modifiées et instanciées à volonté.

.. _docker-presentation:

2.3 Docker
----------

- Docker, basé sur **containerd**, a démocratisé la containerisation grâce à sa simplicité et son répertoire **DockerHub**, contenant des millions d'images officielles.
- Les utilisateurs peuvent créer et partager des images rapidement via des ``Dockerfile``.
- Docker se déploie facilement sur des services cloud via des solutions PaaS.

.. _architecture-docker:

2.4 Architecture de Docker
---------------------------

Docker fonctionne selon un modèle **client-serveur** :

- Le client Docker interagit avec le **démon Docker** (``dockerd``) via une API REST.
- Les **Docker registries** (comme DockerHub) servent de répertoires d'images, publics ou privés.

----

.. _interaction-docker:

3. Interaction avec Docker
==========================

.. _demarrage-docker:

3.1 Démarrage de Docker
------------------------

Démarrer le service Docker :

.. code-block:: bash

   sudo service docker start

Lancer un premier conteneur :

.. code-block:: bash

   docker run hello-world

**Processus :**

1. Le client CLI contacte le démon Docker pour créer un conteneur basé sur l'image spécifiée.
2. Si l'image n'est pas disponible localement, Docker la télécharge depuis DockerHub.
3. Une fois le conteneur exécuté, il s'arrête automatiquement après avoir produit une sortie.

.. _gestion-images-conteneurs:

3.2 Gestion des images et des conteneurs
-----------------------------------------

**Lister les images locales :**

.. code-block:: bash

   docker image ls

Informations affichées : nom du dépôt (``REPOSITORY``), tag (``TAG``), identifiant (``IMAGE ID``), date de création (``CREATED``) et taille (``SIZE``).

**Télécharger une image spécifique :**

.. code-block:: bash

   docker image pull ubuntu:latest

**Lancer un conteneur interactif :**

.. code-block:: bash

   docker run -it ubuntu:latest

.. _interaction-conteneurs:

3.3 Interaction avec les conteneurs
-------------------------------------

**Lister les conteneurs actifs :**

.. code-block:: bash

   docker ls

**Lister tous les conteneurs (actifs et stoppés) :**

.. code-block:: bash

   docker ls --all

**Relancer un conteneur arrêté (avec affichage des sorties) :**

.. code-block:: bash

   docker start -a <container_name_or_id>

**Arrêter un conteneur en fonctionnement :**

.. code-block:: bash

   docker stop <container_name_or_id>

**Supprimer un conteneur arrêté :**

.. code-block:: bash

   docker rm <container_name_or_id>

.. _modes-lancement:

3.4 Modes de lancement des conteneurs
---------------------------------------

**Nommer un conteneur :**

.. code-block:: bash

   docker run --name my_container ubuntu:latest

**Lancer un conteneur en arrière-plan (mode détaché) :**

.. code-block:: bash

   docker run -it --detach --name my_ubuntu ubuntu:latest bash

.. note::

   - Une image Docker peut être identifiée par un nom, un tag ou un ID unique (ex. ``hello-world:latest``).
   - L'option ``-it`` permet d'interagir avec un conteneur via une console (mode interactif).
   - Une image téléchargée localement est réutilisable sans nouveau téléchargement.

----

.. _gestion-suppression:

4. Gestion avancée des conteneurs
===================================

.. _arret-suppression:

4.1 Arrêt et suppression
--------------------------

**Arrêter puis supprimer un conteneur :**

.. code-block:: bash

   docker stop my_ubuntu
   docker rm my_ubuntu

**Lancer un conteneur avec suppression automatique à l'arrêt (option ``--rm``) :**

.. code-block:: bash

   docker run -it --rm --detach --name my_ubuntu ubuntu:latest bash

.. _variables-environnement:

4.2 Variables d'environnement
------------------------------

**Définir une variable d'environnement dans un conteneur :**

.. code-block:: bash

   docker run -it --rm --name my_ubuntu -e "ma_variable='bonjour le monde'" ubuntu:latest bash

**Vérifier la variable depuis l'intérieur du conteneur :**

.. code-block:: bash

   echo $ma_variable

**Résultat attendu :**

.. code-block:: text

   bonjour le monde

.. note::

   Après avoir quitté le conteneur (``exit``), celui-ci est automatiquement supprimé si l'option ``--rm`` a été utilisée.

.. _recap-arguments:

4.3 Récapitulatif des arguments
---------------------------------

.. list-table:: Arguments Docker courants
   :widths: 15 85
   :header-rows: 1

   * - Argument
     - Utilisation
   * - ``-it``
     - Permet d'interagir avec le conteneur via la commande passée (mode interactif + TTY).
   * - ``-d``
     - Lance le conteneur en arrière-plan (mode détaché).
   * - ``--name``
     - Attribue un nom spécifique au conteneur.
   * - ``--rm``
     - Supprime automatiquement le conteneur après son arrêt.
   * - ``-e``
     - Définit une ou plusieurs variables d'environnement dans le conteneur.

.. _inspection:

4.4 Inspection des conteneurs
-------------------------------

**Inspecter un conteneur :**

.. code-block:: bash

   docker inspect my_ubuntu

La sortie est au format JSON et inclut des informations comme l'état (``State``), le nom, l'identifiant (``ID``) et les détails sur l'image utilisée.

**Inspecter une image :**

.. code-block:: bash

   docker image inspect <nom_de_l_image>

.. note::

   Si vous inspectez un conteneur arrêté, l'attribut ``State`` dans la sortie JSON reflète ce changement d'état.

----

.. _communication-http:

5. Communication avec les conteneurs via HTTP
==============================================

La virtualisation isole les processus, mais cette isolation doit permettre des communications contrôlées. L'exemple ci-dessous utilise **Elasticsearch**, accessible via une API HTTP.

.. _lancement-elasticsearch:

5.1 Lancement d'un conteneur Elasticsearch
--------------------------------------------

**Télécharger l'image :**

.. code-block:: bash

   docker image pull elasticsearch:7.2.0

**Lancer le conteneur :**

.. code-block:: bash

   docker run -d --rm \
     -e "discovery.type=single-node" \
     -p 9200:9200 \
     --name my_es_container \
     elasticsearch:7.2.0

.. _acces-api:

5.2 Accès à l'API Elasticsearch
---------------------------------

Elasticsearch expose une API sur le **port 9200**. Cependant, un conteneur possède sa propre adresse IP, différente de celle de la machine hôte.

.. warning::

   Une requête directe sur ``localhost`` échouera, car le conteneur a son propre réseau :

   .. code-block:: bash

      curl -X GET -i http://localhost:9200  # Échec

**Trouver l'adresse IP du conteneur :**

.. code-block:: bash

   docker inspect my_es_container | grep IPAddress

Exemple d'adresse IP attribuée : ``172.17.0.2``

**Accéder à l'API via l'IP du conteneur :**

.. code-block:: bash

   curl -X GET -i http://172.17.0.2:9200

.. note::

   Le conteneur se comporte comme une machine virtuelle : il possède ses propres ports, son propre réseau localhost (``127.0.0.1``) et ses propres adresses IP locales.

   .. admonition:: Correction importante

      Dans le document source, l'adresse interne d'Elasticsearch dans le conteneur est mentionnée comme ``127.0.0.2:9200``, ce qui est incorrect. L'adresse de loopback à l'intérieur d'un conteneur est toujours ``127.0.0.1:9200``. L'adresse ``172.17.0.2`` est l'IP assignée au conteneur sur le réseau bridge Docker, visible depuis l'hôte.

----

.. _reseaux-docker:

6. Réseaux Docker
==================

.. _redirection-ports:

6.1 Redirection des ports
--------------------------

L'option ``-p`` permet de mapper les ports d'un conteneur vers ceux de la machine hôte.

**Exemple :** ``-p 9201:9200`` mappe le port ``9200`` du conteneur vers le port ``9201`` sur la machine hôte.

.. code-block:: bash

   docker run -d --rm \
     -e "discovery.type=single-node" \
     -p 9201:9200 \
     -p 9301:9300 \
     --name my_es_container \
     elasticsearch:7.2.0

**Tester l'accès :**

.. code-block:: bash

   curl -X GET -i http://127.0.0.1:9201

.. _communications-conteneurs:

6.2 Communications entre conteneurs
-------------------------------------

Lors de communications entre conteneurs, les adresses suivantes se comportent différemment :

- ``127.0.0.1:9200`` : refusé depuis un autre conteneur (adresse interne au conteneur Elasticsearch).
- ``127.0.0.1:9201`` : refusé depuis un autre conteneur (port mappé sur la machine hôte, non accessible entre conteneurs).
- ``172.17.0.2:9200`` : **fonctionne** (adresse IP réelle du conteneur sur le réseau bridge).

.. _reseaux-types:

6.3 Types de réseaux Docker
-----------------------------

**Lister les réseaux disponibles :**

.. code-block:: bash

   docker network ls

**Inspecter un réseau :**

.. code-block:: bash

   docker network inspect <nom_du_réseau>

**Réseau bridge (par défaut)**

- Adresses attribuées au format ``172.17.X.X`` avec un sous-réseau ``172.17.0.0/16``.
- Les conteneurs du même réseau peuvent communiquer entre eux.
- Les conteneurs de réseaux différents ne peuvent pas se connecter directement.

**Création d'un réseau personnalisé :**

.. code-block:: bash

   docker network create --subnet 172.50.0.0/16 --gateway 172.50.0.1 my_network

**Attacher un conteneur à un réseau spécifique :**

.. code-block:: bash

   docker run -d --rm \
     --network my_network \
     --name my_es_container3 \
     elasticsearch:7.2.0

.. note::

   Dans un réseau personnalisé, les conteneurs peuvent se contacter par leur **nom** (utilisé comme nom de domaine DNS interne), ce qui est préférable à l'utilisation des adresses IP.

**Réseau host**

Ce mode supprime les frontières réseau entre le conteneur et la machine hôte. Le conteneur utilise directement les ports de la machine hôte (``127.0.0.1``).

.. code-block:: bash

   docker run -d --rm \
     --network host \
     --name my_es_container \
     elasticsearch:7.2.0

.. warning::

   Le mode ``host`` peut poser des problèmes de sécurité car il supprime l'isolation réseau du conteneur.

----

.. _persistance-donnees:

7. Persistance des données (Volumes)
=====================================

.. _probleme-ephemere:

7.1 Problème initial : données éphémères
-----------------------------------------

Par défaut, Docker ne conserve pas les données créées à l'intérieur d'un conteneur une fois celui-ci arrêté. Exemple : un fichier ``/home/test.txt`` créé dans un conteneur Ubuntu disparaît après son arrêt et redémarrage.

.. _solution-volumes:

7.2 Solution recommandée : les volumes Docker
----------------------------------------------

Les volumes Docker permettent de rendre les données persistantes en les stockant dans des répertoires sur la machine hôte.

**a) Créer un volume :**

.. code-block:: bash

   docker volume create --name my_volume

**Lister les volumes :**

.. code-block:: bash

   docker volume ls

**Inspecter un volume :**

.. code-block:: bash

   docker volume inspect my_volume

**b) Monter un volume sur un conteneur :**

.. code-block:: bash

   docker run -it --name my_ubuntu \
     --mount type=volume,src=my_volume,dst=/home/my_folder \
     --rm \
     ubuntu:latest bash

Une fois dans le conteneur, le dossier ``/home/my_folder`` correspond au volume monté.

**c) Vérifier la persistance des données :**

Les fichiers créés dans le volume sont accessibles depuis la machine hôte à :

.. code-block:: text

   /var/lib/docker/volumes/my_volume/_data

**d) Partage d'un volume entre plusieurs conteneurs :**

.. code-block:: bash

   docker run -it --name my_ubuntu1 \
     --mount type=volume,src=my_volume,dst=/home/my_folder1 \
     --rm \
     ubuntu:latest bash

.. code-block:: bash

   docker run -it --name my_ubuntu2 \
     --mount type=volume,src=my_volume,dst=/home/my_folder2 \
     --rm \
     ubuntu:latest bash

Les deux conteneurs partagent les mêmes fichiers via le volume ``my_volume``.

**e) Supprimer un volume :**

.. code-block:: bash

   docker volume rm my_volume

.. _montage-dossier-hote:

7.3 Alternative : montage d'un dossier de la machine hôte
-----------------------------------------------------------

L'option ``--volume`` permet de mapper directement un dossier de la machine hôte dans le conteneur :

.. code-block:: bash

   docker run -it --name my_ubuntu \
     --volume $HOME:/home/my_folder \
     --rm \
     ubuntu:latest bash

.. warning::

   Cette méthode est pratique pour partager des fichiers sans droits administratifs, mais elle est **moins sécurisée** que l'utilisation de volumes nommés.

----

.. _creation-image:

8. Création d'une image Docker (Dockerfile)
============================================

.. _creer-image:

8.1 Créer une image
--------------------

Pour créer une image Docker :

1. Créer un fichier ``Dockerfile`` dans un dossier.
2. Exécuter la commande ``docker image build`` dans ce dossier.

**Exemple : créer un serveur Flask**

Créer le dossier de travail :

.. code-block:: bash

   mkdir my_docker_image
   cd my_docker_image
   nano Dockerfile

Contenu du ``Dockerfile`` :

.. code-block:: dockerfile

   FROM ubuntu:18.04

   RUN apt-get update && apt-get install python3-pip -y && pip3 install flask==2.0.0

   ADD server.py /my_server/server.py

   WORKDIR /my_server/

   EXPOSE 5000

   CMD python3 server.py

Contenu du fichier ``server.py`` :

.. code-block:: python

   from flask import Flask

   server = Flask('my_server')

   @server.route('/')
   def index():
       return 'Hello World from a containerized server'

   if __name__ == '__main__':
       server.run(host='0.0.0.0', port=5000)

**Construire l'image :**

.. code-block:: bash

   docker image build . -t my_image:latest

.. _verification-lancement:

8.2 Vérification et lancement de l'image
-----------------------------------------

**Lister les images locales :**

.. code-block:: bash

   docker image ls

**Lancer le conteneur avec mappage de ports en mode détaché :**

.. code-block:: bash

   docker run -p 5000:5000 -d my_image:latest

**Tester le serveur :**

.. code-block:: bash

   curl -X GET -i http://localhost:5000

.. _commandes-dockerfile:

8.3 Commandes Dockerfile essentielles
---------------------------------------

.. list-table:: Instructions Dockerfile
   :widths: 20 80
   :header-rows: 1

   * - Instruction
     - Utilisation
   * - ``FROM``
     - Choisir une image de base.
   * - ``RUN``
     - Exécuter une commande Bash pour configurer l'image.
   * - ``ADD``
     - Copier un fichier local vers l'image.
   * - ``WORKDIR``
     - Changer le répertoire courant dans l'image.
   * - ``EXPOSE``
     - Exposer un port du conteneur.
   * - ``CMD``
     - Définir la commande exécutée au démarrage du conteneur.
   * - ``ENTRYPOINT``
     - Définir le point d'entrée du conteneur (non surchargeable par défaut).
   * - ``ENV``
     - Déclarer des variables d'environnement.
   * - ``VOLUME``
     - Créer des points de montage pour les volumes.
   * - ``LABEL``
     - Ajouter des métadonnées à l'image.

.. _partage-images:

8.4 Partage d'images Docker
-----------------------------

**a) Via une archive tar :**

.. code-block:: bash

   # Sauvegarder l'image
   docker image save --output my_docker_image.tar my_image

   # Supprimer l'image locale
   docker image rm -f my_image

   # Restaurer l'image depuis l'archive
   docker image load --input my_docker_image.tar

**b) Via DockerHub :**

.. code-block:: bash

   # Se connecter à DockerHub
   docker login

   # Pousser l'image (format : username/imagename:tag)
   docker image push username/imagename:tag

   # Télécharger l'image depuis DockerHub
   docker image pull username/imagename:tag

----

.. _docker-compose:

9. Docker Compose : lancement simultané de conteneurs
======================================================

Docker Compose est un outil permettant de lancer facilement plusieurs conteneurs sur une seule machine. Cela est particulièrement utile pour tester des environnements où plusieurs services doivent interagir (ex. Elasticsearch et Jupyter Notebook).

.. _fichier-compose:

9.1 Fichier docker-compose.yml
--------------------------------

Le fichier ``docker-compose.yml`` définit les services (conteneurs) à créer.

**Exemple complet avec Jupyter et Elasticsearch :**

.. code-block:: yaml

   version: "3.9"
   services:
     jupyter:
       image: jupyter/minimal-notebook:ubuntu-18.04
       container_name: my_jupyter_from_compose
       ports:
         - "4444:8888"
       volumes:
         - .:/home/jovyan/work
       networks:
         - my_network_from_compose
       environment:
         JUPYTER_TOKEN: bonjour

     elasticsearch:
       image: elasticsearch:7.2.0
       container_name: my_es_from_compose
       ports:
         - "9200:9200"
         - "9300:9300"
       networks:
         - my_network_from_compose
       environment:
         discovery.type: single-node

   networks:
     my_network_from_compose:

.. note::

   Le port ``8888`` de Jupyter est mappé vers le port ``4444`` de la machine hôte. L'interface graphique de Jupyter est alors accessible à l'adresse ``http://localhost:4444``.

.. _commandes-compose:

9.2 Commandes Docker Compose
------------------------------

**Lancer tous les conteneurs :**

.. code-block:: bash

   docker-compose up

**Lister les conteneurs actifs :**

.. code-block:: bash

   docker ps

**Arrêter les conteneurs :**

.. code-block:: bash

   # Via le terminal (si lancé en avant-plan)
   CTRL + C

   # Ou via la commande
   docker-compose down

.. _config-avancee-compose:

9.3 Configuration avancée
---------------------------

- **Réseaux personnalisés** : les conteneurs peuvent être placés sur un réseau dédié (``my_network_from_compose``), ce qui leur permet de communiquer par nom.
- **Noms des conteneurs** : utilisez ``container_name`` pour nommer explicitement chaque service.
- **Volumes** : liez un dossier local à un dossier dans le conteneur via la clé ``volumes``.

.. note::

   Docker Compose est idéal pour les déploiements locaux sur une seule machine. Pour des déploiements distribués sur plusieurs machines, des outils comme **Docker Swarm** ou **Kubernetes** sont recommandés.

----

.. _clients-docker:

10. Clients Docker
==================

Plusieurs clients permettent d'interagir avec Docker, en complément de la CLI.

.. _portainer:

10.1 Portainer
--------------

Portainer est une interface graphique open source pour Docker, qui s'exécute elle-même comme un conteneur.

**Lancer Portainer :**

.. code-block:: bash

   docker volume create portainer_data

   docker run -d \
     -p 8000:8000 \
     -p 9000:9000 \
     --name=portainer \
     --restart=always \
     -v /var/run/docker.sock:/var/run/docker.sock \
     -v portainer_data:/data \
     portainer/portainer-ce

**Accès :** connectez-vous via ``http://localhost:9000`` pour accéder à l'interface graphique et gérer vos conteneurs.

.. _client-python:

10.2 Client Python
-------------------

Python peut interagir directement avec Docker grâce à la bibliothèque ``docker``.

**Installation :**

.. code-block:: bash

   pip3 install docker

**Exemple d'utilisation :**

.. code-block:: python

   import docker

   client = docker.from_env()

   # Lister les conteneurs actifs
   for container in client.containers.list():
       print(container.name, container.status)

   # Lancer un nouveau conteneur
   container = client.containers.run(
       "ubuntu:latest",
       command="echo 'Hello from Python Docker client'",
       remove=True
   )
   print(container.decode("utf-8"))

----

.. _conclusion:

11. Conclusion
==============

Docker est un écosystème complet pour la gestion des conteneurs, couvrant :

- **L'isolation** des processus sans machine virtuelle complète.
- **La portabilité** grâce aux images reproductibles (Dockerfile, DockerHub).
- **La persistance des données** via les volumes.
- **La communication réseau** entre conteneurs (bridge, host, réseaux personnalisés).
- **L'orchestration locale** avec Docker Compose.
- **La flexibilité de gestion** via la CLI, Portainer ou le client Python.

Pour des environnements distribués à grande échelle, **Docker Swarm** et **Kubernetes** constituent la suite naturelle de Docker Compose.



12. Publier une image Docker sur GitHub Container Registry (ghcr.io)
====================================================================

Prérequis : Créer un token d'accès personnel (PAT)
---------------------------------------------------

Créer un token d'accès personnel avec les scopes appropriés en cochant les options ``read:packages`` et ``write:packages``.
Ce token servira à l'authentification pour l'accès au registre GitHub.

**Chemin :** Settings > Developer settings > Personal access tokens > Tokens (classic)

.. warning::
   Ne partagez jamais votre token PAT. Utilisez un placeholder dans votre documentation.
   Exemple de format : ``ghp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX``


Étapes
------

12.1 Construire l'image Docker
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   docker image build . -t repo_test:1.0.0

Vérification (optionnel) :

.. code-block:: bash

   docker image ls


12.2 Lancer le container (optionnel)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   docker run repo_test:1.0.0


12.3 S'authentifier sur le registre ghcr.io
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   docker login ghcr.io -u <votre_username_github> -p <votre_token_PAT>


12.4 Tagger l'image pour le registre
------------------------------------

Le nom d'utilisateur doit être en **minuscules uniquement**.

.. code-block:: bash

   docker tag repo_test:1.0.0 ghcr.io/<votre_username_github>/repo_test:latest

Vérification (optionnel) :

.. code-block:: bash

   docker images


12.5 Inspecter l'image
----------------------

Vérifier la présence des deux tags sur l'image :

.. code-block:: bash

   docker image inspect repo_test:1.0.0


12.6 Pousser l'image sur le registre
------------------------------------

.. code-block:: bash

   docker push ghcr.io/<votre_username_github>/repo_test:latest


12.7 Synchronisation du registre avec le repo GitHub
----------------------------------------------------

A. Ouvrez un navigateur et accédez à `https://ghcr.io <https://ghcr.io>`_ pour accéder au GitHub Container Registry.
B. Dans la barre de recherche, tapez le nom du package (ex : ``repo_test``) pour vérifier que l'image a bien été poussée sur le registre.
C. Cliquez sur **"Connect Repository"** pour lier le package à votre repo GitHub.

.. note::
   Une fois le package lié, il apparaîtra directement dans la page de votre repo GitHub sous la section **"Packages"**.