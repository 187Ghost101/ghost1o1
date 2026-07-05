# 🎓 TUTORIAL 03 — INSTRUMENTER comme ghost1o1

> **Choisis tes outils, construis ta trousse**
>
> Méthodologie : Protocole GHOST1O1 · Phase 3
>
> Niveau : Intermédiaire→Avancé · Durée : 90 min
>
> Outils : Python stdlib, ghosteye, scripts custom

---

## 1. PHILOSOPHIE

Instrumenter, c'est **préparer** ses outils. Un outil non documenté est un outil à jeter. Un outil à usage unique est un jouet.

**Trois questions avant chaque outil :**
1. **Quel problème résout-il ?** (pas "j'ai vu sur Twitter", mais "j'ai besoin de X")
2. **Est-il minimal ?** (1-3 outils max pour une mission)
3. **Est-il reproductible ?** (même commande, même output, sur 2 machines)

**La règle :** mieux vaut 3 outils maîtrisés que 30 outils ouverts.

---

## 2. PRÉREQUIS

- TUTORIAL_01 + TUTORIAL_02 complétés
- Python 3.10+ avec stdlib
- `ghosteye` installé
- Un éditeur de texte (vim, nano, VSCode)

---

## 3. THÉORIE — Anatomie d'un bon outil

### Critères GHOST1O1
- ✅ **Mono-fichier** (ou dossier clair)
- ✅ **Zero dépendance cachée** (que la stdlib si possible)
- ✅ **Output structuré** (JSON de préférence)
- ✅ **--help clair** avec exemples
- ✅ **Mode dry-run** (voir sans toucher)
- ✅ **Logs horodatés** (timestamp ISO)
- ❌ Pas de framework lourd (Django, Flask pour 1 endpoint)
- ❌ Pas de GUI native (browser = UI universelle)
- ❌ Pas de "magic" (chaque paramètre explicité)

---

## 4. PRATIQUE — Construis ton premier outil GHOST1O1

### Étape 1 — Le squelette

```python
#!/usr/bin/env python3
"""ghosteye-check — Teste la présence d'un service GHOSTEYE
Usage:
  python3 ghosteye_check.py 192.168.1.77 8082
  python3 ghosteye_check.py --json target 8082
"""
import sys
import json
import socket
import argparse
from datetime import datetime, timezone

def log(msg, level="info"):
    ts = datetime.now(timezone.utc).isoformat()
    print(f"[{ts}] [{level}] {msg}")

def check_port(ip, port, timeout=3):
    """Test si un port TCP est ouvert."""
    try:
        with socket.create_connection((ip, port), timeout=timeout) as s:
            return True
    except (socket.timeout, ConnectionRefusedError, OSError):
        return False

def check_ghosteye(ip, port):
    """Vérifie que ghosteye_proxy.py répond."""
    import urllib.request
    try:
        url = f"http://{ip}:{port}/health"
        with urllib.request.urlopen(url, timeout=3) as r:
            data = json.loads(r.read())
            return data
    except Exception as e:
        return {"error": str(e)}

def main():
    parser = argparse.ArgumentParser(description="Test GHOSTEYE presence")
    parser.add_argument("ip", help="IP cible")
    parser.add_argument("port", type=int, nargs="?", default=8082, help="Port (défaut: 8082)")
    parser.add_argument("--json", action="store_true", help="Output JSON")
    args = parser.parse_args()

    result = {
        "target": args.ip,
        "port": args.port,
        "port_open": check_port(args.ip, args.port),
        "ghosteye_response": None,
        "timestamp": datetime.now(timezone.utc).isoformat()
    }

    if result["port_open"]:
        result["ghosteye_response"] = check_ghosteye(args.ip, args.port)

    if args.json:
        print(json.dumps(result, indent=2))
    else:
        log(f"Target: {args.ip}:{args.port}")
        log(f"Port open: {result['port_open']}")
        if result["ghosteye_response"]:
            log(f"Response: {result['ghosteye_response']}")

    sys.exit(0 if result["port_open"] else 1)

if __name__ == "__main__":
    main()
```

**Output :**
```bash
$ python3 ghosteye_check.py 192.168.1.77 8082
[2026-07-05T...] [info] Target: 192.168.1.77:8082
[2026-07-05T...] [info] Port open: True
[2026-07-05T...] [info] Response: {'status': 'ok', 'version': '3.0'}

$ python3 ghosteye_check.py 192.168.1.77 8082 --json
{
  "target": "192.168.1.77",
  "port": 8082,
  "port_open": true,
  "ghosteye_response": {
    "status": "ok",
    "version": "3.0"
  },
  "timestamp": "2026-07-05T..."
}
```

**Ce que tu as :** un outil **mono-fichier**, **zero dépendance externe** (que stdlib), **--help**, **--json**, **logs horodatés**, **exit code cohérent**.

### Étape 2 — Version dry-run

Ajoute un flag `--dry-run` qui simule sans exécuter :

```python
parser.add_argument("--dry-run", action="store_true", help="N'exécute rien, affiche le plan")
```

```python
if args.dry_run:
    log(f"[DRY-RUN] Would test {args.ip}:{args.port}", "warn")
    log(f"[DRY-RUN] Would call check_port + check_ghosteye", "warn")
    sys.exit(0)
```

### Étape 3 — Documentation intégrée

Le `"""docstring"""` au début **est** ta doc. `--help` la lit automatiquement.

```python
"""ghosteye-check v1.0 — Teste la présence d'un service GHOSTEYE
Usage:
  python3 ghosteye_check.py 192.168.1.77 8082
  python3 ghosteye_check.py --json target 8082
  python3 ghosteye_check.py --dry-run target 8082

Exit codes:
  0 = service GHOSTEYE détecté et fonctionnel
  1 = service absent ou non-GHOSTEYE
  2 = erreur réseau

Author: ghost1o1 — 2026
"""
```

### Étape 4 — Publication

```bash
# Crée un repo pour ton outil
mkdir ghosteye-check && cd ghosteye-check
mv ~/ghosteye_check.py .
cat > README.md << 'EOF'
# ghosteye-check

Teste si un service GHOSTEYE tourne sur une cible.

## Usage
```bash
python3 ghosteye_check.py 192.168.1.77 8082
```
EOF

git init && git add . && git commit -m "Initial commit"
gh repo create ghosteye-check --public --source=. --push
```

**Ce que tu as :** un outil publié, documenté, reproductible.

---

## 5. PIÈGES

| Piège | Solution |
|-------|----------|
| Trop de features | MVP d'abord, features ensuite |
| Dépendances pip partout | Stdlib only si possible |
| Pas de mode dry-run | Toujours l'ajouter |
| Output illisible | JSON option `--json` |
| Pas de exit code | Convention 0=OK, 1=absent, 2=erreur |

---

## 6. ALTERNATIVES — Frameworks d'outils

### A — Click (Python)
Si tu veux plus de structure :
```python
import click
@click.command()
@click.argument("ip")
@click.argument("port", default=8082)
def check(ip, port):
    """Check GHOSTEYE on target."""
    ...
```

### B — Typer (Python moderne)
Plus typé, plus rapide à écrire.

### C — Bash + jq
Si tu veux zero Python :
```bash
#!/bin/bash
IP=$1; PORT=${2:-8082}
curl -s --max-time 3 http://$IP:$PORT/health | jq .
```

**Mon choix :** Python stdlib pour 90% des cas. Bash pour les one-liners.

---

## 7. TRANSMISSION

### Exercise
1. Écris ton propre outil GHOST1O1 (un scanner, un testeur, un wrapper)
2. Respecte les 6 critères (mono-fichier, stdlib, --help, --json, dry-run, logs)
3. Publie-le en repo GitHub avec README
4. **Partage le lien** dans Discussions du hub

### Pourquoi
> Un outil que tu n'as pas publié est un outil que tu devras réécrire. Un outil publié devient **patrimoine collectif**.

---

## 📚 Suite

- **TUTORIAL_04** (à venir) — EXPLOITER : preuve, pas destruction
- **TUTORIAL_05** (à venir) — PARTAGER : transmets ta méthode

---

*"There is no lock." — ghost1o1*
