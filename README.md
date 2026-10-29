# CollectPlus — MVP de collecte et d’analyse

CollectPlus est une première version exécutable d’une application de collecte de données inspirée des outils de formulaires de terrain. Elle comprend une interface web responsive, un formulaire MAPEPI préchargé, un créateur de formulaires, l’importation de certains XLSForm/JSON, une file de soumissions hors ligne dans le navigateur, une synchronisation vers une base SQLite et des exports CSV, Excel et JSON.

## Démarrage sous Windows

1. Installez Python 3.11 ou une version ultérieure depuis [python.org](https://www.python.org/downloads/). Pendant l’installation, activez **Add Python to PATH**.
2. Décompressez le dossier `CollectPlus_MVP.zip` dans un dossier où vous avez le droit d’écrire.
3. Double-cliquez sur `run_windows.bat`. Au premier lancement, le script crée un environnement virtuel et installe les dépendances Python. Une connexion Internet est nécessaire pour cette installation initiale.
4. Le navigateur s’ouvrira sur **http://127.0.0.1:8000**. Gardez la fenêtre de terminal ouverte tant que vous utilisez l’application.

Les données synchronisées sont enregistrées dans `data/collectplus.db`. Ne supprimez pas ce fichier si vous voulez conserver les données collectées.

## Démarrage avec Docker

Installez Docker Desktop, ouvrez un terminal dans le dossier et exécutez :

```bash
docker compose up --build
```

Puis ouvrez http://127.0.0.1:8000. Le volume `./data` conserve la base quand le conteneur est recréé.

## Utilisation sur téléphone

- L’interface est responsive et s’utilise dans le navigateur mobile.
- Pour tester depuis un téléphone sur le même réseau Wi-Fi que l’ordinateur, lancez `run_network_windows.bat`, trouvez l’adresse IP locale de l’ordinateur (`ipconfig` sous Windows), puis ouvrez `http://ADRESSE-IP:8000` sur le téléphone.
- L’accès depuis une autre machine ne fonctionne que tant que le serveur reste démarré et que le pare-feu autorise la connexion.
- La prise en charge PWA (mise sur l’écran d’accueil et cache de la coque de l’application) nécessite un navigateur compatible. Le service worker est généralement disponible sur `localhost` ou en HTTPS. Pour une installation publique, configurez un nom de domaine et HTTPS.
- Les réponses saisies quand le serveur est indisponible sont gardées dans le stockage local du navigateur et peuvent être renvoyées lorsque le serveur redevient joignable. Évitez d’effacer les données du navigateur avant synchronisation.

## Fonctionnalités incluses

- Tableau de bord avec compteurs et graphiques, alimentés uniquement par les soumissions réelles.
- Formulaire « Fiche d’enregistrement et de suivi d’incident » adapté du document MAPEPI transmis : identification, étape du circuit, motifs, maladie/échantillon, impact/traçabilité, notification, mesures immédiates et correctives, clôture.
- Création simple d’enquêtes avec questions texte, texte long, nombre, date, heure, liste à choix unique, choix multiples, e-mail et téléphone.
- Importation `.json` CollectPlus et `.xlsx` XLSForm (feuilles `survey`, `choices` et, facultativement, `settings`). Les règles de pertinence, calculs, répétitions, médias et autres fonctions XLSForm avancées ne sont pas toutes exécutées : contrôlez chaque formulaire importé avant la collecte.
- Export CSV compatible avec Excel, classeur XLSX, archive JSON et impression du tableau de bord vers PDF via le navigateur.
- Personnalisation locale du nom et de la couleur de l’interface.

## Limites importantes de cette version

Cette livraison est un **MVP local prêt à lancer**, pas encore un déploiement de production ni un APK Android compilé. Elle n’intègre pas encore les comptes utilisateurs, les rôles, les permissions multi-organisation, le chiffrement de bout en bout, le contrôle d’accès côté serveur, ni une infrastructure de sauvegardes automatisées. Les préférences visuelles sont stockées dans le navigateur. La synchronisation hors ligne utilise `localStorage` et non une base mobile chiffrée.

**Ne publiez pas ce serveur directement sur Internet et n’y entrez pas de données personnelles ou médicales identifiantes en production avant d’ajouter une authentification forte, des droits d’accès, HTTPS, des sauvegardes testées et les protections de confidentialité requises.** Utilisez d’abord les données de test.

## API utile

- `GET /api/health` — état du serveur.
- `GET /api/forms` — formulaires disponibles.
- `POST /api/forms` — créer un formulaire (JSON).
- `POST /api/forms/import` — importer XLSForm/JSON.
- `POST /api/submissions` — enregistrer une soumission.
- `GET /api/submissions` — consulter les soumissions.
- `GET /api/dashboard` — agrégats pour le tableau de bord.
- `GET /api/export/csv`, `/api/export/xlsx`, `/api/export/json` — exports.

La documentation interactive de l’API est accessible sur **http://127.0.0.1:8000/docs** après démarrage.

## Préparation Android (APK)

Le dossier `android-wrapper/` contient un projet Capacitor et un script Windows pour préparer le projet Android. Pour construire un APK, il faut installer Node.js 22+, Android Studio et un SDK Android. Les dépendances requises pour Capacitor 8 et les étapes de compilation sont détaillées dans `android-wrapper/README_ANDROID.md`. Aucun APK compilé n’est fourni dans cette archive ; la version web locale est celle qui est directement prête à démarrer.

## Structure du projet

```text
CollectPlus_MVP/
├── app/
│   ├── main.py
│   └── static/
│       ├── index.html
│       ├── styles.css
│       ├── app.js
│       ├── manifest.webmanifest
│       ├── service-worker.js
│       └── icon.svg
├── data/
├── run_windows.bat
├── run_network_windows.bat
├── run_linux_mac.sh
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── sample_form.json
```
