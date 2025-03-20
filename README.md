```yml
---

# Playbook : Déployer et configurer un serveur Apache avec Jinja2

- name: Configurer un serveur web Apache avec un template Jinja2
  hosts: all
  become: yes  # Élévation des privilèges pour installer et configurer Apache

  vars:
    apache_document_root: /var/www/html
    apache_server_name: mywebsite.local
    http_port: 80
    allowed_ips:
      - 192.168.1.10
      - 192.168.1.20
      - 192.168.1.30

  tasks:

  # 1. Installer Apache

  - name: Installer le paquet Apache
      ansible.builtin.apt:
        name: apache2
        state: present
        update_cache: yes

  # 2. Créer le répertoire DocumentRoot (si nécessaire)

  - name: Créer le répertoire DocumentRoot
      ansible.builtin.file:
        path: "{{ apache_document_root }}"
        state: directory
        mode: '0755'
        owner: www-data
        group: www-data

  # 3. Déployer le fichier de configuration Apache à partir du template Jinja2

  - name: Déployer le fichier de configuration Apache
      ansible.builtin.template:
        src: apache.conf.j2
        dest: /etc/apache2/sites-available/000-default.conf
        mode: '0644'
        owner: root
        group: root
      notify: Redémarrer Apache

  # 4. Activer la configuration du site Apache

  - name: Activer la configuration du site Apache
      ansible.builtin.command:
        cmd: a2ensite 000-default.conf
      notify: Redémarrer Apache

  # 5. Démarrer et activer le service Apache

  - name: S'assurer qu'Apache est démarré et activé au boot
      ansible.builtin.service:
        name: apache2
        state: started
        enabled: true

  handlers:

  # Redémarrer Apache si nécessaire

  - name: Redémarrer Apache
      ansible.builtin.service:
        name: apache2
        state: restarted
```