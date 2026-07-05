# 🔱 TUTORIAL 01 — OBSERVER

## *La première phase du protocole GHOST1O1*

> *Le tireur d'élite observe sa cible 10x plus longtemps qu'il ne tire.*

**Série :** Protocole GHOST1O1
**Niveau :** Débutant → Intermédiaire
**Durée :** 45-60 minutes
**Prérequis :** Linux/Kali basics, un réseau de test

---

## 1. PHILOSOPHIE

**Pourquoi "Observer" en premier ?**

Dans 95% des pentests ratés, la cause est la même : on a tiré avant d'avoir vu. On a scanné tout /24 sans comprendre ce qu'on cherche. On a lancé un exploit sur un service qu'on n'a pas identifié. Résultat : on a perdu du temps, déclenché des alertes, et on n'a souvent rien appris.

**Observer, c'est pas passif.** C'est l'étape la plus active de toute la mission. C'est là où tu passes **plus de temps à regarder qu'à toucher**. C'est là où tu dessines la cible dans ta tête avant d'agir.

**Trois erreurs classiques que ce tutoriel corrige :**
- ❌ "Je scanne tout" → le bruit te fait perdre l'information
- ❌ "Je vais direct à l'exploit" → tu casses sans comprendre
- ❌ "J'observe avec un seul outil" → chaque outil a un angle mort

**L'objectif de ce tutoriel :** tu sors avec une **méthode d'observation** applicable immédiatement, qui marche sur n'importe quelle cible (caméra, routeur, app web, réseau d'entreprise).

---

## 2. PRÉREQUIS

### Connaissances
- Commandes Linux de base (`ls`, `cd`, `cat`, `grep`)
- Notion de réseau (IP, port, protocole)
- Avoir déjà utilisé `nmap` (même rapidement)

### Matériel
- Une machine Linux (Kali recommandé, Ubuntu OK, Arch OK)
- Une **cible de test** : ton propre réseau / un lab / un service de test public
- 2-3h devant toi (ce tutoriel + la pratique)

### Outils
- `nmap` (scan réseau)
- `ghosteye` (notre outil GHOST1O1 — optionnel mais recommandé)
- `netdiscover` ou `arp-scan` (ARP discovery)
- `tshark` ou `tcpdump` (capture passive)
- Un éditeur de notes (Obsidian, Notion, vim, même un .txt)

### Cible d'entraînement (si t'as rien)

**Option A — Ton LAN :**
```bash
ip route | grep default
# Note l'IP de ta passerelle (souvent 192.168.1.1 ou 10.0.0.1)
# C'est un bon point de départ "safe"
```

**Option B — Service de test public :**
- `scanme.nmap.org` (Nmap's officiel test target)
- `testphp.vulnweb.com` (Acunetix)
- `hackthebox.com` (machine d'entraînement)

**Option C — Ton propre service :**
```bash
# Lancer un faux service en local
python3 -m http.server 8000
# Ou
nc -lvp 9999
```

---

## 3. THÉORIE — Comment ça marche VRAIMENT

### Le modèle OSI et où on regarde

```
OSI Layer    | Protocol    | Outil
-------------|-------------|------------------
7. App       | HTTP, RTSP  | curl, navigateur, ghosteye
6. Présent.  | TLS         | openssl s_client
5. Session   | -           | -
4. Transport | TCP, UDP    | nmap -sT/-sU
3. Réseau    | IP, ICMP    | nmap -sn, ping, traceroute
2. Liaison   | ARP, MAC    | arp-scan, netdiscover
1. Physique  | -           | iwconfig, rfkill
```

**Observer = passer au moins une fois par chaque couche.**

### Les trois types d'observation

**1. Passive (zéro interaction avec la cible) :**
- Écoute du trafic (tshark, tcpdump)
- Captures ARP (arp-scan en listen-only)
- OSINT (whois, DNS, certs)

**Avantage :** indétectable. **Limite :** tu ne vois que ce qui passe.

**2. Active slow (interaction minimale) :**
- Ping simple (`-sn`)
- DNS queries
- HTTP HEAD (pas GET)

**Avantage :** déjà plus d'info. **Détectable :** oui, mais discrète.

**3. Active full (scan complet) :**
- Nmap version detection (`-sV`)
- Nmap scripts (`-sC`)
- Brute force (ghosteye, hydra)

**Avantage :** exhaustif. **Détectable :** très visible dans les logs.

**Règle GHOST1O1 :** commence toujours **passive**, puis **active slow**, puis **active full** seulement si nécessaire.

### Le concept de "surface d'attaque"

À la fin de l'observation, tu dois pouvoir répondre à :

1. **Quelles machines ?** (hosts up)
2. **Quels services ?** (ports ouverts)
3. **Quelles versions ?** (banner grabbing)
4. **Quelles relations ?** (qui parle à qui)
5. **Quels flux ?** (volume, fréquence, protocoles)
6. **Quels points d'attention ?** (versions vulnérables, services exposés)

**Ces 6 réponses = la base de tout le reste.**

---

## 4. PRATIQUE — Pas à pas

### Étape 1 — Setup et cible

```bash
# Crée ton dossier de mission
mkdir -p ~/mission_observer
cd ~/mission_observer
export MISSION=$(pwd)

# Note la date
date > mission_meta.txt
echo "Cible: 192.168.1.0/24" >> mission_meta.txt
```

### Étape 2 — Observation passive (15 min)

```bash
# ARP discovery (silent listen)
sudo arp-scan --localnet --quiet | tee ${MISSION}/01_arp.txt
# Output:
# 192.168.1.1   aa:bb:cc:dd:ee:ff   (Unknown)
# 192.168.1.77  11:22:33:44:55:66   (Hikvision)
# 192.168.1.88  99:88:77:66:55:44   (Dahua)

# Note ce que tu as vu (5 hosts par exemple)
echo "[+] 5 hosts discovered" >> ${MISSION}/mission_meta.txt
```

**Alternative (sans sudo) :**
```bash
# Netdiscover en mode passif
netdiscover -p -r 192.168.1.0/24
```

### Étape 3 — Capture passive du trafic (5 min)

```bash
# Lance une capture en background (10s)
sudo timeout 10 tshark -i eth0 -a duration:10 -w ${MISSION}/02_capture.pcap

# Analyse les hosts qui parlent
tshark -r ${MISSION}/02_capture.pcap -q -z conv,ip | tee ${MISSION}/03_conversations.txt
# Output:
# Conversations IP
# 192.168.1.77 <-> 192.168.1.1   50 packets
# 192.168.1.88 <-> 8.8.8.8       12 packets
# ...
```

**Ce que tu apprends :** qui initie les connexions, vers où, à quelle fréquence. **C'est la VRAIE topologie de la cible.**

### Étape 4 — Active slow (10 min)

```bash
# Ping sweep (détecte les hosts up)
nmap -sn 192.168.1.0/24 | tee ${MISSION}/04_ping_sweep.txt

# Pour chaque host, DNS reverse
for ip in $(cat ${MISSION}/04_ping_sweep.txt | grep "Nmap scan report" | awk '{print $NF}'); do
  echo "=== $ip ===" | tee -a ${MISSION}/05_dns_reverse.txt
  dig -x $ip +short | tee -a ${MISSION}/05_dns_reverse.txt
done
```

**Output exemple :**
```
=== 192.168.1.77 ===
hikvision-cam-1.lan.
```

→ Tu sais maintenant que **192.168.1.77 = une caméra Hikvision**.

### Étape 5 — Active full ciblé (15 min)

**⚠️ Ne scanner QUE les hosts qui ont une raison d'être scannés.**

```bash
# Pour chaque caméra identifiée
GHosteye="192.168.1.77"

# Service version
nmap -sV -p 80,554,8899,37777 ${GHOST} | tee ${MISSION}/06_nmap_${GHOST}.txt

# Scripts par défaut
nmap -sC -p 80 ${GHOST} | tee -a ${MISSION}/06_nmap_${GHOST}.txt

# Avec ghosteye (si installé)
python3 ghosteye_proxy.py 8082 &
curl -s -X POST http://localhost:8082/onvif/probe \
  -H 'Content-Type: application/json' \
  -d "{\"ip\":\"${GHOST}\"}" | python3 -m json.tool
```

**Output type :**
```json
{
  "ip": "192.168.1.77",
  "manufacturer": "Hikvision",
  "model": "DS-2CD2142FWD-I",
  "firmware": "V5.5.0 build 210628",
  "serial": "DS-2CD2142FWD-I20210628"
}
```

→ Tu as maintenant **modèle + firmware exact**.

### Étape 6 — Note de mission structurée

Crée `${MISSION}/RAPPORT_OBSERVATION.md` :

```markdown
# Mission Observation — [DATE]
**Opérateur :** ghost1o1
**Cible :** 192.168.1.0/24

## Hosts découverts (passive ARP)
| IP | MAC | Hostname suspecté |
|----|-----|-------------------|
| 192.168.1.1 | aa:bb:cc:dd:ee:ff | Gateway |
| 192.168.1.77 | 11:22:33:44:55:66 | hikvision-cam-1.lan |
| 192.168.1.88 | 99:88:77:66:55:44 | (à identifier) |

## Services identifiés (active)
| IP | Port | Service | Version |
|----|------|---------|---------|
| 192.168.1.77 | 80 | HTTP | Hikvision-Webs 5.5.0 |
| 192.168.1.77 | 554 | RTSP | Hikvision RTSP 5.5.0 |
| 192.168.1.77 | 8899 | SADP | Hikvision 5.5.0 |

## Points d'attention
- ⚠️ Firmware V5.5.0 : vérifier CVE-2021-36260 (RCE)
- ⚠️ Port 8899 (SADP) exposé sur LAN : informations leak
- ⚠️ RTSP sans auth visible : tester accès direct

## Prochaine étape (phase 2)
- Cartographier les relations entre ces 3 hosts
- Identifier les credentials par défaut
- Tester les CVE potentielles
```

### Étape 7 — Sauvegarde la mission

```bash
# Archive
cd ~
tar czf mission_observer_$(date +%Y%m%d).tar.gz mission_observer/

# Copie vers un endroit safe (cloud, USB, second disk)
```

---

## 5. PIÈGES — Ce qui va casser

### Piège 1 : Scanner trop large

**Symptôme :** tu lances `nmap -p- 192.168.1.0/24` et tu obtiens 50 alertes IDS.

**Solution :** scan ciblé par sous-réseau ou par host :
```bash
# Mauvais
nmap -p- 192.168.1.0/24

# Bon
nmap -sn 192.168.1.0/24  # ping sweep d'abord
nmap -sV 192.168.1.77      # puis ciblé
```

### Piège 2 : Oublier le DNS

**Symptôme :** tu as 5 hosts up mais tu ne sais pas qui est quoi.

**Solution :** toujours faire le reverse DNS en étape 4.
```bash
# Crée un mapping IP → hostname
for ip in $(cat hosts.txt); do
  echo "$ip $(dig -x $ip +short)"
done
```

### Piège 3 : Scanner sans horodater

**Symptôme :** tu ne sais plus quand tu as scanné quoi, et la cible a peut-être changé.

**Solution :** chaque commande de scan doit être horodatée.
```bash
echo "[$(date -Iseconds)] nmap -sV ${IP}" >> mission.log
nmap -sV ${IP} | tee -a mission.log
```

### Piège 4 : Ignorer IPv6

**Symptôme :** la cible a IPv6 actif mais tu n'as scanné qu'IPv4.

**Solution :** au minimum, identifie si IPv6 est utilisé :
```bash
ping6 ff02::1%eth0  # multicast, liste tous les hosts IPv6 sur le lien
```

### Piège 5 : Oublier de sauvegarder

**Symptôme :** tu perds tes notes après un crash ou un reboot.

**Solution :** **toujours** archiver la mission à la fin.

---

## 6. ALTERNATIVES — 3 autres approches

### Alternative A — Automatisée avec GHOSTEYE

```bash
# Discovery + ONVIF + RTSP brute en une commande
python3 ghosteye_proxy.py 8082 &
sleep 2
curl -X POST http://localhost:8082/onvif/discover -H 'Content-Type: application/json' -d '{}'
curl -X POST http://localhost:8082/rtsp/brute -H 'Content-Type: application/json' -d '{"ip":"192.168.1.77"}'
```

**Avantage :** tout en un, plus rapide. **Limite :** moins granulaire que la méthode manuelle.

### Alternative B — Nmap scripting engine (NSE)

```bash
# Utilise les scripts Nmap pour aller plus loin
nmap --script=http-enum,http-title,ssl-cert -p 80,443 192.168.1.77
```

**Avantage :** extensible, scripts communautaires. **Limite :** certains scripts sont bruyants.

### Alternative C — Avec Masscan (très rapide)

```bash
# Masscan = nmap en 100x plus rapide (mais moins précis)
sudo masscan -p1-65535 192.168.1.0/24 --rate=1000
```

**Avantage :** scanne un /16 en minutes. **Limite :** très bruyant, facile à détecter.

**Règle de divergence :** essaie au moins 2 méthodes différentes sur la même cible. Compare les résultats.

---

## 7. TRANSMISSION — Ton exercise

### 🎯 Mission à accomplir

**Trouve 1 CVE exploitable sur une caméra IoT de ton réseau, en suivant UNIQUEMENT la phase 1 du protocole.**

**Étapes :**
1. **Observe** : scan passif + actif de ton LAN
2. **Identifie** : au moins 1 caméra IoT (Hikvision, Dahua, ou autre)
3. **Documente** : sa marque, modèle, firmware
4. **Matche** : trouve au moins 1 CVE correspondante (cherche sur nvd.nist.gov ou cvedetails.com)
5. **Note** : si tu devais l'exploiter, par où tu commencerais (sans le faire)

**Livrable :** publie un fichier `MISSION_OBSERVATION.md` sur ton propre repo ou gist, avec :
- Hosts découverts
- Services identifiés
- Modèle + firmware + CVE
- Notes sur la suite

### 📤 Comment transmettre

- Crée un gist : https://gist.github.com
- Push sur ton propre repo
- Partage le lien dans les Discussions de GHOST1O1 : https://github.com/187Ghost101/ghost1o1/discussions

### 🏅 Crédits

Les transmissions sont **nommées** dans le repo. Si ton travail est solide, tu es crédité dans le CHANGELOG et dans la page Contributors.

**C'est ça, la transmission. C'est pas optionnel. C'est la 5e phase du protocole.**

---

## 📚 Pour aller plus loin

- **TUTORIAL 02 — CARTOGRAPHIER** : la phase suivante
- **ghosteye/README.md** : l'outil utilisé dans ce tuto
- **PROTOCOL.md** : la méthodologie complète

---

## 📜 Sources & Références

- [Nmap Reference Guide](https://nmap.org/book/man.html)
- [NIST CVE Database](https://nvd.nist.gov/)
- [Hikvision Security Advisories](https://www.hikvision.com/en/support/cybersecurity/)
- [Sans IoT Security Resources](https://www.sans.org/iot-security/)

---

<div align="center">

**L'ÉVEIL NOCTURNE** · [ghost1o1](https://github.com/187Ghost101) — 2026

*There is no lock. Du silence naît la lumière.*

</div>
