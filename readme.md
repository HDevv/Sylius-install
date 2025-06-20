# Installation de Sylius

Ce guide explique pas à pas comment installer **Sylius Standard** pour un projet nommé `SyliusInn`.

## Prérequis

| Outil               | Version recommandée                           |
| ------------------- | --------------------------------------------- |
| **PHP**             | ≥ 8.1                                         |
| **Composer**        | 2.x                                           |
| **MySQL / MariaDB** | 10.4 ou ≥ 8.0                                 |
| **Node.js**         | **v20**                                       |
| **Yarn**            | 1.x ou 4.x (PNP désactivé)                    |
| **Symfony CLI**     | Dernière version (facultatif mais recommandé) |

Assurez‑vous également que l’extension PHP **intl** est activée et que votre serveur dispose d’au moins **2 Go de RAM** (Composer peut en utiliser davantage : voir ci‑dessous).

---

## 1. Création du projet


# Le flag COMPOSER_MEMORY_LIMIT=-1 lève la limite de mémoire pour éviter les erreurs pendant l'installation
# à utiliser que ci nécessaire
COMPOSER_MEMORY_LIMIT=-1 composer create-project sylius/sylius-standard SyliusInn

```

Se placer ensuite dans le dossier du projet : 

```bash
cd SyliusInn
```
---

## 2. Installation des dépendances front‑end

```bash
yarn install        # installe les packages NPM listés dans package.json
yarn build          # compile et optimise les assets (utilisez `yarn dev` pour un build non minifié)
```

> **Important :** Sylius 1.13+ nécessite Node **v20**.

---

## 3. Configuration de la base de données

Créez une base MySQL (ou MariaDB) et un utilisateur dédié :

```sql
CREATE DATABASE inn_sylius_dev CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON inn_sylius_dev.* TO 'inn_sylius'@'localhost' IDENTIFIED BY 'csdev';
```

Définissez ensuite la variable d’environnement dans **.env.local** :

```dotenv
# fichier .env.local
DATABASE_URL="mysql://inn_sylius:csdev@127.0.0.1:3306/inn_sylius_dev"
```

---

## 4. Installation de Sylius

```bash
# Création du schéma, des clés secrètes, etc.
bin/console sylius:install
```

Répondez aux questions de l’assistant pour générer les clés JWT et créer le premier utilisateur.

---

## 5. Lancement du serveur de développement

```bash
symfony server:start --no-tls --port=8001 --document-root=public --allow-all-ip
```

---

## 6. (Optionnel) Charger les données de démonstration

```bash
php bin/console sylius:fixtures:load
```

Cette commande :

- crée un **Channel** nommé `FASHION_WEB` (langue, devise, pays, etc.) ;
- ajoute un administrateur `sylius / sylius` ;
- importe des produits exemples.

> Après le chargement, connectez‑vous au back‑office (`/admin`) puis allez dans **Configuration › Canaux › FASHION\_WEB › Modifier** pour ajuster le **Hostname**. Exemple : `IP`.

---

## 7. Accès à l’application

| Espace           | URL par défaut                                                     | Identifiants      |
| ---------------- | ------------------------------------------------------------------ | ----------------- |
| **Front‑office** | [http://IP:8001](http://IP:8001)             | —                 |
| **Back‑office**  | [http://IP:8001/admin](http://IP:8001/admin) | `sylius / sylius` |

---

## 8. (Optionnel) Installer le plugin CMS

1. Autoriser les recettes communautaires :

   ```bash
   composer config extra.symfony.allow-contrib true
   ```

2. Ajouter le plugin :

   ```bash
   composer require sylius/cms-plugin
   ```

3. Installer les dépendances JavaScript requises :

   ```bash
   yarn add trix@^2.0.0 swiper@^11.2.6
   ```

4. Recompiler les assets :

   ```bash
   # Développement
   ```

yarn encore dev

# Production

yarn encore production

````

5. Mettre à jour le schéma de base de données :

```bash
# En environnement de développement
bin/console doctrine:migrations:migrate

# En production
bin/console doctrine:migrations:migrate -e prod
````

---

## 9. Étapes suivantes

- Configurer vos canaux (locales, devises, méthodes de paiement et de livraison).
- Personnaliser le thème (templates Twig, fichiers SCSS, etc.).
- Consulter la [documentation officielle Sylius](https://docs.sylius.com) pour approfondir.

Bonne installation ! 🚀

