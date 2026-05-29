Utilisation du gestionnaire de package UV
==========================

Introduction
------------

UV est un gestionnaire Python ultra-rapide développé par Astral.
Il remplace plusieurs outils classiques comme :

- pip
- venv
- pip-tools
- virtualenv


Installation
------------

.. code-block:: bash

	curl -LsSf https://astral.sh/uv/install.sh | sh

Puis recharger le shell :

.. code-block:: bash

	source ~/.bashrc


Commande de base
----------------

Installation des packages Python :

.. code-block:: bash

	uv pip install pandas


Création et activation d'un environnement virtuel :

.. code-block:: bash

	uv venv
	source .venv/bin/activate


Gestion des dépendances :
 

Exemple 

.. code-block:: bash

	uv add fastapi
	uv add pandas


Gérer plusieurs versions Python : 
