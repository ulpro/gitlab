Voici un résumé des étapes pour l'installation et la configuration de **GitLab CE** sur un serveur Ubuntu 22.04 LTS :

---

## Installation et Configuration de GitLab CE sur Ubuntu 22.04 LTS

### Prérequis
- Un serveur Ubuntu 22.04 avec un utilisateur non-root ayant des privilèges `sudo`.
- Un minimum de **4 cœurs** pour le processeur et **4 Go de RAM**.
- Un pare-feu de base, comme `ufw`, est déjà configuré.

### Étape 1 : Installation des dépendances

1. **Mise à jour des packages du système :**

    ```bash
    sudo apt update
    ```

2. **Installer les dépendances requises :**

    ```bash
    sudo apt install ca-certificates curl openssh-server postfix tzdata perl
    ```

    - Lors de l'installation de **Postfix**, sélectionnez **Internet Site** et entrez le nom de domaine de votre serveur pour configurer l'envoi d'emails.

### Étape 2 : Installation de GitLab

1. **Télécharger et exécuter le script d'installation de GitLab :**

    ```bash
    cd /tmp
    curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
    sudo bash /tmp/script.deb.sh
    ```

2. **Installer GitLab CE :**

    ```bash
    sudo apt install gitlab-ce
    ```

    Cela prendra un certain temps pour télécharger et installer GitLab.

### Étape 3 : Configuration du pare-feu

1. **Vérifiez l'état de votre pare-feu avec `ufw` :**

    ```bash
    sudo ufw status
    ```

2. **Autoriser l'accès HTTP et HTTPS :**

    ```bash
    sudo ufw allow http
    sudo ufw allow https
    sudo ufw allow OpenSSH
    ```

3. **Vérifiez les règles du pare-feu pour vous assurer que les ports HTTP, HTTPS et SSH sont ouverts :**

    ```bash
    sudo ufw status
    ```

### Étape 4 : Configuration du fichier GitLab

1. **Modifier la configuration de GitLab :**

    Ouvrez le fichier de configuration `/etc/gitlab/gitlab.rb` :

    ```bash
    sudo nano /etc/gitlab/gitlab.rb
    ```

    - Modifiez la ligne `external_url` pour correspondre à votre domaine et utilisez `https` pour activer un certificat SSL avec Let's Encrypt :

    ```bash
    external_url 'https://your_domain'
    ```

    - Définissez l'email de contact pour Let's Encrypt (recommandé) :

    ```bash
    letsencrypt['contact_emails'] = ['tech@example.com']
    ```

2. **Exécuter la reconfiguration de GitLab :**

    ```bash
    sudo gitlab-ctl reconfigure
    ```

    Cela va configurer GitLab et générer automatiquement un certificat SSL avec Let's Encrypt.

### Étape 5 : Configuration initiale via l'interface web

1. **Accédez à votre instance GitLab via le navigateur :**

    Ouvrez votre navigateur et allez à `https://your_domain` pour la première connexion.

2. **Définir le mot de passe pour le compte `root` lors de la première connexion.**

3. **Connectez-vous avec le nom d'utilisateur `root` et le mot de passe que vous avez défini.**

---

Vous avez maintenant installé et configuré GitLab CE sur votre serveur Ubuntu. Vous pouvez commencer à utiliser GitLab pour l'hébergement de vos dépôts Git et la gestion de vos projets de développement.

---
