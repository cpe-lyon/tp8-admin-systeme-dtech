# Mot a définir 

<b>Inventaire</b> : L'inventaire est une liste exhaustive d'entités considérées comme un patrimoine ou une somme de biens, matériels ou immatériels, afin d'en faciliter l'évaluation ou la gestion. 

<b>Module</b> : un plugin ou plug-in, aussi nommé module d'extension, module externe, greffon, plugiciel, ainsi que add-in ou add-on en France, est un paquet qui complète un logiciel hôte pour lui apporter de nouvelles fonctionnalités.

<b>playbook</b> : Un playbook Ansible est un ensemble de scripts d'automatisation, appelés plays, qui définissent les tâches de <i>gestion de configuration</i> qu’Ansible doit réaliser sur une ou plusieurs machines. Les playbooks ne sont pas des fichiers texte standard, mais sont formatés en YAML. Il est possible de composer ses playbooks à partir de zéro, ou de récupérer certains modules pré-écrits, soit inclus dans le plateforme ou via sa communauté d'utilisateurs.

<b>gestion de configuration</b> : La gestion de la configuration est souvent désignée comme le contrôle de version dans le développement logiciel mais la portée de la gestion de configuration est beaucoup plus large et le contrôle de version est seulement une partie de celui-ci.
La gestion de configuration implique l'enregistrement de l'état et de modifications apportées aux éléments de configuration dans la configuration des bases de données de gestion et des systèmes de « Tickets ». Les derniers outils de gestion de configuration produisent automatiquement la configuration désirée de l'état en plus d'enregistrer les données de configuration. Cette capacité est appelée comme "infrastructure as code".

# Exercice 1

On crée 2 machines virtuelles.
On les relis par réseau interne avec virtual box.

On installe ansible
```
sudo apt update
sudo apt install software-properties-common
sudo apt install ansible
```
en sudo su on modifie via nano <i>/etc/ansible/hosts</i> en lui ajoutant l'adresse ip du client.
on ajoute au début de <i>/etc/hosts</i> l'adresse du client + node1 
exemple : `192.168.100.2 node1`

sur le client on créé une clé ssh avec:
`ssh-keygen`
on valide les valeurs par defaut et on ajoute pas de passphrase.

on l'envoi ensuite sur le serveur avec :
`ssh-copy-id remote_username@server_ip_address`
ici : `ssh-copy-id serveur@192.168.100.1`


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
