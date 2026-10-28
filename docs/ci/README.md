# Guide CI — SecureClinic


Fichier du workflow : `.github/workflows/ci.yml`

## 1. Quand la CI s'exécute-t-elle ?

| Événement | CI exécutée ? |
|---|---|
| Pull Request vers `main` | Oui |
| Merge (push) sur `main` | Oui |
| Push sur une autre branche sans PR | Non |

## 2. Règles des branches

- `main` est protégée : **aucun push direct**. Toute modification passe par une Pull Request.
- Une PR ne peut être fusionnée que si :
  - tous les checks CI obligatoires sont verts ;
  - au moins **1 coéquipier l'a approuvée**.
- Nommage des branches :

| Préfixe | Usage | Exemple |
|---|---|---|
| `feature/` | Nouvelle fonctionnalité | `feature/patient-booking` |
| `fix/` | Correction de bug | `fix/login-lockout` |
| `ci/` | Modification de la pipeline | `ci/add-gitleaks` |
| `docs/` | Documentation | `docs/threat-model` |



Préfixes de commit : `feat:`, `fix:`, `ci:`, `docs:`, `test:`, `chore:`.

## 4. Jobs CI actuels

| Nom du job | Dossier | Rôle |
|---|---|---|
| `backend-tests` | `backend/` | PHP 8.3, `composer install`, `php artisan test` (SQLite en mémoire) |
| `frontend-build` | `frontend/` | Node 22, `npm ci`, lint, tests, `npm run build` |
| `mobile-check` | `mobile/` | Node 22, `npm ci`, lint, tests |

Un job est **ignoré** (reste vert) tant que son dossier ne contient pas de `composer.json` / `package.json`.

**Les noms des jobs ne doivent jamais être renommés** : la protection de branche les exige par leur nom.


## 5. Hook pre-commit : détection de secrets

Avant chaque commit, [Gitleaks](https://github.com/gitleaks/gitleaks) vérifie qu'aucun secret (mot de passe, clé d'API, jeton) n'est commité. La configuration est dans `.pre-commit-config.yaml`.

**Installation : une seule fois, après avoir cloné le dépôt** (Python 3.9 ou plus récent ; sur Mac/Linux, remplacer `python` par `python3`) :

```
python -m pip install pre-commit
python -m pre_commit install
```

Le premier commit est plus long (1 à 3 minutes) : pre-commit prépare Gitleaks.

**Si un secret est détecté**, le commit est refusé : retirer le secret du fichier, puis recommencer. Ne jamais contourner le hook avec `git commit --no-verify` : un secret poussé sur un dépôt public est compromis, même s'il est supprimé ensuite.

**Faux positif** (ce n'est pas un vrai secret) : ajouter la valeur `Fingerprint` affichée par Gitleaks dans un fichier `.gitleaksignore`, et le signaler dans la PR.
