<div align="center">

```
   ▄█████ █  ██  ▄█████ ▄█████▄  ██   ██ ▄█████ █    ██  ██ ██    ██
  ██      ██▄██  ██     ██   ██  ██▄▄▄██ ██     ██    ██  ██ ██    ██
  ██  ███ ██▀██  █████  ██████   ██   ██ █████  ██    ██  ██ ██    ██
  ██   ██ ██  ██ ██     ██   ██  ██   ██ ██      ██  ▄██  ██  ██  ██
   ▀████▀ ██  ██ ▀█████ ██   ██  ██   ██ ▀█████   ▀███▀██▄██  ▀███▀
```

![GHOST1O1](https://img.shields.io/badge/GHOST1O1-NOCTURNE-e63946?style=for-the-badge&logo=ghost&logoColor=white)
![Status](https://img.shields.io/badge/STATUS-OPERATIONAL-2ecc71?style=for-the-badge)
![Philosophy](https://img.shields.io/badge/PHILOSOPHY-OPEN-00d4ff?style=for-the-badge)

# **There is no lock.**

### Le hub central de l'écosystème GHOST1O1 NOCTURNE
**6 outils · 1 méthodologie · Zéro gatekeeping**

</div>

---

## 🔱 Qu'est-ce que GHOST1O1 ?

GHOST1O1 n'est pas un ensemble d'outils. C'est une **méthodologie complète** d'attaque, d'analyse et de transmission — appliquée à 6 projets concrets, libres, documentés à 100%, utilisables en 60 secondes depuis n'importe quel OS, par n'importe qui.

**Trois piliers non négociables :**

| 🔓 Ouverture | 🔀 Divergence | ⬆️ Élévation |
|--------------|----------------|----------------|
| Tout est libre, tout est documenté, tout est reproductible. | Chaque problème a N solutions. La première n'est jamais la bonne. | Celui qui sait et ne transmet pas trahit sa propre maîtrise. |

---

## 📦 Les 6 projets

<div align="center">

| Projet | Description | Cible | Status |
|--------|-------------|-------|--------|
| 🎯 **[ghosteye](https://github.com/187Ghost101/ghosteye)** | Plateforme RTSP/HLS pour caméras IoT | Caméras IP, NVR, DVR | ✅ v12.0 |
| 🏴‍☠️ **[ycc365-ghost](https://github.com/187Ghost101/ycc365-ghost)** | Scanner & reverse firmware YCC365/Hipcam | Caméras IoT grand public | ✅ v1.0 |
| 🎣 **[phishcloner-ultimate](https://github.com/187Ghost101/phishcloner-ultimate)** | Framework AiTM phishing modulaire (9 modules, 20 templates) | Red Team / formation Blue | ✅ v1.0.0 |
| 🧬 **[biobypass](https://github.com/187Ghost101/biobypass)** | 15 vecteurs anti-biométrie | Systèmes d'auth biométrique | ✅ v11.0 |
| 🔍 **[quebec-ultimate](https://github.com/187Ghost101/quebec-ultimate)** | Moteur de chaînage OSINT récursif | OSINT, threat intel | ✅ v3.0 |
| 🎨 **[ghost1o1-design](https://github.com/187Ghost101/ghost1o1-design)** | Design system hacker avant-gardiste | UI/UX outils sécu | ✅ v1.1 |

</div>

---

## 🚀 Démarrage ultra-rapide (60 secondes)

```bash
# 1. Clone le hub
git clone https://github.com/187Ghost101/ghost1o1.git
cd ghost1o1

# 2. Choisis ton outil (exemple : ghosteye)
git clone https://github.com/187Ghost101/ghosteye.git
cd ghosteye

# 3. Installe (méthode auto-détectée)
bash install.sh

# 4. Lance
python3 ghosteye_proxy.py 8082

# 5. Ouvre
firefox http://localhost:8082
```

**Sur ton cell depuis n'importe où :**
```bash
# Tunnel gratuit instantané
curl -sL https://github.com/ekzhang/bore/releases/download/v0.5.2/bore-v0.5.2-x86_64-unknown-linux-musl.tar.gz | tar xz -C /tmp/ && sudo mv /tmp/bore /usr/local/bin/
bore local 8082 --to bore.pub
# → http://bore.pub:XXXXX → cell
```

---

## 🔄 Le Protocole GHOST1O1 — La méthodologie

Chaque outil applique les **5 phases** du protocole :

```
   ┌──────────────┐
   │  1. OBSERVER │  Comprendre AVANT d'agir
   └──────┬───────┘
          ▼
   ┌──────────────────┐
   │ 2. CARTOGRAPHIER │  Topologie, surfaces, dépendances
   └──────┬───────────┘
          ▼
   ┌────────────────────┐
   │ 3. INSTRUMENTER    │  Outils minimaux, reproductibles
   └──────┬─────────────┘
          ▼
   ┌────────────────┐
   │  4. EXPLOITER  │  Preuve, pas destruction
   └──────┬─────────┘
          ▼
   ┌────────────────┐
   │  5. PARTAGER   │  Transmission, zero rétention
   └────────────────┘
```

📜 **[MANIFESTO.md](https://github.com/187Ghost101/ghost1o1/blob/main/MANIFESTO.md)** — la philosophie complète
📜 **[PROTOCOL.md](https://github.com/187Ghost101/ghost1o1/blob/main/PROTOCOL.md)** — la méthodologie des 5 phases

---

## 🌐 Cross-platform — Fonctionne PARTOUT

| OS | Support | Méthode |
|----|---------|---------|
| 🐧 **Kali / Debian / Ubuntu** | ✅ Natif | `bash install.sh` |
| 🏔️ **Arch / Manjaro** | ✅ Natif | `bash install.sh` |
| 🍎 **macOS** | ✅ Homebrew | `bash install.sh` |
| 🪟 **Windows (WSL2)** | ✅ WSL | `bash install.sh` |
| 📱 **Termux (Android)** | ✅ proot-distro | `bash install.sh` |
| 🐳 **Docker** | ✅ One-liner | `docker run` |
| ☁️ **Replit / Codespaces** | ✅ One-click | Bouton "Run" |
| ☁️ **bore.pub tunnel** | ✅ Gratuit | `bore local PORT` |
| ☁️ **ngrok** | ✅ Auth configuré | `ngrok http PORT` |

Chaque repo a son **`INSTALL.md`** détaillé par OS.

---

## 📚 Tutoriels — Le standard GHOST1O1

Tous les tutoriels suivent la structure 7-sections :

```
1. PHILOSOPHIE   → pourquoi cette technique existe
2. PRÉREQUIS     → ce que tu dois savoir
3. THÉORIE       → comment ça marche VRAIMENT
4. PRATIQUE      → commande par commande, output attendu
5. PIÈGES        → ce qui va casser, comment éviter
6. ALTERNATIVES  → 2-3 autres approches (divergence = choix)
7. TRANSMISSION  → exercise : trouve X, fais Y, publie Z
```

📂 **[TUTORIALS/](https://github.com/187Ghost101/ghost1o1/tree/main/tutorials)** — 3 tutoriels flagship, plus chaque mois

---

## 🎯 Démos live

| Service | URL | Description |
|---------|-----|-------------|
| 🎯 **ghosteye Pages** | [ghosteye-demo](https://187ghost101.github.io/ghosteye/) | Dashboard live, démo no-proxy |
| 🏴‍☠️ **ycc365-ghost Pages** | [ycc365-demo](https://187ghost101.github.io/ycc365-ghost/) | Scanner UI démo |
| ☁️ **ghosteye Replit** | [Run on Replit](https://replit.com/github/187Ghost101/ghosteye) | One-click cloud |
| 🐳 **ghosteye Docker** | `docker run -p 8082:8082 187ghost101/ghosteye` | Image officielle |

---

## 🤝 Contribution

GHOST1O1 vit grâce à ses contributeurs. Tu peux :

- 🐛 **Reporter un bug** → Issues sur le repo concerné
- 💡 **Proposer une feature** → Discussion dans le repo
- 🔧 **Soumettre un PR** → Toujours avec tests + doc
- 📚 **Écrire un tutoriel** → Structure 7-sections obligatoire
- 🌍 **Traduire** → FR/EN minimum

**Règles :**
- Pas de drama, pas d'ego, pas de gatekeeping
- Lis le code avant de proposer un PR
- Teste sur 2 OS minimum
- Signe ton travail (`Signed-off-by:`)

📜 **[CONTRIBUTING.md](https://github.com/187Ghost101/ghost1o1/blob/main/CONTRIBUTING.md)**

---

## 🔒 Sécurité & Légalité

**GHOST1O1 est un outil éducatif et de recherche.** Toute utilisation sur des systèmes sans autorisation explicite est **illégale** et **contraire à l'éthique du projet**.

Tu es responsable de ce que tu fais avec ces outils. Le protocole impose :

- Lab personnel ou cibles avec **autorisation écrite**
- Jamais de DoS, jamais de wipe, jamais d'exfiltration non-contrôlée
- **Disclosure responsable** : si tu trouves une 0-day, contacte le vendor AVANT de publier

📜 **[SECURITY.md](https://github.com/187Ghost101/ghost1o1/blob/main/SECURITY.md)** — politique complète

---

## 📜 Licence

Tous les repos GHOST1O1 sont sous **MIT License** sauf mention contraire. Tu peux fork, modifier, redistribuer, commercialiser — tant que tu gardes la signature `ghost1o1` et que tu ne gates pas l'accès.

---

## 📊 Le sommet en chiffres

```
6 repos actifs  |  1 méthodologie documentée  |  3 tutoriels flagship
0 paywall       |  0 gatekeeping             |  100% open source
4 OS supportés  |  3 méthodes de déploiement  |  ∞ divergences
```

---

## 🔥 La phrase

> **"There is no lock."**

Pas une métaphore de destruction. Une **constatation opérationnelle** : chaque système a une logique, chaque logique a des failles, chaque faille a un chemin d'entrée. Le hacker est celui qui trouve le chemin, pas celui qui défonce la porte.

---

<div align="center">

### Forged in the dark by [ghost1o1](https://github.com/187Ghost101) — 2026

*There is no lock.*

</div>
