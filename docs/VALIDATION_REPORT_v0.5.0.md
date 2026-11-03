# Rapport de revue technique — CollectPlus v0.5.0

## Périmètre exécuté
- Revue du code source de l’archive `collectplus.zip` : structure FastAPI/SQLite, pages web, JS frontend, styles, importateur XLSForm/JSON, collecte locale et exports.
- Implémentation du dashboard filtrable, cartographie GPS, partage public tokenisé, page de réponse anonyme, CSV import, relevant simple, styles de l’application, cache PWA et documentation Render.

## Contrôles automatisés réalisés
- `python -m py_compile app/main.py` : réussi.
- `node --check app/static/app.js` : réussi.
- Extraction du script inline `public.html` puis `node --check` : réussi.
- Vérification HTML de présence des neuf vues, scripts Leaflet/QR et unicité des IDs : réussie.
- Tests API sur base SQLite temporaire distincte :
  - health et page d’accueil;
  - création de formulaire;
  - activation du lien de partage;
  - page publique et récupération du schéma;
  - validation d’un champ obligatoire (422 attendu);
  - enregistrement anonyme (200);
  - dashboard filtré par formulaire/province;
  - extraction d’une position GPS;
  - désactivation du partage (formulaire ensuite indisponible);
  - import CSV;
  - condition relevant : question conditionnelle non requise quand masquée et obligatoire quand affichée.
- Les tests ont été exécutés sans utiliser ni écraser une base réelle du projet.

## Limites de cette version
- Les endpoints d’administration ne sont pas authentifiés et il n’y a pas encore de RBAC complet.
- Pas encore de rate limiting/CAPTCHA, audit log ni stratégie backup automatisée.
- L’évaluation des expressions XLSForm `relevant` couvre les expressions simples documentées; elle n’est pas un moteur XPath complet.
- La carte nécessite un accès internet aux tuiles OpenStreetMap. Les positions s’affichent uniquement si les soumissions comportent des coordonnées GPS reconnues.
- Le QR est généré par une bibliothèque CDN (fallback image externe); avant production sensible, privilégier une dépendance embarquée localement.
- La validation visuelle interactive de l’ensemble des vues sur plusieurs navigateurs/tailles d’écran reste à effectuer côté navigateur de déploiement.

## Consigne importante
Ne pas utiliser cette version pour des données de santé ou données personnelles sensibles en production avant d’ajouter l’authentification et les contrôles de sécurité listés ci-dessus.
