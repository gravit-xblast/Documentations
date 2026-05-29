Clonage dépôt en sécurité avec SSH
====================================

.. contents:: Table des matières
   :depth: 2
   :local:

Préparation de l'environnement
-------------------------------

.. rubric:: Étape 1 — Créer un nouveau repository sur GitHub

Créer un nouveau repository depuis l'interface GitHub avant de procéder aux étapes suivantes.

Génération de la clé SSH
-------------------------

.. rubric:: Étape 2 — Générer la clé SSH

Dans le terminal, lancer la commande suivante :

.. code-block:: bash

   ssh-keygen -t ed25519 -C "email@exemple.com"

.. note::

   - Renommer la clé ``id_ed25519`` avec un nom explicite (exemple : ``key_test``).
   - La clé SSH sera générée dans le dossier caché ``~/.ssh`` :

     - Clé privée : ``~/.ssh/key_test``
     - Clé publique : ``~/.ssh/key_test.pub``

Enregistrement de la clé SSH dans GitHub
-----------------------------------------

.. rubric:: Étape 3 — Afficher la clé publique

.. code-block:: bash

   cat ~/.ssh/key_test.pub

.. rubric:: Étape 4 — Ajouter la clé dans GitHub

Dans GitHub, naviguer vers :

**Settings** > **SSH and GPG keys** > **New SSH key**

Puis coller le contenu de la clé publique dans le champ dédié.

Cloner le repository en local
-------------------------------

.. rubric:: Étape 5 — Cloner avec SSH

.. code-block:: bash

   git clone <URL_SSH>