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

| Volume | Point de montage | Type |
|----------|----------|----------|
| nvme0n1p1 | /boot/efi | vfat |
| nvme0n1p2 | /boot | xfs |
| rhel-root | / | xfs |
| rhel-var | /var | xfs |
| rhel-var_log | /var/log | xfs |
| rhel-home | /home | xfs |
| rhel-swap | swap | swap |

Le partitionnement a été réalisé en utilisant LVM afin de faciliter l’extension des volumes logiques lors des opérations d’administration futures.
## 4. Installation

## 5. Mises à jour

## 6. Comptes et accès distant

## 7. Pare-feu

## 8. Écarts entre les deux distributions

## 9. Incidents et corrections

## 10. Reconstruction rapide
