
```md
name:
Un nom descriptif pour le playbook, indiquant son objectif : déployer un serveur web Apache avec des restrictions IP.
hosts:
Spécifie les hôtes cibles où le playbook sera exécuté. Ici, my_hosts est un placeholder et doit être remplacé par le groupe ou le nom d'hôte réel (par exemple, all, webservers, etc.).
become: yes
Indique que les tâches nécessitent des privilèges élevés (accès root). Cela est nécessaire pour installer des paquets, modifier des configurations système et gérer des services.


vars:
  apache_document_root: /var/www/html
  apache_server_name: mywebsite.local
  allowed_ips:
    - 192.168.1.10
    - 192.168.1.20
    - 192.168.1.30

    vars:
Définit des variables utilisées dans le playbook pour le rendre réutilisable et configurable.
apache_document_root:
Spécifie le répertoire où Apache servira le contenu web. Par défaut, c'est /var/www/html.
apache_server_name:
Définit le nom du serveur Apache. Ici, il est défini comme mywebsite.local.
allowed_ips:
Liste les adresses IP autorisées à accéder au serveur Apache. Cela garantit que seules les IPs spécifiées peuvent atteindre le serveur.


- name: Installer Apache
  ansible.builtin.apt:
    name: apache2
    state: present
    update_cache: yes

name:
Une description de la tâche : "Installer Apache."
ansible.builtin.apt:
Utilise le module apt pour gérer les paquets sur les systèmes Debian/Ubuntu.
name: apache2
Spécifie le paquet à installer (apache2).
state: present
S'assure que le paquet apache2 est installé. Si c'est déjà fait, cette tâche ne fait rien.
update_cache: yes
Met à jour le cache des paquets avant l'installation pour s'assurer d'avoir la dernière version.


- name: Déployer le modèle de configuration Apache
  ansible.builtin.template:
    src: apache.conf.j2
    dest: /etc/apache2/sites-available/000-default.conf
    mode: '0644'
    owner: root
    group: root
  notify: Redémarrer Apache


  name:
Décrit la tâche : "Déployer le modèle de configuration Apache."
ansible.builtin.template:
Utilise le module template pour déployer un fichier de configuration à partir d'un modèle Jinja2.
src: apache.conf.j2
Spécifie le fichier source du modèle (apache.conf.j2) situé dans le dossier templates du playbook.
dest: /etc/apache2/sites-available/000-default.conf
Spécifie le chemin de destination où le modèle sera rendu et sauvegardé.
mode: '0644'
Définit les permissions du fichier à 0644 (lisible par tous, modifiable par le propriétaire).
owner: root
Définit le propriétaire du fichier à root.
group: root
Définit le groupe du fichier à root.
notify: Redémarrer Apache
Déclenche le gestionnaire Redémarrer Apache si cette tâche modifie le fichier de configuration. Cela garantit qu'Apache est redémarré après tout changement.

Activer la Configuration du Site Apache
- name: Activer la configuration du site Apache
  ansible.builtin.command:
    cmd: a2ensite 000-default.conf
  notify: Redémarrer Apache


  name:
Décrit la tâche : "Activer la configuration du site Apache."
ansible.builtin.command:
Exécute une commande shell sur l'hôte cible.
cmd: a2ensite 000-default.conf
Active le fichier de configuration du site Apache (000-default.conf) à l'aide de la commande a2ensite.
notify: Redémarrer Apache
Déclenche le gestionnaire Redémarrer Apache pour s'assurer qu'Apache redémarre après l'activation du site.


- name: Démarrer et activer Apache
  ansible.builtin.service:
    name: apache2
    state: started
    enabled: true

  name:
Décrit la tâche : "Démarrer et activer Apache."
ansible.builtin.service:
Gère le service Apache à l'aide du module service.
name: apache2
Spécifie le nom du service (apache2).
state: started
S'assure que le service Apache est en cours d'exécution. S'il n'est pas démarré, il sera lancé.
enabled: true
Configure le service pour qu'il démarre automatiquement au boot.

4. Section des Gestionnaires


handlers:
  - name: Redémarrer Apache
    ansible.builtin.service:
      name: apache2
      state: restarted


    handlers:
Définit des actions qui sont déclenchées lorsqu'elles sont notifiées par des tâches.
name: Redémarrer Apache
Décrit le gestionnaire : "Redémarrer Apache."
ansible.builtin.service:
Gère le service Apache.
name: apache2
Spécifie le nom du service (apache2).
state: restarted
Redémarre le service Apache chaque fois que ce gestionnaire est déclenché.

Résumé
Ce playbook automatise les étapes suivantes :

Installe le paquet du serveur web Apache.
Déploie un modèle de configuration Apache personnalisé avec des restrictions IP.
Active la configuration du site Apache.
Démarre et active le service Apache.
Redémarre Apache chaque fois que la configuration change.

```

Il garantit que seules les adresses IP spécifiées (192.168.1.10, 192.168.1.20, 192.168.1.30) peuvent accéder au serveur, offrant ainsi une sécurité basique via des restrictions IP.

  


