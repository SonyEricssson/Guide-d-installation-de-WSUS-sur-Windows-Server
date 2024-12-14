# Guide-d-installation-de-WSUS-sur-Windows-Server
WSUS (Windows Server Update Services) permet de gérer centralement les mises à jour de Windows dans un environnement d'entreprise. Ce guide vous aidera à configurer WSUS sur Windows Server.
Guide complet pour l'installation et la configuration de WSUS

Introduction

WSUS (Windows Server Update Services) permet de centraliser la gestion des mises à jour de Windows dans un environnement professionnel. Ce guide détaille chaque étape de l'installation, de la configuration et de la gestion de WSUS sur un serveur Windows.

Contenu

Introduction

Installation de WSUS

Configuration initiale de WSUS

Organisation des ordinateurs dans WSUS

Activation du ciblage WSUS côté client

Création d'une GPO pour le groupe WSUS_Clients

Configuration d'un client WSUS dans un domaine

Dépannage

Ressources utiles

1. Installation de WSUS

Préparation des machines virtuelles

Activer le mode d'évaluation (optionnel) :

slmgr /rearm

Exécutez cette commande dans une invite de commande en mode administrateur sur chaque machine virtuelle (VM).

Installer les prérequis sur le serveur Windows :

Assurez-vous que Microsoft .NET Framework 4.7 est installé.

Téléchargez et installez les éléments suivants :

SQLSysClrTypes (x86)

SQLSysClrTypes (x64)

Microsoft Report Viewer Redistributable 2012

Effectuez toutes les mises à jour disponibles via Windows Update.

Désactivez le pare-feu Windows pour éviter les conflits durant l'installation.

Installation du rôle WSUS

Ajoutez le rôle WSUS via le "Gestionnaire de serveur" :

Sélectionnez :

WID Connectivity

WSUS Services

Spécifiez un chemin pour stocker les mises à jour (par exemple : C:\WSUS).

Confirmez l'installation d'IIS (Serveur Web) et laissez les options par défaut.

Exécutez les tâches de post-installation :

Une fois l'installation terminée, cliquez sur "Lancer les tâches de post-installation" dans le Gestionnaire de serveur.

Vérifiez que le message "Configuration terminée pour Services WSUS" apparaît.

2. Configuration initiale de WSUS

Lancement de la console WSUS

Ouvrez le Gestionnaire de serveur et accédez à :

Outils > Service WSUS.

Lors de la première ouverture, un assistant de configuration s'exécutera automatiquement.

Configuration des paramètres initiaux

Connexion à Microsoft Update :

Sélectionnez Synchroniser à partir de Microsoft Update.

Si un proxy est utilisé, renseignez les informations nécessaires.

Choix des langues et des produits :

Langues : sélectionnez uniquement Français et Anglais.

Produits : cochez uniquement les systèmes d'exploitation utilisés dans votre organisation.

Synchronisation initiale :

Cochez "Commencer la synchronisation initiale".

Cliquez sur Terminer pour lancer la synchronisation.

3. Organisation des ordinateurs dans WSUS

Accéder à la console WSUS

Dans la console WSUS, cliquez sur Ordinateurs pour afficher les groupes actuels.

Comprendre les groupes d'ordinateurs

Ordinateurs non attribués : par défaut, toutes les nouvelles machines apparaissent ici lorsqu'elles sont détectées par WSUS.

Création de groupes personnalisés

Faites un clic droit sur Tous les ordinateurs et sélectionnez Ajouter un groupe d'ordinateurs.

Créez les groupes suivants :

GWSUS_Serveurs

GWSUS_Clients

Vérifiez que les groupes apparaissent sous "Tous les ordinateurs".

4. Activation du ciblage WSUS côté client

Configuration dans WSUS

Ouvrez la console WSUS et assurez-vous que le groupe GWSUS_Clients est présent.

Accédez à Options > Ordinateurs et activez Utiliser les paramètres de stratégie de groupe ou de registre sur les ordinateurs.

Cliquez sur Appliquer.

5. Création d'une GPO pour le groupe WSUS_Clients

Création de la GPO

Ouvrez le Gestionnaire des stratégies de groupe (GPMC).

Faites un clic droit sur votre domaine ou UO spécifique et sélectionnez Créer un objet GPO dans ce domaine et le lier ici.

Donnez-lui le nom GWSUS_Clients.

Configuration de la GPO

Accédez à :

Configuration ordinateur > Modèles d'administration > Composants Windows > Windows Update.

Configurez les paramètres suivants :

Spécifier l’emplacement intranet du service de mise à jour Microsoft :

HTTP : http://<votre_serveur>:8530

HTTPS : https://<votre_serveur>:8531

Autoriser le ciblage côté client : entrez le nom du groupe cible : GWSUS_Clients.

Appliquez et fermez.

6. Configuration d'un client WSUS dans un domaine

Application de la GPO

Sur le poste client, ouvrez une invite de commande en administrateur et exécutez :

gpupdate /force

Accédez à Windows Update et cliquez sur Rechercher des mises à jour.

Vérifiez dans la console WSUS que le client apparaît dans le groupe correspondant.

7. Dépannage

Problèmes courants

Synchronisation échouée :

Vérifiez les paramètres de proxy.

Assurez-vous que le serveur WSUS a accès à Internet.

Clients non visibles dans WSUS :

Assurez-vous que la GPO est correctement appliquée.

Vérifiez que le service Windows Update est actif sur le client.

Configurations spécifiques

Fichiers .ESD :

Ajoutez un type MIME .esd dans IIS : application/octet-stream.

Redémarrez le site IIS WSUS.

Ressources utiles

Documentation officielle de WSUS

Téléchargements Microsoft nécessaires

Ce guide offre une solution complète pour la gestion et la configuration de WSUS dans votre infrastructure. Pour toute question ou amélioration, n'hésitez pas à contribuer via GitHub !
