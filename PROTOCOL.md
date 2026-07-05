# 🔱 PROTOCOLE GHOST1O1 — La Méthodologie des 5 Phases

> *Observer. Cartographier. Instrumenter. Exploiter. Partager.*

---

## Vue d'ensemble

Le protocole GHOST1O1 est une **méthode d'exécution** applicable à toute cible : caméra IP, routeur, application web, réseau d'entreprise, infrastructure cloud.

Cinq phases, **toujours dans cet ordre**, jamais raccourcies.

```
   ┌──────────────┐
   │  1. OBSERVER │ ──► Comprendre AVANT d'agir
   └──────┬───────┘
          ▼
   ┌──────────────────┐
   │ 2. CARTOGRAPHIER │ ──► Topologie, surfaces, dépendances
   └──────┬───────────┘
          ▼
   ┌────────────────────┐
   │ 3. INSTRUMENTER    │ ──► Outils minimaux, reproductibles
   └──────┬─────────────┘
          ▼
   ┌────────────────┐
   │  4. EXPLOITER  │ ──► Preuve, pas destruction
   └──────┬─────────┘
          ▼
   ┌────────────────┐
   │  5. PARTAGER   │ ──► Transmission, zero rétention
   └────────────────┘
```

---

## Phase 1 — OBSERVER

### Philosophie
> Le tireur d'élite observe sa cible 10x plus longtemps qu'il ne tire. Le hacker observe son environnement 10x plus longtemps qu'il ne scan.

### Actions
- Identifier le **périmètre physique** : WiFi ? Ethernet ? Bluetooth ? Zigbee ? LoRa ?
- Identifier les **services exposés** : ports ouverts, banners, headers HTTP.
- Identifier les **comportements normaux** : trafic légitime, heures d'activité, utilisateurs.
- Identifier les **points d'attention** : services par défaut, versions connues, firmwares anciens.

### Outils GHOST1O1
- `ghosteye` — discovery RTSP/ONVIF, scan ports, identification marques
- `nmap` — scan de surface (preset `-sV -A -T4`)
- `netdiscover` / `arp-scan` — reconnaissance réseau local
- Wireshark — capture passive

### Livrable
- Note structurée : "Voici ce que j'ai vu, sans rien casser."

---

## Phase 2 — CARTOGRAPHIER

### Philosophie
> Une carte sans légende est inutile. Une cible sans schéma est incompréhensible.

### Actions
- Dessiner la **topologie réseau** : IP, MAC, routeurs, switches, VLANs.
- Lister les **relations** : qui parle à qui, à quelle fréquence, sur quel port.
- Identifier les **dépendances logicielles** : frameworks, libs, versions, CVEs associées.
- Identifier les **utilisateurs** : credentials par défaut, sessions actives, niveaux d'accès.

### Outils GHOST1O1
- `ghosteye` — cartographie ONVIF complète avec GetCapabilities
- `quebec-ultimate` — chaînage OSINT récursif (ASN → IP → domains → emails)
- `ycc365-ghost` — extraction firmware, analyse binaire
- `phishcloner-ultimate` — compréhension des flux d'auth cibles

### Livrable
- Schéma ASCII ou Mermaid de la cible. Liste des services. Matrice de risque.

---

## Phase 3 — INSTRUMENTER

### Philosophie
> Un outil non documenté est un outil à jeter. Un outil à usage unique est un jouet.

### Actions
- Choisir le **minimum d'outils** nécessaires (1-3 max).
- Vérifier qu'ils sont **reproductibles** : même commande, même output, sur 2 machines différentes.
- Documenter **chaque paramètre** : ce qu'il fait, pourquoi il est là, ce qu'il sort.
- Préparer un **mode "dry-run"** : voir sans toucher.

### Outils GHOST1O1
- `ghosteye_proxy.py` — proxy RTSP→HLS réutilisable
- `biobypass` — génération de vecteurs anti-bio reproductible
- Tous les scripts GHOST1O1 sont **mono-fichier** ou **mono-dossier**, **zéro dépendance cachée**.

### Livrable
- Procédure pas-à-pas copiable. Sortie attendue pour chaque commande.

---

## Phase 4 — EXPLOITER

### Philosophie
> La preuve remplace la destruction. Le rapport remplace le flex.

### Actions
- **Capturer la preuve** : screenshot, log, hash, requête HTTP complète.
- **Limiter l'impact** : pas de DoS, pas de wipe, pas d'exfiltration inutile.
- **Documenter chaque étape** : timestamp, commande, output, interprétation.
- **Préparer le rollback** : comment défaire ce qu'on a fait.

### Règle d'or
> Si tu ne peux pas **expliquer** ce que tu viens de faire à quelqu'un qui ne l'a pas vu, **tu n'avais pas le droit de le faire**.

### Livrable
- Rapport structuré : timeline, commandes, outputs, preuves, recommandations.

---

## Phase 5 — PARTAGER

### Philosophie
> La connaissance non transmise est un crime contre la discipline.

### Actions
- Publier le **rapport** (anonymisé si nécessaire).
- Publier l'**outil** (s'il est généralisable).
- Publier le **tutorial** (si la méthode est reproductible).
- **Nommer les contributeurs** : ceux qui ont aidé, testé, critiqué.
- **Accepter la critique** : si quelqu'un trouve un défaut dans ta méthode, tu corriges et tu remercies.

### Canaux GHOST1O1
- GitHub : repos + Issues + Discussions
- README + INSTALL + USAGE **toujours** à jour
- Tutorials publiés dans `/tutorials/` de chaque repo

### Livrable
- Issue/PR/discussion publié(e). Retour intégré.

---

## Anti-patterns — Ce que le protocole INTERDIT

| ❌ Anti-pattern | Pourquoi | Alternative |
|-----------------|----------|-------------|
| Sauter la phase 1 | Tu casses sans comprendre | Toujours observer 10x plus que tu n'agis |
| Scanner tout /24 sans raison | Bruit, détection, perte de temps | Cible précise, scan focalisé |
| Exécuter un script sans le lire | Tu ne sais pas ce que tu fais | Lis le code. Chaque ligne. |
| Pas de preuve | "Je l'ai fait" ne vaut rien | Screenshot + log + hash |
| Garder le tool pour soi | Égoïsme déguisé en expertise | Publie ou ce n'est pas GHOST1O1 |
| Flex sur les forums | Bruit, drama, zéro transmission | Tutoriel, rapport, contribution |

---

## Application par repo GHOST1O1

| Repo | Phase 1 | Phase 2 | Phase 3 | Phase 4 | Phase 5 |
|------|---------|---------|---------|---------|---------|
| `ghosteye` | Scan réseau passif | Cartographie ONVIF/RTSP | Proxy HLS | Lecture streams | Tutoriel caméra IoT |
| `ycc365-ghost` | Identification firmware | Reverse engineering | Extraction vulnérabilités | PoC RCE | Disclosure responsable |
| `phishcloner-ultimate` | Analyse flux auth | Vectorisation | Templates réalistes | Simulation lab | Formation Blue Team |
| `biobypass` | Test liveness | Analyse biométrie | Vecteurs de contournement | Validation lab | Éducation anti-spoof |
| `quebec-ultimate` | OSINT passif | Chaînage récursif | Corrélation ASN | Pivot identités | Cartographie threat |
| `ghost1o1-design` | UI/UX hacker | Design system | Composants | Live demo | Standards ouverts |

---

*Le protocole n'est pas une prison. C'est un **tremplin**. Maîtrise-le, puis innove.*
