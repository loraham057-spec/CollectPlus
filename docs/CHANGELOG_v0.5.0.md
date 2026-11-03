# CollectPlus v0.5.0 — Revue et évolutions

## Interface
- Thème global harmonisé bleu nuit / turquoise / fond bleu-gris clair.
- Dashboard plus dense et adaptatif, filtres de période, formulaire et province.
- KPI de géolocalisation, chart de tendance et vues map/dashboard.
- Navigation additionnelle Partage public et Cartographie.
- Styles adaptatifs pour écrans intermédiaires et mobiles.

## Fonctionnalités
- API dashboard accepte `period`, `form_id`, `province`.
- Extraction GPS depuis formats payloads courants.
- Liens publics tokenisés: création, activation/désactivation, lecture du schéma et soumission anonyme.
- Validation server-side des champs obligatoires pour submissions publiques.
- Page publique mobile responsive et ajout GPS opt-in.
- Import CSV structuré en plus du JSON et XLSForm.
- Conditions d’affichage simples dans import XLSForm/CSV et builder.
- Cache PWA versionné.

## Revue et tests
- Compilation Python et vérification syntaxique JavaScript.
- Smoke tests API: health, création enquête, activation partage, route public HTML, schéma public, validation obligatoire, soumission, dashboard filtré, extraction GPS, désactivation partage, import CSV.
- Les tests ont été réalisés sur une base SQLite de test distincte, sans modifier la base du projet fourni.

## Limites / backlog production
- Authentification administrateur et RBAC à intégrer avant usage production.
- Rate limiting, CAPTCHA optionnel, journal d’audit, politique de sauvegarde et migration versionnée.
- XLSForm: relevant simple uniquement; pas encore XPath complet, calculations, repeats, media workflow ODK.
- QR code généré via service image tiers; éviter de transmettre des tokens de formulaires sensibles par un service QR externe ou remplacer par génération locale avant production.
