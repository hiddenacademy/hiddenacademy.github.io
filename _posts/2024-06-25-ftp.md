---
layout: post
title: "Énumération FTP : Guide Complet"
date: 2025-11-20
categories: tutorial
image: /assets/images/ftp.jpg
comments: 8
---

# 🛠️ Énumération FTP — Cheat Sheet

---

**Énumération FTP**
**FTP (File Transfer Protocol)** fonctionne principalement sur **TCP/21**. L’énumération permet d’identifier si le service est accessible, la version du serveur, et si un accès anonyme est possible.

---

## 🔎 1 — Découverte & Reconnaissance

```bash
# Scan simple du port FTP
nmap -p 21 <CIBLE>

# Détection de version & scripts par défaut
nmap -p 21 -sV -sC <CIBLE>

# Scripts NSE utiles pour l'énumération
nmap -p 21 --script=ftp-anon,ftp-syst,ftp-bounce,ftp-brute <CIBLE>
```

---

## 🔑 2 — Connexion & Authentification

```bash
# Connexion interactive basique
ftp <CIBLE>

# Connexion en une ligne (authentifiée)
ftp ftp://<user>:<password>@<CIBLE>

# Connexion anonyme
ftp <CIBLE>
# USER : anonymous
# PASS : anonymous (ou adresse email)
```

---

## 📂 3 — Commandes FTP Essentielles

|        Commande | Action                            |
| --------------: | :-------------------------------- |
|    `ls` / `dir` | Lister fichiers & dossiers        |
|      `cd <dir>` | Changer de répertoire             |
|           `pwd` | Afficher répertoire courant       |
| `get <fichier>` | Télécharger un fichier            |
|        `mget *` | Télécharger plusieurs fichiers    |
| `put <fichier>` | Envoyer un fichier                |
|        `mput *` | Envoyer plusieurs fichiers        |
|        `binary` | Mode binaire (fichiers non-texte) |
|         `ascii` | Mode ASCII (fichiers texte)       |
|       `passive` | Basculer en mode passif           |
|  `bye` / `quit` | Quitter la session                |

---

## 📥 Download / 📤 Upload — Avancé

```bash
# Téléchargement récursif (anonyme)
wget -m --no-passive ftp://anonymous:anonymous@<CIBLE>/

# Téléchargement récursif (authentifié)
wget -m --no-passive ftp://<user>:<password>@<CIBLE>/

# Télécharger un fichier avec curl
curl -u <user>:<pass> ftp://<CIBLE>/chemin/fichier -O

# Uploader un fichier avec curl
curl -T localfile.txt -u <user>:<pass> ftp://<CIBLE>/remote/path/

# Mode passif forcé (client en ligne)
ftp> passive
Passive mode on.
```

---

## 🔐 4 — Brute-Force & Tests d’Auth (énumération)

```bash
# Test simple d'utilisateur/mot de passe (session FTP interactive)
quote USER <username>
quote PASS <password>
```
> Nmap brute-force (NSE)
```bash
nmap -p 21 --script ftp-brute <CIBLE>
```
>Hydra (brute-force)
```bash
hydra -L users.txt -P passwords.txt ftp://<CIBLE>
```

> ⚠️ Utilise ces outils **uniquement** avec autorisation explicite (pentest / scope autorisé).

---

## 📜 5 — Bannière & Fingerprinting

```bash
# Récupérer la bannière
echo | nc -nv <CIBLE> 21
telnet <CIBLE> 21

# FTPS / STARTTLS (voir certificat)
openssl s_client -connect <CIBLE>:21 -starttls ftp

# Nmap pour version
nmap -sV -p 21 <CIBLE>
```

---

## 🧭 6 — Mode Actif vs Passif (rappel rapide)

* **Actif** : client ouvre connexion contrôle (21) → serveur se connecte au client pour la donnée (port client). Problème si firewall côté client.
* **Passif** : client initie aussi la connexion de données vers un port annoncé par le serveur → mieux derrière firewall/NAT.

---

## ✅ 7 — Points Clés (cheats)

* Tester **anonymous:anonymous** systématiquement.
* Récupérer **bannière** pour fingerprinting (version serveur).
* Utiliser NSE : `ftp-anon`, `ftp-syst`, `ftp-bounce`, `ftp-brute`.
* Vérifier **FTPS** (STARTTLS sur 21 ou port 990).
* Pour transferts massifs, `wget -m --no-passive` est pratique (attention aux logs/alarme).

---

