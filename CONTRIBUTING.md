# 🤝 CONTRIBUTING — GHOST1O1 NOCTURNE

> *Trois règles. Pas de drama. Pas de gatekeeping.*

---

## Règle 1 — Lis avant de toucher

Avant de proposer un PR :

1. **Lis le code** du fichier que tu modifies
2. **Lis le README + INSTALL + USAGE** du repo
3. **Teste** sur au moins 2 OS différents
4. **Documente** chaque changement

Si tu n'as pas fait ces 4 étapes, ton PR sera fermé sans review.

---

## Règle 2 — Respecte le style

### Python
- PEP 8 (avec quelques tolérances pour la lisibilité)
- Docstrings pour toute fonction publique
- Type hints sur les fonctions core
- Stdlib only par défaut ; dépendances pip justifiées

### Markdown / Docs
- Titres en `#`/`##`/`###` (pas de bold-as-titre)
- Code blocks avec langage explicite (` ```bash `, ` ```python `)
- Liens absolus vers les autres repos GHOST1O1
- Pas de blabla introductif — va au but

### Commits
Format : `type(scope): description`
- `feat(ghosteye): add Shodan panel`
- `fix(ghosteye): em-dash bytes literal crash`
- `docs(readme): update install for macOS`
- `chore: bump version to 3.1`
- `test: add unit tests for ONVIF probe`

---

## Règle 3 — Teste et documente

### Tests
- Tests unitaires pour les fonctions de logique (pas le GUI)
- Test manuel : au moins 3 scénarios dans ton PR
- Format : paste le output dans la description du PR

### Documentation
- Si tu ajoutes une feature → update le README + USAGE
- Si tu casses un truc → update le CHANGELOG
- Si tu changes l'install → update INSTALL.md

---

## Process de contribution

```
1. Fork le repo
2. Crée une branche : git checkout -b feat/ma-feature
3. Code + tests
4. Vérifie : bash install.sh && python3 ...
5. Commit : feat(scope): description
6. Push : git push origin feat/ma-feature
7. PR avec :
   - Description claire
   - Issues résolues (Fixes #123)
   - Screenshots / output si UI
   - Liste des OS testés
8. Review + merge
```

---

## Types de contributions bienvenues

### 🐛 Bug reports
- Output complet
- Steps to reproduce
- Environnement (OS, version Python, version outil)
- Comportement attendu vs observé

### 💡 Features
- Cas d'usage concret
- Mockup / sketch si UI
- Pas de "ça serait cool si" sans justification

### 📚 Documentation
- Traductions (FR, EN, ES minimum)
- Tutorials (structure 7-sections obligatoire)
- Clarifications de commandes obscures

### 🔧 Code
- Refactor (avec tests)
- Performance (avec benchmarks)
- Sécurité (audit, fix)

### ❌ Pas bienvenue
- Drama, ego, disputes dans les issues
- PRs massifs sans discussion préalable
- Refactor cosmétique sans valeur
- Ajout de dépendances lourdes sans justification

---

## Structure des issues

### Bug report
```markdown
**Environnement**
- OS: Kali 2026.2
- Python: 3.12
- Version: ghosteye v3.0

**Steps to reproduce**
1. ...
2. ...
3. ...

**Comportement attendu**
...

**Comportement observé**
...

**Output**
```
paste here
```

**Screenshots**
si applicable
```

### Feature request
```markdown
**Problème**
Quel problème ça résout ?

**Solution proposée**
Comment tu vois la feature

**Alternatives considérées**
Autres approches

**Use case**
Exemple concret d'utilisation
```

---

## Conventions de code

### Variables et fonctions
```python
# Bon
def check_ghosteye_health(ip: str, port: int = 8082) -> dict:
    """Test if GHOSTEYE is running on target."""
    ...

# Pas bon
def chk(i, p=8082):
    ...
```

### Erreurs
```python
# Bon — explicite
try:
    r = requests.get(url, timeout=3)
    r.raise_for_status()
except requests.exceptions.Timeout:
    log("Request timed out", "warn")
    return None
except requests.exceptions.HTTPError as e:
    log(f"HTTP error: {e}", "error")
    return None

# Pas bon — catch all silencieux
try:
    ...
except:
    pass
```

### Logs
```python
# Bon — structuré
log(f"[{target}] Port scan complete: {len(results)} ports", "info")

# Pas bon — print brut
print("done")
```

---

## Reconnaissance

Tes contributions sont reconnues dans :
- `CONTRIBUTORS.md` du repo concerné
- Section "Crédits" du CHANGELOG
- Mentions dans les release notes

Les contributions significatives (feature majeure, tutorial phare) te donnent **droit de commit** sur le repo concerné.

---

## Code de conduite

Pas de :
- Harcèlement, discrimination, attaque personnelle
- Gatekeeping ("tu n'es pas prêt")
- Spam / pub / hors-sujet
- Plagiat (cite tes sources)

Oui à :
- Critique constructive
- Mentorat
- Questions (même "idiotes" — y a pas de question idiote)
- Humilité et honnêteté sur ses limites

---

## Contact

- **Discussions** : [github.com/187Ghost101/ghost1o1/discussions](https://github.com/187Ghost101/ghost1o1/discussions)
- **Issues** : par repo
- **Mentorat** : DM [@187Ghost101](https://github.com/187Ghost101) (réponse sous 7 jours)

---

*"There is no lock." — et il y a toujours une porte pour contribuer.*

**— ghost1o1**
