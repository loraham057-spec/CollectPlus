# Déploiement de CollectPlus v0.5.0 sur Render

## 1. Mettre le projet dans GitHub
1. Décompresser l’archive CollectPlus v0.5.0.
2. Copier le contenu du dossier racine du projet dans le dépôt `loraham057-spec/CollectPlus`.
3. Préserver les données en sauvegardant d’abord la base SQLite et en ne committant pas `data/*.db`.
4. Commit puis push sur la branche connectée au service Render.

## 2. Configurer le service Render
- Type : Web Service avec Docker (le projet fournit un `Dockerfile`).
- Root Directory : dossier racine contenant le Dockerfile.
- Port exposé : 8000; la commande uvicorn du Dockerfile écoute sur `0.0.0.0:8000`.
- Health check path : `/api/health`.

## 3. Rendre la base persistante
Dans Render, ajouter un Persistent Disk au service :
- Mount Path : `/app/data`
- Taille : adaptée aux volumes de collecte
- Variable d’environnement : `COLLECTPLUS_DB=/app/data/collectplus.db`

Sans disque persistant, les données SQLite peuvent être perdues lors d’un redéploiement ou d’un remplacement de l’instance. Faire des sauvegardes régulières.

## 4. Configurer les origines CORS
Ajouter une variable d’environnement Render :
`COLLECTPLUS_CORS_ORIGINS=https://collectplus.onrender.com,capacitor://localhost,http://localhost:8000,http://127.0.0.1:8000`

Ajouter le domaine de production réel si le frontend est hébergé ailleurs.

## 5. Déployer et contrôler
- Manual Deploy → Deploy latest commit.
- Attendre l’état Live.
- Vérifier `https://collectplus.onrender.com/api/health` (version attendue 0.5.0).
- Tester le dashboard, les formulaires, le partage public, le formulaire anonyme, l’enregistrement de la réponse, la désactivation du lien et les coordonnées GPS.

## 6. Test de partage public
1. Dans CollectPlus, ouvrir « Partage public ».
2. Choisir un formulaire et générer le lien/QR.
3. Ouvrir ce lien dans une fenêtre privée ou un autre téléphone sans session.
4. Remplir et soumettre le formulaire.
5. Vérifier la nouvelle soumission dans l’espace propriétaire et dans le dashboard.

## Avertissement
Les endpoints d’administration restent non authentifiés dans cette version. Ne pas exposer CollectPlus aux données de santé/personnelles réelles avant d’ajouter login/RBAC, journal d’audit, protection anti-abus et stratégie de sauvegarde/restauration testée. Le partage public est anonyme par conception; limiter le contenu de ces formulaires et les champs identifiants.
