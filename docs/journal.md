# Journal TP01

## Séance 1

### Actions réalisées


- Création du dépôt GitHub
- Création des machines virtuelles rhel9-lab et ubuntu24-lab.
- Vérification des ressources allouées (CPU, mémoire, disque).
- Configuration des interfaces réseau NAT et réseau interne.
- Vérification des machines dans VMware.

### Pourquoi séparer /var et /var/log ?
La séparation de /var et /var/log permet d'éviter qu'une croissance excessive des journaux remplisse la partition racine. Par exemple, en cas d'erreur applicative générant plusieurs gigaoctets de logs, le système pourrait devenir inutilisable si /var/log partageait le même espace disque que /. L'isolation limite l'impact de ce type d'incident.

### Pourquoi /boot hors LVM ?
La partition /boot reste classique afin d'être accessible directement par le chargeur de démarrage. Cette configuration simplifie le démarrage du système et évite les dépendances à la couche LVM avant le chargement du noyau.

### Pourquoi 2 Gio de swap ?
La machine dispose de 2 Gio de RAM. Une partition swap de même taille offre une marge de sécurité lorsque la mémoire physique est saturée. Une taille de 8 Gio serait disproportionnée pour ce contexte, tandis qu'une absence de swap pourrait provoquer un arrêt brutal de certains processus.

### Pourquoi laisser de l'espace libre ?
L'espace libre permet d'agrandir ultérieurement un volume logique sans interruption de service. Cette réserve sera utilisée lors du TP suivant pour simuler des opérations d'administration réelles.

### Quel volume agrandir en premier ?
Le volume /var/log est probablement le premier candidat à un agrandissement car les fichiers journaux peuvent croître rapidement. Il faudra maîtriser l'extension d'un volume logique LVM ainsi que l'agrandissement du système de fichiers sans interruption de service.

### Difficultés rencontrées

- Compréhension des consignes

### Temps passé

- À compléter
