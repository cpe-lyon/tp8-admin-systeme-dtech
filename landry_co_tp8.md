# Mot a définir 

<b>Inventaire</b> : L'inventaire (latin inventus) est une liste exhaustive d'entités considérées comme un patrimoine ou une somme de biens, matériels ou immatériels, afin d'en faciliter l'évaluation ou la gestion. 

<b>Module</b> : un plugin ou plug-in, aussi nommé module d'extension, module externe, greffon, plugiciel, ainsi que add-in ou add-on en France, est un paquet qui complète un logiciel hôte pour lui apporter de nouvelles fonctionnalités.

# Exercice 2. Tâches à réaliser

1. Si l’on n’est pas connecté à un noeud en tant que root, on aura besoin d’exécuter certaines commandes
à l’aide de sudo. Pour cela, modifiez la section [privilege_escalation] du fichier de configuration
/etc/ansible/ansible.cfg

`become_method=sudo` à décommenter

2. Si l’on n’est pas connecté à un noeud en tant que root, toutes les commandes sudo demanderont de
saisir un mot de passe. Pour éviter ceci, modifiez la configuration sudo pour que l’utilisateur sur le
noeud n’ait pas à saisir de mot de passe quand il utilise sudo

`@admin ALL=(ALL) NOPASSWD:ALL` à ajouter puis créer le group admin

3. Utilisez le module ping d’Ansible pour valider la configuration

`ansible -m ping node`

4. Utilisez le module setup pour récupérer la liste des périphériques (disques, cartes réseau…) présents
sur les noeuds

`ansible -m setup node`

5. Créez un premier playbook qui installe git sur le noeud

Créer un dossier
créer un fichier git.yml
taper `ansible-galaxy init git`

editer git.yml
```
---

- hosts: webservers
  remote_user: user
  become: yes

  roles:
    - git
```

editer git/tasks/main.yml
```
---

- name: Install git
  apt:
    name: git
    state: present
    update_cache: yes
```

6. Créez un second playbook qui installe tmux et screen en utilisant une liste pour éviter de dupliquer
inutilement les commandes


7. Modifiez ce playbook pour créer un utilisateur ; cet utilisateur devra faire partie des ”sudoers” sur le
serveur, et le fichier de règles sudo pour cet utilisateur devra être copié depuis la machine de supervision
(cf. tuto de Grafikart)
