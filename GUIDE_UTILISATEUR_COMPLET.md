# Guide utilisateur complet et professionnel
## De la création d’un projet à la mise en ligne finale

> **Public cible** : utilisateur non développeur ou débutant avancé.  
> **Objectif** : suivre un parcours **pas à pas**, clair et structuré, pour construire, tester, préparer et déployer une application de manière professionnelle.

---

## Sommaire

1. [Création du projet](#1-création-du-projet)  
2. [Configuration de l’environnement](#2-configuration-de-lenvironnement)  
3. [Développement de l’application](#3-développement-de-lapplication)  
4. [Interface utilisateur](#4-interface-utilisateur)  
5. [Gestion de la base de données](#5-gestion-de-la-base-de-données)  
6. [Gestion des fichiers et imports](#6-gestion-des-fichiers-et-imports)  
7. [Tests et validation](#7-tests-et-validation)  
8. [Sauvegarde et versionning](#8-sauvegarde-et-versionning)  
9. [Préparation à la mise en ligne](#9-préparation-à-la-mise-en-ligne)  
10. [Mise en ligne / Déploiement](#10-mise-en-ligne--déploiement)  
11. [Maintenance et évolutions](#11-maintenance-et-évolutions)  
12. [Annexes](#12-annexes)

---

## 1. Création du projet

### 1.1 Créer le dossier du projet

Choisissez un nom clair, court et stable : `gestion_stock`, `crm_client`, `suivi_projets`.

```bash
# Exemple : création d'un nouveau dossier de projet
mkdir mon_application
cd mon_application
```

### 1.2 Organisation recommandée des fichiers

Structure de base recommandée :

```text
mon_application/
├── app/
│   ├── main.py
│   ├── ui/
│   ├── services/
│   ├── models/
│   └── db/
├── tests/
├── data/
├── docs/
├── scripts/
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

### 1.3 Bonnes pratiques de nommage

| Élément | Bonne pratique | Exemple |
|---|---|---|
| Dossiers | minuscules + `_` | `gestion_clients` |
| Fichiers Python | minuscules + `_` | `import_service.py` |
| Classes | `PascalCase` | `ClientRepository` |
| Fonctions / variables | `snake_case` | `calculer_total()` |
| Branches Git | préfixe + sujet | `feature/import-csv` |

### 1.4 Initialisation Git

```bash
git init
git add .
git commit -m "Initialisation du projet"
```

### 1.5 Création de l’environnement virtuel

```bash
python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows (PowerShell)
# .venv\Scripts\Activate.ps1
```

### 1.6 Installation des dépendances

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

> Si `requirements.txt` n’existe pas encore, installez les premières bibliothèques puis exportez :

```bash
pip install fastapi uvicorn sqlalchemy python-dotenv
pip freeze > requirements.txt
```

### Check-list du chapitre 1

- [ ] Le dossier projet est créé et bien nommé.
- [ ] La structure de base est en place.
- [ ] Git est initialisé.
- [ ] L’environnement virtuel fonctionne.
- [ ] Les dépendances sont installées.

---

## 2. Configuration de l’environnement

### 2.1 Configuration Python

- Utilisez une version stable (ex. Python 3.11+).
- Uniformisez la version pour toute l’équipe.

```bash
python --version
```

### 2.2 Installation des bibliothèques nécessaires

Exemple courant :

| Besoin | Bibliothèque |
|---|---|
| API web | `fastapi`, `uvicorn` |
| Base de données | `sqlalchemy`, `alembic` |
| Variables d’environnement | `python-dotenv` |
| Validation de données | `pydantic` |
| Tests | `pytest` |

### 2.3 Gestion du fichier `requirements.txt`

- **Toujours versionner** `requirements.txt`.
- Le mettre à jour après ajout/suppression de paquets.

```bash
pip freeze > requirements.txt
```

### 2.4 Gestion des variables d’environnement

Créer `.env` (non versionné) et `.env.example` (versionné).

**Exemple `.env.example`** :

```env
# Variables d'environnement minimales
APP_ENV=development
APP_DEBUG=true
DATABASE_URL=sqlite:///./app.db
SECRET_KEY=change_me
```

### 2.5 Structure idéale d’un projet professionnel

Principes :
- séparation claire des couches,
- documentation centralisée dans `docs/`,
- scripts d’automatisation dans `scripts/`.

### Check-list du chapitre 2

- [ ] Version Python validée.
- [ ] Bibliothèques nécessaires installées.
- [ ] `requirements.txt` à jour.
- [ ] `.env` et `.env.example` correctement gérés.
- [ ] Structure professionnelle confirmée.

---

## 3. Développement de l’application

### 3.1 Architecture recommandée

Architecture modulaire (exemple) :
- `ui/` : interface utilisateur,
- `services/` : logique métier,
- `models/` : objets et schémas,
- `db/` : accès aux données.

### 3.2 Séparation métier / interface / base de données

**Règle d’or** : l’interface ne doit pas contenir la logique métier complexe.  
La logique métier ne doit pas dépendre directement de l’interface.

### 3.3 Gestion des erreurs

- Toujours capturer les cas prévisibles (données absentes, format invalide, connexion BDD indisponible).
- Retourner un message clair à l’utilisateur final.

```python
# Exemple commenté : gestion d'une erreur fonctionnelle
if montant < 0:
    raise ValueError("Le montant ne peut pas être négatif.")
```

### 3.4 Logs et journalisation

- Utilisez `logging` (pas `print` pour la production).
- Niveaux recommandés : `INFO`, `WARNING`, `ERROR`.

### 3.5 Conseils d’optimisation du code

- Éviter les duplications.
- Écrire des fonctions courtes (1 responsabilité).
- Documenter les fonctions importantes.

### 3.6 Sécurité minimale

- Ne jamais stocker de secret en dur dans le code.
- Valider toutes les entrées utilisateur.
- Activer l’authentification sur les zones sensibles.

### Check-list du chapitre 3

- [ ] Architecture modulaire respectée.
- [ ] Séparation des responsabilités claire.
- [ ] Gestion d’erreurs mise en place.
- [ ] Journalisation active.
- [ ] Mesures minimales de sécurité appliquées.

---

## 4. Interface utilisateur

### 4.1 Recommandations UI/UX

- Navigation simple et cohérente.
- Textes explicites (éviter le jargon).
- Messages d’erreur compréhensibles.

### 4.2 Uniformisation des boutons, menus et onglets

Créez une mini charte :

| Composant | Règle |
|---|---|
| Bouton principal | couleur unique + verbe d’action |
| Bouton secondaire | style neutre |
| Menus | ordre stable sur toutes les pages |
| Onglets | intitulés courts et explicites |

### 4.3 Gestion des thèmes et couleurs

- Palette limitée (3 à 5 couleurs principales).
- Contraste élevé pour l’accessibilité.

### 4.4 Responsive design

- Vérifier sur desktop, tablette, mobile.
- Prioriser les éléments critiques sur petit écran.

### Exemple de capture fictive (description)

> **Capture fictive A** : écran d’accueil avec barre de navigation en haut, bouton principal « Créer », panneau de statistiques, thème clair avec couleur d’accent bleu.

### Check-list du chapitre 4

- [ ] Interface cohérente et simple.
- [ ] Composants uniformisés.
- [ ] Thème lisible et accessible.
- [ ] Affichage vérifié sur plusieurs tailles d’écran.

---

## 5. Gestion de la base de données

### 5.1 Création de la base

- Définissez les entités principales (utilisateur, produit, commande, etc.).
- Créez un schéma initial propre.

### 5.2 Sauvegarde automatique

- Planifier une sauvegarde quotidienne.
- Conserver plusieurs versions (rotation).

### 5.3 Migration et maintenance

- Utiliser un outil de migration (ex. Alembic).
- Versionner les scripts de migration.

### 5.4 Sécurisation des données

- Droits d’accès minimaux.
- Chiffrement des données sensibles.
- Journalisation des actions critiques.

### Check-list du chapitre 5

- [ ] Schéma BDD défini.
- [ ] Sauvegardes automatiques configurées.
- [ ] Processus de migration en place.
- [ ] Données sensibles protégées.

---

## 6. Gestion des fichiers et imports

### 6.1 Import Excel/CSV

- Accepter les formats `.csv` et `.xlsx`.
- Vérifier les colonnes attendues.

### 6.2 Validation des données

Contrôles essentiels :
- champs obligatoires,
- format des dates,
- types numériques,
- doublons.

### 6.3 Gestion des erreurs d’import

- Produire un rapport d’erreurs ligne par ligne.
- Permettre la correction et la réimportation.

### 6.4 Export des données

- Formats conseillés : CSV, XLSX, PDF.
- Ajouter date et filtre utilisés dans le nom de fichier exporté.

### Check-list du chapitre 6

- [ ] Formats d’import définis.
- [ ] Validation robuste implémentée.
- [ ] Rapport d’erreurs d’import disponible.
- [ ] Export standardisé.

---

## 7. Tests et validation

### 7.1 Tests fonctionnels

- Vérifier chaque fonctionnalité métier clé.
- Simuler des cas réels utilisateur.

### 7.2 Vérification des erreurs

- Tester les scénarios invalides volontairement.
- Contrôler la qualité des messages d’erreur.

### 7.3 Gestion des bugs

Cycle recommandé :
1. Reproduire,
2. Isoler,
3. Corriger,
4. Tester,
5. Documenter.

### 7.4 Procédure de validation avant mise en ligne

| Étape | Vérification |
|---|---|
| Technique | tests passés, dépendances stables |
| Sécurité | secrets absents, accès protégés |
| Fonctionnel | parcours utilisateur validés |
| Documentation | guide utilisateur + notes de version à jour |

### Check-list du chapitre 7

- [ ] Tests fonctionnels effectués.
- [ ] Scénarios d’erreur validés.
- [ ] Bugs critiques corrigés.
- [ ] Validation pré-production signée.

---

## 8. Sauvegarde et versionning

### 8.1 Utilisation de Git

Flux simple recommandé :
- `main` = stable,
- `develop` = intégration,
- `feature/*` = nouvelles fonctions.

### 8.2 Création des commits

Règles :
- petits commits,
- message clair au présent.

```bash
git add .
git commit -m "Ajoute la validation des imports CSV"
```

### 8.3 Gestion des branches

```bash
git checkout -b feature/tableau-de-bord
# ... travail ...
git checkout develop
git merge feature/tableau-de-bord
```

### 8.4 Sauvegardes locales et distantes

- Local : copie quotidienne du dossier de projet.
- Distant : push régulier sur GitHub/GitLab/Bitbucket.

### 8.5 Procédure de restauration

- Restaurer la dernière sauvegarde valide.
- Rejouer les migrations si nécessaire.
- Vérifier la cohérence applicative.

### Check-list du chapitre 8

- [ ] Stratégie Git définie.
- [ ] Commits clairs et fréquents.
- [ ] Branches maîtrisées.
- [ ] Sauvegardes locales + distantes actives.
- [ ] Procédure de restauration documentée.

---

## 9. Préparation à la mise en ligne

### 9.1 Nettoyage du projet

- Supprimer fichiers temporaires.
- Retirer dépendances inutilisées.
- Vérifier les droits d’accès fichiers.

### 9.2 Vérification des dépendances

```bash
pip list
pip freeze > requirements.txt
```

### 9.3 Sécurisation des accès

- Changer les mots de passe par défaut.
- Régénérer les clés sensibles.
- Désactiver les comptes de test.

### 9.4 Fichier exécutable (si nécessaire)

Pour application desktop Python : `pyinstaller` peut être utilisé.

### 9.5 Préparation du dossier de déploiement

Contenu minimal :
- code source,
- `requirements.txt`,
- variables d’environnement documentées,
- scripts de démarrage,
- documentation.

### Check-list du chapitre 9

- [ ] Projet nettoyé.
- [ ] Dépendances verrouillées.
- [ ] Accès sécurisés.
- [ ] Artifacts de déploiement prêts.

---

## 10. Mise en ligne / Déploiement

### 10.1 Déploiement local

Objectif : valider en environnement proche de la production.

```bash
# Exemple générique
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

### 10.2 Déploiement sur serveur

Étapes :
1. Créer un utilisateur dédié,
2. Installer runtime Python,
3. Déployer le code,
4. Configurer service système (systemd),
5. Mettre un reverse proxy (Nginx).

### 10.3 Déploiement cloud

Options courantes : AWS, Azure, GCP, Render, Railway, Fly.io.

### 10.4 Domaine et SSL

- Pointer le domaine vers le serveur.
- Installer un certificat SSL (Let’s Encrypt).
- Forcer HTTPS.

### 10.5 Vérifications post-déploiement

- Disponibilité de l’application,
- Connexion base de données,
- Logs sans erreur critique,
- Test d’un parcours utilisateur complet.

### Check-list du chapitre 10

- [ ] Déploiement local validé.
- [ ] Déploiement serveur/cloud opérationnel.
- [ ] Domaine correctement configuré.
- [ ] SSL actif et HTTPS forcé.
- [ ] Contrôles post-déploiement effectués.

---

## 11. Maintenance et évolutions

### 11.1 Mise à jour de l’application

- Planifier des fenêtres de maintenance.
- Tester en préproduction avant production.

### 11.2 Gestion des correctifs

- Prioriser : critique > majeur > mineur.
- Documenter chaque correctif.

### 11.3 Sauvegardes périodiques

- Vérifier régulièrement la restauration (test réel).

### 11.4 Surveillance des erreurs

- Centraliser les logs.
- Mettre des alertes automatiques (email, Slack, etc.).

### 11.5 Documentation des nouvelles versions

Fournir à chaque release :
- changelog,
- impacts,
- procédure de rollback.

### Check-list du chapitre 11

- [ ] Processus de mise à jour défini.
- [ ] Correctifs tracés et priorisés.
- [ ] Sauvegardes testées.
- [ ] Monitoring actif.
- [ ] Changelog maintenu.

---

## 12. Annexes

### 12.1 Arborescence type d’un projet

```text
mon_application/
├── app/
│   ├── main.py
│   ├── api/
│   ├── services/
│   ├── models/
│   └── db/
├── tests/
├── docs/
├── scripts/
├── data/
├── requirements.txt
├── .env.example
└── README.md
```

### 12.2 Commandes Git utiles

| Action | Commande |
|---|---|
| Initialiser | `git init` |
| Voir l’état | `git status` |
| Créer une branche | `git checkout -b feature/nom` |
| Committer | `git commit -m "message"` |
| Envoyer distant | `git push origin branche` |
| Historique | `git log --oneline --graph` |

### 12.3 Commandes Python utiles

| Action | Commande |
|---|---|
| Créer venv | `python -m venv .venv` |
| Activer venv (Linux/macOS) | `source .venv/bin/activate` |
| Installer dépendances | `pip install -r requirements.txt` |
| Export dépendances | `pip freeze > requirements.txt` |
| Lancer tests | `pytest` |

### 12.4 Check-list finale avant mise en production

- [ ] Tous les tests sont validés.
- [ ] Sauvegardes testées et restaurables.
- [ ] Secrets sécurisés (aucune clé dans le code).
- [ ] HTTPS actif et certifié.
- [ ] Documentation utilisateur à jour.
- [ ] Plan de rollback disponible.

### 12.5 Conseils professionnels et erreurs fréquentes à éviter

#### Bonnes pratiques pro
- Commencer simple, itérer vite.
- Documenter au fur et à mesure.
- Automatiser les tâches répétitives.
- Surveiller la sécurité dès le début.

#### Erreurs fréquentes
- Déployer sans tests suffisants.
- Mélanger logique métier et interface.
- Oublier les sauvegardes.
- Versionner des secrets dans Git.
- Négliger la documentation.

---

## Modèle de page de garde (option PDF/DOCX)

**Titre** : Guide complet de développement et déploiement  
**Version** : 1.0  
**Date** : (à renseigner)  
**Auteur/Équipe** : (à renseigner)

---

## Conseils de mise en forme pour export PDF/DOCX

- Police lisible : Inter, Calibri ou Arial (11 ou 12 pt)
- Titres hiérarchisés (H1/H2/H3)
- Marges régulières et interlignage 1.15 à 1.5
- Tableaux avec en-têtes visibles
- Ajout possible d’un en-tête/pied de page (version/date)

Ce document est prêt à être exporté en PDF ou DOCX depuis un éditeur Markdown compatible (Typora, VS Code + extension, Pandoc, etc.).
