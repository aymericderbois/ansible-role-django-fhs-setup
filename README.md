# Ansible Role: Django FHS Setup

[![MIT License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)

Rôle Ansible minimaliste pour préparer l'installation d'une application Django en suivant la [Filesystem Hierarchy Standard (FHS)](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard) sur Debian 11/12/13.

## Fonctionnalités

- Création d'un utilisateur et d'un groupe système dédiés à l'application
- Création de la structure de répertoires FHS pour Django
- Installation des paquets système requis (Python, PostgreSQL dev)
- Configuration des permissions appropriées

## Plateformes supportées

- Debian 11 (Bullseye)
- Debian 12 (Bookworm)
- Debian 13 (Trixie)

## Structure FHS créée

Le rôle crée automatiquement la structure suivante :

- `/opt/{app_name}` : Répertoire racine de l'application
- `/etc/opt/{app_name}` : Fichiers de configuration de l'application
- `/var/opt/{app_name}` : Données variables (SQLite, etc.)
- `/var/log/{app_name}` : Fichiers de logs
- `/srv/{app_name}` : Données servies par l'application
- `/srv/{app_name}/media` : Fichiers média Django

## Utilisation

### Installation basique

```yaml
- hosts: servers
  become: true
  roles:
    - aymericderbois.django_fhs_setup
  vars:
    django_fhs_setup_app_name: myapp
```

### Configuration avancée

```yaml
- hosts: servers
  become: true
  roles:
    - aymericderbois.django_fhs_setup
  vars:
    django_fhs_setup_app_name: myapp
    django_fhs_setup_deployment_user: deploy
    django_fhs_setup_system_packages:
      - python3-dev
      - libpq-dev
      - libmysqlclient-dev
```

## Variables

### Variables obligatoires

| Variable                      | Description                          |
|-------------------------------|--------------------------------------|
| `django_fhs_setup_app_name`   | Nom de l'application Django          |

### Variables optionnelles

| Variable                            | Défaut              | Description                                      |
|-------------------------------------|---------------------|--------------------------------------------------|
| `django_fhs_setup_deployment_user`  | `debian`            | Utilisateur propriétaire des répertoires         |
| `django_fhs_setup_user_shell`       | `/usr/sbin/nologin` | Shell de l'utilisateur système de l'application  |
| `django_fhs_setup_system_packages`  | voir ci-dessous     | Liste des paquets système à installer            |

### Paquets système par défaut

```yaml
django_fhs_setup_system_packages:
  - python3-dev
  - libpq-dev
```

## Tests

### Avec `make`

```bash
# Lancer les linters
make lint

# Lancer les tests Molecule (Debian 12)
make test

# Lancer les tests sur toutes les plateformes
make test-all
```

### Avec `uv`

```bash
uv run ansible-lint
uv run yamllint .
uv run molecule test
```

## Licence

MIT - [aymericderbois](https://github.com/aymericderbois)

## Auteur

Ce rôle a été créé par [aymericderbois](https://github.com/aymericderbois).
