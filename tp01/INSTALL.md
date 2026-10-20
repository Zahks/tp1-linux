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

## 5. Mises à jour

## 6. Comptes et accès distant

## 7. Pare-feu

## 8. Écarts entre les deux distributions

## 9. Incidents et corrections

## 10. Reconstruction rapide
