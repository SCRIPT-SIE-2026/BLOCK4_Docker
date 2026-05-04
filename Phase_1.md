# Créer mon premier conteneur avec Docker

À la base d'un conteneur Docker, on trouve un fichier texte souvent appelé 'Dockerfile'.
Ce fichier texte regrouper l'ensemble des instructions permettant de créer un envrionment de travail isolé de votre machine hôte.

Un exemple minimal de Dockerfile: 

```Dockerfile
# Dockerfile
FROM python:3.11    # Image de base
RUN pip install numpy matplotlib scipy # Isntruction
```

Lancer le conteneur associé à `Dockerfile`