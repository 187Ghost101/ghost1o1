# 🎓 TUTORIAL 01 — OBSERVER un réseau comme ghost1o1

> **Du WiFi au premier device en 7 étapes**
>
> Méthodologie : Protocole GHOST1O1 · Phase 1
>
> Niveau : Débutant→Intermédiaire · Durée : 45 min
>
> Outils : `ghosteye`, `nmap`, `netdiscover`, `arp-scan`

---

## 1. PHILOSOPHIE

Observer, c'est **comprendre** avant d'agir. C'est la phase 1 du protocole GHOST1O1, et c'est la plus critique.

**Trois erreurs classiques du newbee :**
1. Lancer un scan `-p-` (1-65535) dès l'arrivée sur un réseau. **Bruit, détection, perte de temps.**
2. Croire qu'un seul outil suffit. **Un hacker qui n'a qu'un outil n'est pas un hacker.**
3. Ne jamais cartographier avant d'attaquer. **Tu casses ce que tu ne comprends pas.**

**La règle du tireur d'élite :** tu observes 10× plus longtemps que tu n'agis.

**À la fin de ce tutoriel, tu sauras :**
- Identifier le subnet sur lequel tu te trouves
- Découvrir les devices actifs sans bruit excessif
- Identifier les services exposés
- Comprendre la **logique du réseau** (qui est qui, qui parle à qui)
- Documenter tes observations de manière structurée

---

## 2. PRÉREQUIS

### Connaissances
- Savoir ouvrir un terminal
- Savoir taper `cd`, `ls`, `cat`
- Connaître la différence entre une IP, un port, un protocole

### Outils
- **Kali Linux** (ou Debian/Ubuntu) — [kali.org](https://www.kali.org/get-kali/)
- **Python 3.10+** — préinstallé sur Kali
- **nmap** — préinstallé sur Kali, sinon `sudo apt install nmap`
- **ghosteye** — cloné depuis GitHub

### Réseau
- Un réseau local à auditer (**le tien**, ou un lab)
- Une connexion WiFi ou Ethernet

### ⚠️ Légalité
**Tu ne fais ça QUE sur :**
- Ton propre réseau domestique
- Un lab (VMs isolées)
- Un réseau avec **autorisation écrite**

Auditer le WiFi de ton voisin = **illégal**. C'est pas GHOST1O1, c'est pas du hacking, c'est juste un crime.

---

## 3. THÉORIE — Comment fonctionne un réseau local

### Le modèle OSI en 3 lignes
- **Couche 1 (Physique)** : câbles, ondes WiFi
- **Couche 2 (Liaison)** : adresses MAC, switches
- **Couche 3 (Réseau)** : adresses IP, routeurs
- **Couche 7 (Application)** : HTTP, SSH, RTSP

**Pour observer, tu utilises principalement les couches 2 et 3.**

### ARP — Le protocole magique
**Address Resolution Protocol** (ARP) traduit IP → MAC. Quand un device veut parler à un autre, il demande "qui a 192.168.1.1 ?" sur tout le réseau (broadcast ARP). Le device ciblé répond.

**Conséquence pratique :** tu peux **voir tous les devices actifs** d'un réseau juste en envoyant des requêtes ARP. C'est ce que font `arp-scan` et `netdiscover`.

### TCP/UDP — Les protocoles de transport
- **TCP** : connexion fiable, 3-way handshake (SYN, SYN-ACK, ACK)
- **UDP** : sans connexion, plus rapide, moins fiable

**Pour identifier un service :** tu te connectes sur un port, tu lis le **banner** (la première réponse du serveur).

### Les phases d'un scan TCP (avec nmap)
1. **SYN scan** (`-sS`) : tu envoies SYN, le serveur répond SYN-ACK, tu termines sans ACK → furtif
2. **Connect scan** (`-sT`) : connexion complète, plus visible mais compatible partout
3. **Version detection** (`-sV`) : tu lis le banner pour identifier le service
4. **OS detection** (`-O`) : tu analyses les réponses pour deviner l'OS

---

## 4. PRATIQUE — 7 étapes concrètes

### Étape 1 — Identifier ton réseau

```bash
# Trouve ton IP locale
ip addr show wlan0 | grep inet
# → inet 192.168.1.42/24

# OU plus simple
hostname -I
# → 192.168.1.42

# Ta passerelle (routeur)
ip route | grep default
# → default via 192.168.1.1 dev wlan0
```

**Output attendu :** `inet 192.168.1.42/24` et `default via 192.168.1.1 dev wlan0`

**Ce que tu apprends :** tu es sur `192.168.1.0/24`, ta passerelle est `192.168.1.1`.

---

### Étape 2 — Découverte passive (zéro bruit)

```bash
# Ta table ARP locale (devices que tu as déjà contactés)
ip neigh show
# → 192.168.1.1 dev wlan0 lladdr aa:bb:cc:dd:ee:ff REACHABLE
# → 192.168.1.77 dev wlan0 lladdr 11:22:33:44:55:66 STALE
```

**Ce que tu apprends :** sans rien envoyer, tu sais déjà quels devices tu as contactés récemment. Le routeur + la caméra 192.168.1.77.

**Pourquoi c'est passif :** ta table ARP est construite par le trafic légitime. Tu ne génères aucun paquet.

---

### Étape 3 — Découverte active (faible bruit)

```bash
# Ping sweep du /24
nmap -sn 192.168.1.0/24
```

**Output :**
```
Nmap scan report for 192.168.1.1
Host is up (0.0050s latency).
MAC Address: AA:BB:CC:DD:EE:FF (Tp-link Technologies)
Nmap scan report for 192.168.1.77
Host is up (0.012s latency).
MAC Address: 11:22:33:44:55:66 (Hikvision)
Nmap scan report for 192.168.1.88
Host is up (0.008s latency).
MAC Address: 22:33:44:55:66:77 (Dahua)
Nmap done: 256 IP addresses (4 hosts up) scanned in 3.2 seconds
```

**Ce que tu apprends :** 4 devices up, dont 2 caméras (Hikvision + Dahua identifiées par MAC OUI).

**Niveau de bruit :** faible. `-sn` ne fait que des pings, pas de scan de ports.

---

### Étape 4 — Scan focalisé sur les devices intéressants

```bash
# Scan rapide (top 100 ports) sur les caméras
nmap -sV -T4 192.168.1.77,88
```

**Output :**
```
Nmap scan report for 192.168.1.77
PORT     STATE SERVICE     VERSION
80/tcp   open  http        Hikvision-Webs
554/tcp  open  rtsp        Hikvision RTSP
8899/tcp open  sadp        Hikvision SADP
MAC Address: 11:22:33:44:55:66 (Hikvision)

Nmap scan report for 192.168.1.88
PORT     STATE SERVICE     VERSION
80/tcp   open  http        Dahua-httpd
37777/tcp open  dahua      Dahua DVR
MAC Address: 22:33:44:55:66:77 (Dahua)
```

**Ce que tu apprends :**
- 192.168.1.77 = Hikvision (web + RTSP + SADP)
- 192.168.1.88 = Dahua (web + port DVR propriétaire)

---

### Étape 5 — Identification précise (banner grabbing)

```bash
# HTTP headers
curl -sI http://192.168.1.77
# → Server: Hikvision-Webs/1.0
# → WWW-Authenticate: Basic realm="IP Camera"
# → Connection: close

curl -sI http://192.168.1.88
# → Server: Dahua httpd/1.0
# → WWW-Authenticate: Basic realm="Dahua"
```

**Ce que tu apprends :** les deux caméras utilisent Basic Auth. Le serveur s'identifie clairement → modèles précis possibles.

---

### Étape 6 — Cartographie ONVIF (caméras uniquement)

```bash
# Lance ghosteye
cd ~/ghosteye
python3 ghosteye_proxy.py 8082 &

# Probe ONVIF
curl -X POST http://localhost:8082/onvif/probe \
  -H 'Content-Type: application/json' \
  -d '{"ip":"192.168.1.77"}'
```

**Output :**
```json
{
  "ip": "192.168.1.77",
  "manufacturer": "Hikvision",
  "model": "DS-2CD2142FWD-I",
  "firmware": "V5.5.0 build 170123",
  "serial": "DS-2CD2142FWD-I20180101",
  "mac": "11:22:33:44:55:66"
}
```

**Ce que tu apprends :** modèle exact + version firmware. Avec ça, tu peux chercher les CVE associées.

---

### Étape 7 — Documenter

```bash
# Crée ton rapport
mkdir -p ~/ghost1o1_ops/2026-07-05_audit_home
cd ~/ghost1o1_ops/2026-07-05_audit_home

cat > observations.md << 'EOF'
# Audit réseau domestique — 2026-07-05

## Réseau
- Subnet: 192.168.1.0/24
- Ma machine: 192.168.1.42
- Passerelle: 192.168.1.1 (TP-Link)

## Devices découverts
| IP | MAC | Type | Ports ouverts | Services |
|----|-----|------|---------------|----------|
| 192.168.1.1 | AA:BB:CC:DD:EE:FF | Routeur TP-Link | 80, 443 | HTTP/HTTPS |
| 192.168.1.42 | ma MAC | Mon laptop | - | - |
| 192.168.1.77 | 11:22:33:44:55:66 | Hikvision DS-2CD2142FWD-I | 80, 554, 8899 | HTTP/RTSP/SADP |
| 192.168.1.88 | 22:33:44:55:66:77 | Dahua IPC | 80, 37777 | HTTP/Dahua DVR |

## Findings
- 2 caméras avec credentials par défaut potentiels
- 1 routeur grand public (vulnérabilités possibles selon modèle)
- Firmware Hikvision V5.5.0 (build 170123) = ancien, vérifier CVE

## Prochaines étapes
- Phase 2 (Cartographier) : scan complet + identification CVE
EOF
```

**Ce que tu apprends :** tu as un rapport structuré, daté, exploitable pour la phase 2.

---

## 5. PIÈGES — Ce qui va casser

| Piège | Symptôme | Solution |
|-------|----------|----------|
| Scan `-p-` sur /24 | Trop lent, détection IDS | Utilise `-T4` + ports ciblés |
| WiFi désynchronisé | Perte de packets, scans incomplets | `ping -c 5 8.8.8.8` pour vérifier |
| Routeur qui bloque ICMP | `nmap -sn` voit 0 host | Utilise `arp-scan` à la place |
| Permissions insuffisantes | `nmap: operation not permitted` | `sudo nmap ...` |
| Firewall sur la cible | Ports fermés alors qu'ils sont ouverts | Le firewall bloque SYN scan, essaie `-sT` |
| Faux positifs | nmap voit un port ouvert mais il ne répond pas | Vérifie avec `nc -zv IP PORT` |
| Interface WiFi en mode monitor | nmap ne scan qu'eth0 | `iwconfig` pour vérifier |

---

## 6. ALTERNATIVES — 3 autres approches

### Approche A — `arp-scan` (le plus rapide pour ARP pur)
```bash
sudo arp-scan 192.168.1.0/24
```
**Avantage :** ultra rapide, fonctionne même si ICMP est bloqué
**Inconvénient :** ne donne pas l'OS ni les services

### Approche B — `netdiscover` (avec UI live)
```bash
sudo netdiscover -r 192.168.1.0/24
```
**Avantage :** visuel, bon pour démos
**Inconvénient :** passif par défaut, moins précis que nmap

### Approche C — ghosteye (intégré)
```bash
python3 ghosteye_proxy.py 8082 &
curl -X POST http://localhost:8082/scan/ports \
  -H 'Content-Type: application/json' \
  -d '{"target":"192.168.1.0/24","ports":[80,554,8899,37777]}'
```
**Avantage :** un seul outil, dashboard intégré, ONVIF auto
**Inconvénient :** moins complet que nmap pour OS detection

**Mon choix :** **combo des 3**. `arp-scan` pour le quick win, `nmap -sV` pour la précision, `ghosteye` pour l'ONVIF.

---

## 7. TRANSMISSION — Ton exercise

### Exercice 1 — Audit ton réseau domestique
1. Fais les 7 étapes ci-dessus sur ton propre réseau
2. Documente tout dans `~/ghost1o1_ops/DATE_audit_home/observations.md`
3. Identifie **au moins 1 device** avec un firmware ancien ou credentials par défaut

### Exercice 2 — Compare les 3 approches
1. Refais l'étape 3 (scan /24) avec `nmap -sn`, `arp-scan`, et `netdiscover`
2. Note les différences : vitesse, résultats, bruit
3. **Publie** ta comparaison en issue sur [github.com/187Ghost101/ghost1o1](https://github.com/187Ghost101/ghost1o1)

### Exercice 3 — Identifie 1 CVE réelle
1. Prends le modèle de caméra trouvé (ex: Hikvision DS-2CD2142FWD-I V5.5.0)
2. Cherche sur [cve.mitre.org](https://cve.mitre.org) les CVE associées
3. Vérifie si ta caméra est vulnérable
4. **Publie** ton finding (anonymisé) en discussion

### Pourquoi la section 7 existe

> *"Celui qui sait et ne transmet pas trahit sa propre maîtrise."*

T'as fait le tutoriel. T'as le savoir. Si tu le gardes, il meurt avec la session. Si tu le publies, il aide 1000 autres. **Transmettre, c'est pas de la charité. C'est un devoir de discipline.**

---

## 📚 Pour aller plus loin

- **TUTORIAL_02** — CARTOGRAPHIER : du device au graphe d'attaque
- **TUTORIAL_03** — INSTRUMENTER : choisis tes outils, construis ta trousse
- **ghosteye** : [github.com/187Ghost101/ghosteye](https://github.com/187Ghost101/ghosteye)
- **Protocole complet** : [PROTOCOL.md](https://github.com/187Ghost101/ghost1o1/blob/main/PROTOCOL.md)
- **Manifeste** : [MANIFESTO.md](https://github.com/187Ghost101/ghost1o1/blob/main/MANIFESTO.md)

---

*"There is no lock." — ghost1o1*

**Signé : ghost1o1 — 2026-07-05 — Pour ceux qui osent apprendre.**
