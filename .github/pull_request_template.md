## Description

<!-- Qu'est-ce que cette PR change, et pourquoi ? -->

Closes #

## Comment tester

<!-- Les étapes pour vérifier que ça fonctionne. -->

## Checklist sécurité

<!-- Coche ce qui s'applique. Si une case ne concerne pas cette PR, écris N/A à côté. -->

- [ ] L'autorisation est vérifiée côté serveur, pas seulement dans l'interface
- [ ] Les données reçues de l'utilisateur sont validées
- [ ] Aucun secret dans le code (mot de passe, clé, jeton, fichier .env)
- [ ] Aucune donnée médicale ou personnelle dans les logs ni dans les messages d'erreur
- [ ] Les tests couvrent le cas autorisé **et** le cas refusé (ex. patient A / patient B)
- [ ] Uniquement des données fictives
- [ ] Toute nouvelle dépendance est justifiée dans la description
- [ ] Le modèle de menaces est mis à jour si un nouveau flux de données apparaît
