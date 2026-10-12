# ais_active_directory_hardening_pingcastle
Audit de sécurité Active Directory avec PingCastle et remédiation méthodique (LAPS, Kerberos, GPO Security).

# 🛡️ Audit & Hardening Active Directory avec PingCastle

## 📋 Présentation du projet
Ce projet réalisé dans le cadre de ma formation consiste à effectuer un audit de sécurité complet d'un domaine Active Directory (`ais.local`), d'en analyser les vulnérabilités et d'appliquer un plan de remédiation méthodique selon les recommandations de l'ANSSI et de Microsoft.

🎯 **Résultat clé :** Réduction du score de risque PingCastle de **55/100** à **16/100** (passage en Zone Verte).

## 🏗️ Architecture & Environnement technique
* **Contrôleur de Domaine :** Windows Server 2022 (Rôles AD DS & DNS).
* **Domaine :** `ais.local`.
* **Outils d'audit & sécurité :** PingCastle, Microsoft LAPS, `auditpol`.

## ⚙️ Étapes de mise en œuvre & Remédiations

### 1. Audit Initial
* **Score initial :** **55/100** (Indicateurs de risque sur les comptes à privilèges, l'absence de LAPS et la corbeille AD désactivée).

### 2. Plan de Remédiation & Durcissement (Hardening)

* **Mise en place de Microsoft LAPS (Local Administrator Password Solution) :**
  * Extension du schéma Active Directory (`Update-AdmPwdADSchema`).
  * Délégation des droits (écriture limitée aux ordinateurs, lecture restreinte aux Admins du domaine).
  * Création d'une GPO (`Securite_LAPS`) imposant des mots de passe locaux complexes de 14 caractères minimum avec rotation automatique.

* **Protection des comptes à hauts privilèges :**
  * Activation de l'attribut *AccountNotDelegated* ("Ce compte est sensible et ne peut pas être délégué") pour bloquer les attaques de relais Kerberos.
  * Activation de la **Corbeille Active Directory** (*AD Recycle Bin*) à l'échelle de la forêt et protection contre la suppression accidentelle sur les Unités d'Organisation (OU).

* **Renforcement de la politique globale & Traçabilité :**
  * Élévation de la stratégie de mots de passe du domaine (longueur minimale fixée à 14 caractères).
  * Désactivation du service d'impression *Spooler* sur le contrôleur de domaine pour supprimer ce vecteur d'attaque critique.
  * Activation des politiques d'audit avancées via `auditpol` (connexions, modifications d'objets, verrouillages).
  * Désactivation des protocoles obsolètes/vulnérables (LLMNR, NTLM).

### 3. Audit Final de Contrôle
* **Score post-remédiation :** **16/100** (Niveau de risque faible / Zone verte validée).

---
📁 *Projet de durcissement Active Directory documenté et validé.*
