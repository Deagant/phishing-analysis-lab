# Phishing Analysis Lab

Analyse d'emails suspects réalisée dans un cadre de formation.  
Objectif : identifier les indices de phishing, collecter les artefacts pertinents et produire un verdict clair et exploitable.

---

## Contexte

| Élément | Détail |
|---|---|
| Type | Exercice de formation |
| Environnement | Emails de test fournis dans un cadre pédagogique autorisé |
| Périmètre | Autorisé et contrôlé |
| Cas analysés | Mail 33 · Mail 21 |
| Données sensibles | Supprimées ou anonymisées |

---

## Objectifs

- Identifier les indices de phishing dans les en-têtes et le corps de l'email
- Analyser les champs Received, From, Reply-To, DKIM, SPF, DMARC
- Vérifier l'IP d'origine et la réputation du domaine expéditeur
- Documenter les artefacts collectés
- Produire un verdict structuré et des recommandations

---

## Structure du dépôt

| Fichier / Dossier | Contenu |
|---|---|
| `report-phishing-analysis.md` | Rapport structuré des deux analyses |
| `artifacts/` | En-têtes sanitisés (à déposer manuellement avant publication) |
| `screenshots/` | Captures anonymisées (à déposer manuellement avant publication) |
| `CHECKLIST.md` | Checklist avant publication |

---

## Compétences démontrées

- Analyse d'en-têtes email (Received, From, Reply-To, DKIM, SPF, DMARC)
- Identification d'incohérences expéditeur / domaine réel
- Analyse d'IP d'origine et réputation de domaine
- Détection de liens masqués et pièces jointes suspectes
- Rédaction d'un rapport court orienté Blue Team
- Recommandations de sécurité utilisateur

---

## Avertissement

Exercice réalisé dans un cadre légal, éducatif et autorisé.  
Aucune donnée réelle ou sensible n'est publiée dans ce dépôt.
