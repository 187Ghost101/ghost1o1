# 🔱 L'ÉVEIL NOCTURNE — Guide de Voix

> *Comment parler comme le mouvement. Pour tous ceux qui écrivent, postent, enseignent.*

---

## Le ton en 3 mots

**Direct. Sobre. Lumineux.**

Pas de hype. Pas de drama. Pas de gatekeeping verbal. Pas de condescendance.

On parle comme à un ami à 3h du matin. Pas comme à un client. Pas comme à un élève. Comme à un **pair**.

---

## ✅ Ce qu'on fait

| Fais | Pourquoi |
|------|----------|
| **Tutoie** (1ère personne: "tu") | C'est plus rapide, plus proche, plus honnête |
| **Phrases courtes** | Le lecteur scanne, pas lit |
| **Impératif direct** | "Fais ça. Maintenant." au lieu de "il faudrait faire" |
| **Métaphores** | Une image vaut 1000 mots techniques |
| **Nomme les gens** | "Comme ghost1o1 l'a montré" > "il a été montré" |
| **Assume l'émotion** | "C'est excitant" > "c'est intéressant" |
| **Pose des questions** | "Tu vois où je veux en venir ?" |

---

## ❌ Ce qu'on évite

| Évite | Pourquoi |
|-------|----------|
| "Dans ce tutoriel nous allons voir" | Intro inutile, on y va direct |
| "C'est très simple" | Condescendant, et souvent faux |
| "Bonne chance !" | Patronisant |
| "Pour plus d'informations" | Vague, paresseux |
| "En conclusion" | Résume au lieu de conclure |
| Phrases > 30 mots | Le lecteur décroche |
| "Nous allons apprendre" | Scolaire, distant |
| Vouvoiement distant | Crée une barrière artificielle |

---

## Les phrases signature

À utiliser **au moins une fois** par document officiel :

> *There is no lock.*
> *Du silence naît la lumière.*
> *Là où l'ignorance dort, nous allumons.*

**Une seule règle :** tu peux les utiliser librement, tu ne les changes pas.

---

## Le mouvement en 1 phrase

> *L'ÉVEIL NOCTURNE est un mouvement open-source qui refuse le gatekeeping dans l'éducation hacker.*

## Le mouvement en 3 phrases

> *L'ÉVEIL NOCTURNE, c'est 6 outils open-source, 1 méthodologie en 5 phases, 0 paywall, et ∞ divergence.*
>
> *C'est construit pour les brebis noires, les curieux solitaires, et ceux qui ne rentrent dans aucune case.*
>
> *There is no lock. Du silence naît la lumière.*

## Le mouvement en 1 paragraphe

> L'ÉVEIL NOCTURNE est un mouvement open-source qui pose une question simple : pourquoi 95% des newbees qui découvrent le hacking quittent en moins de 6 mois ? La réponse documentée est dans notre manifeste. La solution est dans 6 outils MIT-licensed, une méthodologie en 5 phases (Observer / Cartographier / Instrumenter / Exploiter / Partager), et un serment de 4 actes : ne pas cacher, ne pas détruire, ne pas mentir, ne pas gater. Le mouvement est signé `ghost1o1` et vit sur github.com/187Ghost101/ghost1o1. Là où l'ignorance dort, nous allumons.

---

## Le Manifeste en 1 ligne

> *On ne forme pas des exécutants. On éveille des architectes de la divergence.*

## Le Protocole en 1 ligne

> *Observer avant d'agir. Cartographier avant de toucher. Instrumenter le minimum. Exploiter avec preuve. Partager sans rétention.*

## Le Serment en 1 ligne

> *Je ne cache pas. Je ne détruis pas. Je ne mens pas. Je ne gates pas.*

---

## L'écriture d'un tutorial — structure fixe

**Section 1 — PHILOSOPHIE**
> *Pourquoi cette technique existe. Pas "ce que c'est", mais "pourquoi ça compte". 2 min de lecture max.*

**Section 2 — PRÉREQUIS**
> *Ce que tu dois savoir avant. Liens, pas de blabla. Liste courte.*

**Section 3 — THÉORIE**
> *Comment ça marche VRAIMENT. Mécanique, pas magie. Avec schémas si possible.*

**Section 4 — PRATIQUE**
> *Commande par commande. Output attendu après chaque commande. Pas de "ça devrait marcher".*

**Section 5 — PIÈGES**
> *Ce qui va casser. Comment éviter. Liste des erreurs classiques avec solutions.*

**Section 6 — ALTERNATIVES**
> *2-3 autres approches. La divergence n'est pas optionnelle. C'est la philosophie.*

**Section 7 — TRANSMISSION**
> *Exercise concret. "Trouve X, fais Y, publie Z." Cette section est NON-NÉGOCIABLE. C'est la transmission qui crée la scène.*

---

## L'écriture d'un commit message

**Format GHOST1O1 :**
```
[tag] description courte (50 chars max)

Explication si besoin (72 chars par ligne max)
- Ce que ça change
- Pourquoi

Refs: #issue, #discussion
Signed-off-by: Ton Nom <email>
```

**Tags standards :**
- `[fix]` — correction de bug
- `[feat]` — nouvelle feature
- `[docs]` — documentation
- `[style]` — formatting
- `[refactor]` — refactor sans changement de comportement
- `[test]` — ajout de tests
- `[chore]` — maintenance

**Exemples :**
```
[fix] em-dash causes SyntaxError in bytes literal

Replaced U+2014 (—) with ASCII hyphen (-) in ghosteye_proxy.py
line 160. The non-ASCII character prevented module import.

This was the root cause of the v6.1 "buttons not rendering"
bug — the proxy never started, so the page never loaded.

Refs: #42
```

---

## L'écriture d'un post social

**Hook (1ère ligne) :** 5 mots max. Doit accrocher.

> ❌ "Voici mon nouveau projet"
> ✅ "On vous a menti."

**Développement :** 2-3 phrases courtes.

**CTA (call to action) :** 1 phrase claire.

**Hashtags :** 2-3 max. Pas plus.

**Exemple :**
```
On vous a menti.

Le savoir n'est pas une propriété.
Le hack n'est pas un privilège.
L'éveil n'est pas une récompense.

Là où l'ignorance dort, nous allumons.
github.com/187Ghost101/ghost1o1

#LEveilNocturne #ThereIsNoLock
```

---

## L'écriture d'un email

**Sujet :** clair, pas clickbait.

> ❌ "URGENT !!! Découvrez le truc INCROYABLE..."
> ✅ "L'ÉVEIL NOCTURNE — un mouvement qui pourrait t'intéresser"

**1ère ligne :** pourquoi tu contactes CETTE personne spécifiquement.

> "Je te suis depuis ton talk à DEF CON sur..."

**Corps :** court. 100-200 mots max. Donne avant de demander.

**CTA :** 1 seule action possible. Pas 5.

> "Si tu regardes et que ça te parle, même un mot suffit."

**Signature :** sobre.

> — ghost1o1
> github.com/187Ghost101/ghost1o1
> "There is no lock."

---

## L'écriture d'un commentaire de contribution

Quand tu commentes un PR, suis cette structure :

```
**[verdict]** — 1 ligne

**Ce qui marche :**
- point 1
- point 2

**Ce qui pourrait être mieux :**
- point 1
- suggestion concrète

**Action :** [approve | request changes | comment]
```

Pas de "looks good" vague. Pas de "hmm peut-être" sans suite. **Sois précis, sois utile, sois bref.**

---

## La signature type pour tes propres messages

Quand tu parles du mouvement dans tes propres posts / tutos / vidéos, finis par :

```
🔱 L'ÉVEIL NOCTURNE
github.com/187Ghost101/ghost1o1
"There is no lock."
```

Court. Reconnu. Rappel.

---

## Le serment à signer (avant ton premier post officiel)

```
✦ SERMENT L'ÉVEIL NOCTURNE ✦

Je, [ton nom/pseudo], reconnais que :
- Je ne cache pas. Je documente et partage.
- Je ne détruis pas. Je teste sur mes cibles ou je signale.
- Je ne mens pas. Je dis "je ne sais pas" quand c'est le cas.
- Je ne gates pas. Le savoir est un commons.

Signé le [date], pour le mouvement L'ÉVEIL NOCTURNE.
```

---

**Tu as maintenant le ton. Pas la marque. Pas le logo. Le TON.**

**Le ton, c'est ce qui fait que les gens restent ou partent.** 🔱
