# Installation de rhel9-lab et ubuntu24-lab

## 1. Périmètre

## 2. Ressources des machines virtuelles

| Paramètre | rhel9-lab | ubuntu24-lab |
|------------|------------|------------|
| vCPU | 2 | 2 |
| Mémoire | 2048 Mio | 2048 Mio |
| Disque | 30 Gio | 25 Gio |
| Réseau 1 | NAT | NAT |
| Réseau 2 | Réseau interne | Réseau interne |
| Hyperviseur | VMware Workstation | VMware Workstation |

## 3. Partitionnement appliqué


### Rhel9

| Volume      | Point de montage | Type |
|-------------|------------------|------|
| nvme0n1p1   | /boot/efi        | vfat |
| nvme0n1p2   | /boot            | xfs  |
| rhel-root   | /                | xfs  |
| rhel-var    | /var             | xfs  |
| rhel-var_log| /var/log         | xfs  |
| rhel-home   | /home            | xfs  |
| rhel-swap   | swap             | swap |

### Volumes logiques (LVM)

| Volume logique | Taille |
|----------------|---------|
| root | 6 Gio |
| var | 5 Gio |
| var_log | 3 Gio |
| home | 2 Gio |
| swap | 2 Gio |

Le partitionnement a été réalisé avec LVM afin de permettre une gestion flexible du stockage et un éventuel agrandissement des volumes logiques. Les tailles ont été adaptées à la capacité disponible du disque virtuel tout en conservant la séparation fonctionnelle des différents espaces du système.

En raison de la capacité limitée du disque virtuel, la quasi-totalité de l'espace disponible a été allouée aux volumes logiques. L'espace libre restant dans le groupe de volumes est donc limité.

### Ubuntu 23.04

| Volume | Point de montage | Type |
|---------|---------|---------|
| sda2 | /boot | ext4 |
| ubuntu-lv | / | ext4 |
| var | /var | ext4 |
| var_log | /var/log | ext4 |
| home | /home | ext4 |

### Volumes logiques (LVM)

| Volume logique | Taille |
|---------|---------|
| ubuntu-lv | 10 Gio |
| var | 3 Gio |
| var_log | 2 Gio |
| home | 2 Gio |

Le partitionnement a été réalisé avec LVM afin de faciliter l'administration du stockage. Un espace libre de 1,22 Gio a été conservé dans le groupe de volumes pour les futurs agrandissements.

## 4. Installation

### RHEL 9

- Installation minimale réalisée.
- Red Hat Enterprise Linux 9.0 (Plow).
- Architecture x86_64.
- Locale : fr_FR.UTF-8.
- Fuseau horaire : Europe/Paris.
- Synchronisation NTP active.
- Service en échec observé : NetworkManager-wait-online.service.

### Ubuntu 24.04

- Installation minimale réalisée.
- Ubuntu 24.04.5 LTS (Noble Numbat).
- Architecture x86_64.
- Locale : fr_FR.UTF-8.
- Fuseau horaire : Europe/Paris.
- Synchronisation NTP active.
- Aucune unité systemd en échec détectée.

## 5. Mises à jour

### RHEL 9

Commande exécutée :

```bash
sudo dnf -y update
```

La mise à jour n'a pas pu être effectuée car le système n'était pas enregistré auprès du service de souscription Red Hat.

Message obtenu :

```text
Le système n'est pas inscrit auprès un serveur de droits d'accès.
```

Aucun dépôt n'était disponible.

L'historique DNF a néanmoins été vérifié :

```bash
sudo dnf history
```

Une entrée d'installation datée du 30/09/2026 est présente.

### Ubuntu 24.04

Les commandes suivantes ont été exécutées :

```bash
sudo apt update
sudo apt -y full-upgrade
```

Les dépôts Ubuntu étaient accessibles et plusieurs paquets ont été mis à jour.

Les mises à jour ont notamment concerné :

- open-vm-tools
- open-vm-tools-desktop
- thermald
- snapd
- ca-certificates

L'historique des paquets a été vérifié dans :

```bash
/var/log/apt/history.log
```

Un nouveau noyau Linux (6.8.0-147-generic) a été installé. Un redémarrage est nécessaire pour le charger.

## 6. Comptes et accès distant

### Verrouillage du compte root

Sur les deux distributions, le compte root a été verrouillé afin d'empêcher les connexions directes.

Commande utilisée :

```bash
sudo passwd -l root
```

Commande de vérification :

```bash
sudo passwd -S root
```

Résultats :

- RHEL 9 : statut `LK`
- Ubuntu 24.04 : statut `L`

### Comptes nominatifs

Un compte nominatif `florinadmin` a été créé sur les deux systèmes.

#### RHEL 9

```bash
sudo useradd -m -c "Florin Awountsac" florinadmin
sudo usermod -aG wheel florinadmin
```

Le compte a été ajouté au groupe :

```text
wheel
```

#### Ubuntu 24.04

```bash
sudo useradd -m -c "Florin Awountsac" florinadmin
sudo usermod -aG sudo florinadmin
```

Le compte a été ajouté au groupe :

```text
sudo
```

Les tâches d'administration sont désormais réalisées par l'intermédiaire de comptes nominatifs et du mécanisme sudo.

### Vérification de la délégation

La délégation des privilèges a été testée après ouverture d'une nouvelle session avec le compte `florinadmin`.

Commandes utilisées :

```bash
groups
sudo -l
```

Résultat :

- RHEL : appartenance au groupe `wheel`
- Ubuntu : appartenance au groupe `sudo`

Observation :

L'ajout à un groupe n'est effectif qu'après reconnexion de l'utilisateur. Ce comportement a été observé et vérifié sur les deux distributions.

### Authentification par clé SSH

Une paire de clés ED25519 a été générée depuis le poste d'administration.

Commande utilisée :

```bash
ssh-keygen -t ed25519 -a 100 -C "florin@lab"
```

Résultat :

- Clé privée : `id_ed25519`
- Clé publique : `id_ed25519.pub`

Empreinte de la clé :

```text
SHA256:KaMc8fNsWCqsGi+6pUQf1CiwBqBF+MB81knsOVJk8tI
```
### Test de connexion distante

Depuis le poste d'administration Windows, une connexion SSH a été réalisée avec succès :

```bash
ssh florinadmin@192.168.80.140
```

Une seconde session a été ouverte avant toute modification de la configuration SSH conformément aux recommandations du sujet.

Cette vérification a permis de confirmer :

- le bon fonctionnement du service SSH ;
- l'accessibilité du serveur sur le réseau ;
- l'authentification du compte nominatif ;
- la possibilité d'administrer le serveur à distance.

## 7. Pare-feu

*À compléter lors de la Partie D.5 (firewalld sur RHEL et UFW sur Ubuntu).*

## 8. Écarts entre les deux distributions

| Opération | RHEL 9 | Ubuntu 24.04 | Justification |
|------------|------------|------------|------------|
| Gestionnaire de paquets | dnf | apt | Outils natifs différents |
| Historique des paquets | dnf history | /var/log/apt/history.log | Méthode de traçabilité différente |
| Dépôts | Dépend de subscription-manager | Disponibles immédiatement | Politique de distribution différente |
| Mise à jour système | Impossible sans enregistrement Red Hat | Fonctionnelle immédiatement | Gestion des licences différente |
| Locale | fr_FR.UTF-8 | fr_FR.UTF-8 | Configuration similaire |
| Synchronisation horaire | chronyd | systemd-timesyncd | Services différents |
| Groupe d'administration | wheel | sudo | Convention propre à chaque distribution |
| Système de fichiers racine | xfs | ext4 | Choix par défaut de la distribution |
| Verrouillage root | passwd -l root | passwd -l root | Même commande, même objectif |

## 9. Incidents et corrections

### Problème de dépôts RHEL

La machine RHEL 9 n'était pas enregistrée auprès des services Red Hat.

Conséquences :

- Impossible d'effectuer les mises à jour via `dnf update`
- Aucun dépôt disponible
- Impossible d'activer EPEL

Diagnostic obtenu :

```text
Le système n'est pas inscrit auprès un serveur de droits d'accès.
```

### Erreur de syntaxe lors de la mise à jour Ubuntu

La commande suivante a été saisie par erreur :

```bash
sudo -apt -y full-upgrade
```

Correction appliquée :

```bash
sudo apt -y full-upgrade
```

La mise à jour a ensuite été exécutée correctement.

### Service en échec sur RHEL

Le service suivant apparaissait en échec :

```text
NetworkManager-wait-online.service
```

Le fonctionnement réseau restant opérationnel, l'incident a été documenté sans impact sur les manipulations réalisées.

## 10. Reconstruction rapide

### RHEL 9

1. Créer la machine virtuelle.
2. Configurer le partitionnement LVM.
3. Créer les volumes :
   - root
   - var
   - var_log
   - home
   - swap
4. Configurer la locale française.
5. Configurer le fuseau horaire Europe/Paris.
6. Verrouiller le compte root.
7. Créer un compte administrateur dans le groupe wheel.
8. Configurer le pare-feu.

### Ubuntu 24.04

1. Créer la machine virtuelle.
2. Configurer le partitionnement LVM.
3. Créer les volumes :
   - ubuntu-lv
   - var
   - var_log
   - home
4. Configurer la locale française.
5. Configurer le fuseau horaire Europe/Paris.
6. Appliquer les mises à jour système.
7. Verrouiller le compte root.
8. Créer un compte administrateur dans le groupe sudo.
9. Configurer le pare-feu.
