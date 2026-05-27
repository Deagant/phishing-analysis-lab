# Phishing Analysis Lab

Deux exercices de formation sur le phishing : analyse d'emails suspects et simulation de campagne avec GoPhish.

---

## Contexte

| Élément | Détail |
|---|---|
| Type | Exercices de formation — Jedha Fullstack Cybersécurité |
| Date | 19 mai 2026 |
| Approche | Analyse Blue Team + simulation pour comprendre les mécanismes |
| Données | Emails de lab fournis — aucune cible réelle |
| Périmètre | Autorisé et contrôlé |

---

## Exercice 1 — Analyse d'emails suspects

Deux emails suspects analysés au format SOC junior / PhishTool.

| Email | Verdict | Technique observée |
|---|---|---|
| `mail 33.eml` | ⚠️ Phishing probable | Usurpation de marque Stripe + PDF externe sur Amazon S3 |
| `mail 21.eml` | ⚠️ Phishing probable | Display name spoofing — nom affiché ≠ adresse réelle |

**Artefacts collectés :** IP d'origine, URL neutralisée, domaine expéditeur, adresse email réelle.  
**Outils mentionnés :** urlscan.io, VirusTotal, AbuseIPDB.

---

## Exercice 2 — Simulation de campagne (GoPhish)

Simulation d'une campagne phishing sur 4 cibles fictives (@yopmail.com) pour comprendre les mécanismes côté attaquant.

| Indicateur | Résultat |
|---|---|
| Cibles | 4 utilisateurs fictifs |
| Taux de clic | 1/4 — 25% |
| Délai avant premier clic | 54 secondes |
| Navigateur de la cible | Firefox 128 / Linux |

Objectif : comprendre le tracking GoPhish (clic, heure, OS, navigateur) pour mieux détecter les beacons dans les logs proxy lors d'une vraie attaque.

---

## Structure du dépôt

| Fichier | Contenu |
|---|---|
| `report-phishing-analysis.md` | Analyse complète Mail 33 et Mail 21 — artefacts, IOCs, recommandations |
| `gophish-campaign.md` | Simulation de campagne GoPhish — résultats et analyse défensive |
| `CHECKLIST.md` | Checklist avant publication |

---

## Compétences démontrées

- Analyse d'en-têtes email et collecte d'artefacts (IP, URL, adresse réelle)
- Identification du display name spoofing
- Reconnaissance de l'usurpation de marque + hébergement cloud comme vecteur de contournement de filtres
- Neutralisation d'URLs dans les rapports (notation `hxxps` + `[.]`)
- Utilisation de GoPhish pour une simulation de sensibilisation
- Analyse des résultats de campagne (taux de clic, délais, métadonnées)
- Documentation orientée Blue Team et recommandations actionnables

---

## Avertissement

Exercices réalisés dans un cadre légal, éducatif et autorisé.  
Les adresses email et noms dans la simulation GoPhish sont entièrement fictifs.  
Les IOCs des emails suspects sont publiés car ils constituent des indicateurs de menace documentés.
