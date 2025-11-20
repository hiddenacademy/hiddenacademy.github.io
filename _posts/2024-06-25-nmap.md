---
layout: post
title: "Énumération FTP : Guide Complet"
date: 2025-11-20
categories: tutorial
image: /assets/images/nmap-scan-result.png
comments: 8
---

# 🕵️‍♂️ Énumération des services

## 🔎 Nmap – Scans de base

### Scan complet des ports avec détection de versions

```bash
sudo nmap 10.129.2.28 -p- -sV
```

## **Options :**

| Option        | Description                          |
| ------------- | ------------------------------------ |
| `10.129.2.28` | Analyse de la cible spécifiée        |
| `-p-`         | Analyse tous les ports (1 à 65535)   |
| `-sV`         | Détection de la version des services |

---

### Scan avec suivi de progression

```bash
sudo nmap 10.129.2.28 -p- -sV --stats-every=5s
```

## **Options supplémentaires :**

| Option             | Description                                  |
| ------------------ | -------------------------------------------- |
| `--stats-every=5s` | Affiche la progression toutes les 5 secondes |

---

### Scan avec verbosité

```bash
sudo nmap 10.129.2.28 -p- -sV -v
```

## **Options supplémentaires :**

| Option | Description                                              |
| ------ | -------------------------------------------------------- |
| `-v`   | Verbosité (plus de détails). Peut être doublé avec `-vv` |

---

### Scan furtif/détaillé

```bash
sudo nmap 10.129.2.28 -p- -sV -Pn -n --disable-arp-ping --packet-trace
```

## **Options supplémentaires :**

| Option               | Description                                           |
| -------------------- | ----------------------------------------------------- |
| `-Pn`                | Ignore le ping ICMP (considère la cible comme active) |
| `-n`                 | Désactive la résolution DNS                           |
| `--disable-arp-ping` | Désactive le ping ARP                                 |
| `--packet-trace`     | Affiche tous les paquets envoyés/reçus                |

---

## 📡 Saisie de bannières & validation manuelle

### Tcpdump – Capture du trafic réseau

```bash
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28
```

**But :** Observer les paquets échangés entre notre machine (`10.10.14.2`) et la cible (`10.129.2.28`).

---

### Netcat – Connexion directe à un service

```bash
nc -nv 10.129.2.28 25
```

## **Options :**

| Option | Description                 |
| ------ | --------------------------- |
| `-n`   | Pas de résolution DNS       |
| `-v`   | Mode verbeux (plus d’infos) |
| `25`   | Port ciblé (ici SMTP)       |

---
