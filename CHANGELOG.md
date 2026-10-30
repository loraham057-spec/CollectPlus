# Journal des modifications

## 0.2.0 — 30 octobre 2026
- Harmonisation de la palette bleu-vert et amélioration de la hiérarchie visuelle.
- Amélioration des états interactifs, de la lisibilité des tableaux et de l'affichage mobile.
- Respect de la préférence système de réduction des animations.
- Limitation des fichiers de formulaires importés à 10 Mo.
- Protection des exports CSV/XLSX contre l'injection de formules tableur.
- CORS désormais désactivé par défaut en mode même origine et configurable par liste blanche.
- Invalidation du cache PWA pour forcer le chargement des nouveaux fichiers statiques.

## Limites connues à traiter avant production
- Authentification et gestion des rôles/permissions.
- Journal d'audit et contrôle d'accès aux données.
- Stratégie de sauvegarde/restauration testée et chiffrement adapté au contexte.
- Validation métier approfondie des formulaires importés et des soumissions.
