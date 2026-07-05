# 🔒 SECURITY — GHOST1O1 NOCTURNE

> *Outils offensifs = responsabilité offensive.*

---

## Politique de sécurité

GHOST1O1 publie des outils de pentest, d'audit et de recherche en sécurité. Comme tout outil offensif, ils peuvent être utilisés à mauvais escient. Cette politique encadre notre approche.

### Principe fondamental

**Tu es 100% responsable de l'usage que tu fais de ces outils.**

Le projet GHOST1O1, son auteur (`ghost1o1`), et ses contributeurs déclinent toute responsabilité en cas d'usage non autorisé, illégal, ou malveillant.

---

## ⚠️ Usage AUTORISÉ

Les outils GHOST1O1 sont destinés exclusivement à :

- ✅ **Lab personnel** (VMs, conteneurs, devices que tu possèdes)
- ✅ **Audit autorisé** (avec **autorisation écrite** explicite)
- ✅ **Recherche en sécurité** (académique, privée, publication)
- ✅ **Formation** (Red Team / Blue Team, CTF, cours)
- ✅ **Bug bounty** (dans le scope autorisé par la plateforme)

## 🚫 Usage INTERDIT

- ❌ Caméras / systèmes **sans autorisation écrite**
- ❌ Attaque de réseaux WiFi d'autrui
- ❌ Phishing réel hors lab / hors autorisation
- ❌ Exfiltration de données sans consentement
- ❌ Diffusion / revente d'outils GHOST1O1 modifiés pour nuire
- ❌ Toute activité illégale au regard des lois en vigueur

**Aucune de ces activités n'est cautionnée par GHOST1O1.**

---

## 🔓 Disclosure responsable

Si tu découvres une vulnérabilité dans un outil GHOST1O1 lui-même :

1. **Ne la publie pas** publiquement avant 90 jours (ou avant qu'un fix soit disponible)
2. **Contacte** : ouvre une issue GitHub avec le tag `security` (privée si possible)
3. **Fournis** : PoC, impact, reproduction steps, environnement
4. **Coordonne** : on travaille ensemble sur le fix avant disclosure publique

### Process

```
1. Tu trouves une vuln → Tu contactes (issue security)
2. On confirme (24-72h)
3. On développe le fix (7-30 jours)
4. Disclosure coordonnée (90 jours max)
5. Crédit dans le CHANGELOG + advisories
```

---

## 🔍 Audit des dépendances

GHOST1O1 utilise **uniquement la stdlib Python** pour les outils core. Les dépendances tierces sont :
- Minimales (`requirements.txt` = pip, pas de Poetry/uv complexe)
- Auditées à chaque release
- Verrouillées par `requirements.lock` quand applicable

### Outils utilisés pour audit
- `pip-audit`
- `safety`
- `trivy` (conteneurs Docker)

---

## 🛡️ Bonnes pratiques pour les utilisateurs

### Avant d'utiliser GHOST1O1 sur une cible

```markdown
- [ ] J'ai l'autorisation écrite du propriétaire
- [ ] J'ai documenté le scope (périmètre, durée, outils autorisés)
- [ ] J'ai préparé une procédure de rollback
- [ ] J'ai un point de contact en cas d'incident
- [ ] J'ai un plan de disclosure pour les findings
```

### Pendant l'audit

- Logs horodatés de **toutes** les actions
- Limitation de l'impact (pas de DoS, pas de wipe)
- Stockage sécurisé des preuves (chiffré, accès restreint)

### Après l'audit

- Rapport dans les **30 jours**
- Anonymisation des données sensibles
- Destruction des preuves après délai convenu

---

## 🐛 Bug bounty pour GHOST1O1

On n'a pas de programme formel, mais :
- Vuln critique dans un outil core → crédit + merci public
- Suggestion d'amélioration → crédit dans le CHANGELOG
- Code contribution → ton nom dans les `CONTRIBUTORS.md`

---

## 📜 Cadre légal

Ce projet est publié sous **MIT License**. Cela signifie :

- ✅ Tu peux utiliser, modifier, distribuer
- ⚠️ **Mais** tu restes responsable de ton usage
- ❌ Les auteurs ne sont **pas responsables** des usages违法违规

**En cas de doute :** consulte un juriste spécialisé en cybersécurité dans ta juridiction.

---

## 📞 Contact

- **Issues security** : [github.com/187Ghost101/ghost1o1/issues/new](https://github.com/187Ghost101/ghost1o1/issues/new) (tag `security`)
- **Email** : voir le profil GitHub [@187Ghost101](https://github.com/187Ghost101)

---

*"There is no lock." — et il y a toujours une façon éthique de l'ouvrir.*

**— ghost1o1**
