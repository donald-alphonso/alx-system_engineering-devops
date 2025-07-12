# DNS Subdomain Audit Script

## 📋 Description

Ce projet implémente un script Bash pour auditer et afficher les informations DNS des sous-domaines d'un domaine donné. Il fait partie d'une infrastructure de load balancing où différents sous-domaines pointent vers différents serveurs.

## 🎯 Objectifs pédagogiques

* Comprendre le fonctionnement du DNS (Domain Name System)
* Maîtriser l'utilisation de la commande `dig`
* Apprendre à traiter les données avec `awk`
* Développer des fonctions Bash réutilisables
* Gérer les paramètres en ligne de commande

## 🏗️ Architecture DNS

```
┌─────────────────┐    ┌─────────────────┐
│   www.domain    │───▶│   Load Balancer │
│   lb-01.domain  │───▶│     (lb-01)     │
└─────────────────┘    └─────────────────┘
                              │
                              ▼
                   ┌─────────────────┐
                   │   Distribution  │
                   └─────────────────┘
                           │
              ┌────────────┴────────────┐
              ▼                         ▼
    ┌─────────────────┐       ┌─────────────────┐
    │   web-01.domain │       │   web-02.domain │
    │   Web Server 1  │       │   Web Server 2  │
    └─────────────────┘       └─────────────────┘
```

## 📁 Structure du projet

```
0x10-https_ssl/
├── 0-world_wide_web           # Script principal
└── README.md                  # Documentation
```

## 🔧 Configuration DNS requise

### Enregistrements A à configurer :


| Sous-domaine | Type | Destination         |
| ------------ | ---- | ------------------- |
| `www`        | A    | IP du load balancer |
| `lb-01`      | A    | IP du load balancer |
| `web-01`     | A    | IP du serveur web 1 |
| `web-02`     | A    | IP du serveur web 2 |

### Exemple de configuration :

```dns
www.example.com     IN  A   54.210.47.110
lb-01.example.com   IN  A   54.210.47.110
web-01.example.com  IN  A   34.198.248.145
web-02.example.com  IN  A   54.89.38.100
```

## 🚀 Installation et utilisation

### 1. Cloner le repository

```bash
git clone https://github.com/votre-username/alx-system_engineering-devops.git
cd alx-system_engineering-devops/0x10-https_ssl
```

### 2. Rendre le script exécutable

```bash
chmod +x 0-world_wide_web
```

### 3. Utilisation

#### Afficher tous les sous-domaines par défaut :

```bash
./0-world_wide_web example.com
```

**Sortie attendue :**

```
The subdomain www is a A record and points to 54.210.47.110
The subdomain lb-01 is a A record and points to 54.210.47.110
The subdomain web-01 is a A record and points to 34.198.248.145
The subdomain web-02 is a A record and points to 54.89.38.100
```

#### Afficher un sous-domaine spécifique :

```bash
./0-world_wide_web example.com web-02
```

**Sortie attendue :**

```
The subdomain web-02 is a A record and points to 54.89.38.100
```

## 🛠️ Détails techniques

### Fonctionnalités du script :

* **Paramètres** :
  * `domain` (obligatoire) : Nom de domaine à auditer
  * `subdomain` (optionnel) : Sous-domaine spécifique à auditer
* **Comportement** :
  * Sans sous-domaine : affiche `www`, `lb-01`, `web-01`, `web-02` dans cet ordre
  * Avec sous-domaine : affiche uniquement le sous-domaine spécifié
* **Outils utilisés** :
  * `dig` : Interrogation DNS
  * `awk` : Traitement et extraction des données
  * `grep` : Filtrage des résultats

### Structure interne :

```bash
get_subdomain_info() {
    # Fonction pour extraire les informations DNS
    # Utilise dig + grep + awk pour parser les résultats
}

# Logique principale
# Gestion des paramètres et appel de fonction
```

## 🔍 Exemple d'analyse DNS

### Commande `dig` manuelle :

```bash
dig www.holberton.online | grep -A1 'ANSWER SECTION:'
```

### Sortie brute :

```
;; ANSWER SECTION:
www.holberton.online.   87  IN  A   54.210.47.110
```

### Extraction avec `awk` :

* Champ 4 : Type d'enregistrement (`A`)
* Champ 5 : Adresse IP (`54.210.47.110`)

## 📚 Concepts clés

### DNS (Domain Name System)

* Système de résolution de noms de domaines
* Traduit les noms en adresses IP
* Utilise différents types d'enregistrements (A, AAAA, CNAME, MX, etc.)

### Enregistrement A

* Associe un nom de domaine à une adresse IPv4
* Format : `nom TTL classe type adresse`
* Exemple : `www.example.com. 300 IN A 192.168.1.1`

### Load Balancing

* Répartition du trafic entre plusieurs serveurs
* Améliore la disponibilité et les performances
* Permet la redondance en cas de panne

## 🧪 Tests et validation

### Test de base :

```bash
# Vérifier que le script fonctionne
./0-world_wide_web holberton.online

# Test avec sous-domaine spécifique
./0-world_wide_web holberton.online www
```

### Validation DNS :

```bash
# Vérifier manuellement les enregistrements
dig www.votre-domaine.com +short
dig lb-01.votre-domaine.com +short
dig web-01.votre-domaine.com +short
dig web-02.votre-domaine.com +short
```

## 📋 Checklist de validation

* [ ]  Script exécutable (`chmod +x`)
* [ ]  Fonctionne avec domaine seul
* [ ]  Fonctionne avec domaine + sous-domaine
* [ ]  Utilise une fonction Bash
* [ ]  Utilise `awk` pour l'extraction
* [ ]  Ignore `shellcheck SC2086`
* [ ]  Format de sortie correct
* [ ]  Ordre des sous-domaines respecté

## 🐛 Dépannage

### Erreurs communes :

1. **"command not found"** :
   ```bash
   chmod +x 0-world_wide_web
   ```
2. **Aucun résultat DNS** :
   * Vérifier la configuration DNS
   * Tester avec `dig` manuellement
   * Vérifier la connectivité réseau
3. **Format de sortie incorrect** :
   * Vérifier les champs extraits par `awk`
   * Tester la regex `grep`

## 📖 Ressources complémentaires

* [Documentation `dig`](https://linux.die.net/man/1/dig)
* [Guide `awk`](https://www.gnu.org/software/gawk/manual/gawk.html)
* [DNS RFC 1035](https://tools.ietf.org/html/rfc1035)
* [Bash Functions Guide](https://tldp.org/LDP/abs/html/functions.html)

## 📄 Licence

Ce projet fait partie du curriculum ALX System Engineering & DevOps.

---

**Repository:**`alx-system_engineering-devops`
**Directory:**`0x10-https_ssl`
**File:**`0-world_wide_web`
