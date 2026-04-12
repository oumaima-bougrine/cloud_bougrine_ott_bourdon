# cloud_bougrine_ott_bourdon
# 📦 Projet Azure – Architecture WordPress

## 🧱 Architecture

Ce projet déploie une architecture complète sur Azure avec :

* 1 VM **jumpoff** (accès SSH)
* 2 VM **app** (WordPress + Nginx + PHP)
* 1 **Load Balancer** (HTTP/HTTPS)
* 1 **MySQL Flexible Server**
* 1 **Private Endpoint** (connexion sécurisée DB)
* Réseau avec 3 subnets :

  * `jumpoff`
  * `app`
  * `data`

---

## ⚙️ Déploiement

Le déploiement est automatisé avec **Ansible** :

```bash
ansible-playbook cloud-playbook.yaml --extra-vars azure_tenant="..." --extra-vars azure_subscription_id="..." --extra-vars azure_client_id="..." --extra-vars azure_secret="..."
```

Le provisioning des VM inclut un **cloud-init** qui :

* installe Nginx, PHP et dépendances
* déploie WordPress
* configure la connexion à la base MySQL
* active HTTP et HTTPS (certificat auto-signé)

---

## 🌐 Accès

* Accès SSH via la VM jumpoff
* Accès web via l’IP publique du Load Balancer :

  * HTTP : port 80
  * HTTPS : port 443

---

## ⚠️ Problème rencontré

### ❌ Erreur MySQL

```text
ProvisionNotSupportedForRegion
Provisioning in requested region is not supported.
```

### 📌 Explication

Cette erreur signifie que :

* La région choisie (`northeurope`) **ne supporte pas MySQL Flexible Server**
* OU
* Le **compte Azure** n’a pas accès à ce service dans cette région

👉 C’est une **limitation Azure (quota / région)** et non une erreur de configuration.

---

## 🔐 Sécurité

* Accès SSH limité à une IP
* Accès DB limité au subnet app
* Communication DB via Private Endpoint
* HTTPS activé (certificat auto-signé)

---

## 🧪 Tests réalisés

* ✔️ Connexion SSH jumpoff → app
* ✔️ Nginx actif sur les VMs app
* ✔️ Load Balancer configuré
* ✔️ Accès HTTP/HTTPS fonctionnel
* ❌ Connexion MySQL bloquée (erreur Azure)

---

## 💬 Remarque

Le projet est fonctionnel côté infrastructure (réseau, VM, LB).
Le blocage MySQL est externe et lié à Azure.

---

---
