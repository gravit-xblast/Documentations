FICHIERS DE CONFIGURATION (/.git/config, /.ssh/config)
======================================================


~/.ssh/config
==============

Ce fichier définit des alias d'hôtes SSH pour gérer plusieurs comptes GitHub avec des clés SSH différentes.

Exemple fichier ``~/.ssh/config`` :

.. code-block:: text

    Host github.com-quantum-rlk
    HostName github.com
    User git
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/novablast
    IdentitiesOnly yes

    Host github.com.mok
    HostName github.com
    User git
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_ed25519_repo_test
    IdentitiesOnly yes

Explications
------------

Bloc 1 — alias ``github.com-quantum-rlk``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: text

    Host github.com-quantum-rlk
    HostName github.com
    User git
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/novablast
    IdentitiesOnly yes

→ Correspond à un compte GitHub "quantum-rlk", authentifié avec la clé ``novablast``.

+-----------------------------------------+--------------------------------------------------------------+
| Directive                               | Description                                                  |
+=========================================+==============================================================+
| ``Host github.com-quantum-rlk``         | Nom de l'alias (inventé)                                     |
+-----------------------------------------+--------------------------------------------------------------+
| ``HostName github.com``                 | Vrai serveur contacté                                        |
+-----------------------------------------+--------------------------------------------------------------+
| ``User git``                            | L'utilisateur SSH est toujours ``git`` chez GitHub           |
+-----------------------------------------+--------------------------------------------------------------+
| ``PreferredAuthentications publickey``  | Force l'authentification par clé publique                    |
+-----------------------------------------+--------------------------------------------------------------+
| ``IdentityFile ~/.ssh/novablast``       | Clé privée SSH utilisée                                      |
+-----------------------------------------+--------------------------------------------------------------+
| ``IdentitiesOnly yes``                  | Utilise uniquement la clé spécifiée                          |
+-----------------------------------------+--------------------------------------------------------------+


Bloc 2 — alias ``github.com.mok``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: text

    Host github.com.mok
    HostName github.com
    User git
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_ed25519_repo_test
    IdentitiesOnly yes

→ Correspond à un compte GitHub "mok", authentifié avec la clé ``id_ed25519_repo_test``.

+--------------------------------------------------+--------------------------------------------------------------+
| Directive                                        | Description                                                  |
+==================================================+==============================================================+
| ``Host github.com.mok``                          | Nom de l'alias                                               |
+--------------------------------------------------+--------------------------------------------------------------+
| ``HostName github.com``                          | Vrai serveur contacté                                        |
+--------------------------------------------------+--------------------------------------------------------------+
| ``User git``                                     | L'utilisateur SSH est toujours ``git`` chez GitHub           |
+--------------------------------------------------+--------------------------------------------------------------+
| ``PreferredAuthentications publickey``           | Force l'authentification par clé publique                    |
+--------------------------------------------------+--------------------------------------------------------------+
| ``IdentityFile ~/.ssh/id_ed25519_repo_test``     | Clé privée SSH utilisée                                      |
+--------------------------------------------------+--------------------------------------------------------------+
| ``IdentitiesOnly yes``                           | Utilise uniquement la clé spécifiée                          |
+--------------------------------------------------+--------------------------------------------------------------+


~/.git/config
=============

C'est la configuration locale du dépôt Git.

Exemple fichier ``.git/config`` :

.. code-block:: ini

    [core]
            repositoryformatversion = 0
            filemode = true
            bare = false
            logallrefupdates = true

    [remote "origin"]
            url = git@github.com.mok:mokingJu/repo_test.git
            fetch = +refs/heads/*:refs/remotes/origin/*

    [branch "main"]
            remote = origin
            merge = refs/heads/main

[core] — Paramètres fondamentaux du dépôt
-----------------------------------------

``repositoryformatversion = 0``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
``0`` : Version du format interne de Git. ``0`` = format standard.
Git vérifie cette valeur au démarrage pour savoir s'il est capable de lire ce dépôt.


``filemode = true``
^^^^^^^^^^^^^^^^^^^
``true`` : Git surveille les permissions des fichiers (ex: ``chmod +x``).
Si un fichier devient exécutable, Git le détecte comme une modification.
Mettre ``false`` sur Windows où les permissions Unix n'existent pas.


``bare = false``
^^^^^^^^^^^^^^^^
``false`` : Ce dépôt n'est pas bare — il possède un répertoire de travail normal avec les fichiers.
Un dépôt ``bare = true`` n'a pas de working directory, c'est typiquement un dépôt serveur (ex: GitHub).


``logallrefupdates = true``
^^^^^^^^^^^^^^^^^^^^^^^^^^^
``true`` : Active le reflog — Git enregistre l'historique de tous les déplacements de branches et HEAD dans ``.git/logs/``.
Indispensable pour récupérer des commits "perdus" avec :

.. code-block:: bash

    git reflog


[remote "origin"] — Définition du dépôt distant nommé ``origin``
----------------------------------------------------------------

``url = git@github.com.mok:mokingJu/repo_test.git``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

L'URL SSH est l'adresse du dépôt distant.

Utilise l'alias SSH ``.mok`` (défini dans ``~/.ssh/config``) pour choisir la bonne clé.

Format :

.. code-block:: text

    git@<alias>:<user>/<repo>.git


``fetch = +refs/heads/*:refs/remotes/origin/*``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Le refspec est la règle de synchronisation.

Elle définit quoi rapatrier lors d'un ``git fetch`` :

- ``+`` = force la mise à jour même en cas de divergence
- ``refs/heads/*`` = toutes les branches distantes
- ``refs/remotes/origin/*`` = les stocke localement sous ``origin/``


[branch "main"] — Comportement de la branche locale ``main``
-------------------------------------------------------------

``remote = origin``
^^^^^^^^^^^^^^^^^^^

``origin`` indique que la branche ``main`` est liée au remote ``origin``.

Git sait où pousser/tirer par défaut sans préciser la cible.

Permet de faire :

.. code-block:: bash

    git push

sans écrire :

.. code-block:: bash

    git push origin main


``merge = refs/heads/main``
^^^^^^^^^^^^^^^^^^^^^^^^^^^

``refs/heads/main`` précise que ``main`` locale suit la branche ``main`` distante.

Utilisé par :

.. code-block:: bash

    git pull

pour savoir quelle branche merger automatiquement.