# Rapport d'analyse phishing

> Exercice de formation — cadre pédagogique autorisé  
> Format SOC junior / PhishTool — 19/05/2026

---

## Synthèse

| Email | Verdict | Indicateur principal |
|---|---|---|
| `mail 33.eml` | ⚠️ Phishing probable | Lien vers un PDF externe hébergé sur Amazon S3, thème Stripe |
| `mail 21.eml` | ⚠️ Phishing probable | Incohérence display name / adresse réelle (display name spoofing) |

---

## Cas 1 — Mail 33

### Verdict : ⚠️ Phishing probable

### Résumé

L'email utilise le thème Stripe pour inciter à ouvrir un document présenté comme un rapport sécurisé.
Le fichier n'est pas en pièce jointe mais accessible via une URL externe hébergée sur Amazon S3 —
méthode parfois utilisée pour contourner les filtres de messagerie qui analysent les pièces jointes
mais ne suivent pas systématiquement les liens.

Les termes "report" et "secure" dans le nom du fichier visent à rassurer et inciter au clic.
L'hébergement cloud d'un document supposément officiel Stripe est incohérent.

### Artefacts collectés

| Artefact | Valeur |
|---|---|
| Fichier email | `mail 33.eml` |
| IP d'origine | `116.98.175.159` |
| URL neutralisée | `hxxps://cyber-fullstack-assets[.]s3[.]eu-west-3[.]amazonaws[.]com/M02-Email_Security/stripe_report_march2025_secure[.]pdf` |
| Fichier distant | `stripe_report_march2025_secure.pdf` |
| Thème usurpé | Stripe |
| Pièce jointe | Non — lien externe |
| SPF / DKIM / DMARC | Non analysés dans ce lab |

### Indices suspects observés

- Marque connue (Stripe) utilisée comme prétexte — vecteur de confiance classique
- Document PDF hébergé sur Amazon S3 et non sur les serveurs officiels Stripe
- Nom de fichier volontairement rassurant : "report", "secure", date récente
- Hébergement cloud public : méthode qui contourne parfois les filtres de messagerie qui ne suivent pas les liens
- L'URL contient un chemin de type formation (`M02-Email_Security`) — dans ce lab, le PDF est un sample pédagogique

### Analyse technique

**IP d'origine**  
L'IP `116.98.175.159` est l'adresse identifiée comme point d'envoi.
Elle devrait être vérifiée sur des outils de réputation (VirusTotal, AbuseIPDB, urlscan.io)
pour identifier un éventuel serveur compromis ou une réputation négative.

**Lien**  
L'URL est neutralisée dans ce rapport (notation `hxxps` + `[.]`) pour éviter tout clic accidentel.
Elle pointe vers un bucket S3 Amazon (`eu-west-3`) — hébergement cloud public.
Un document officiel Stripe ne serait pas hébergé sur un bucket S3 générique.

**Pièce jointe**  
Aucune pièce jointe directe — la charge utile potentielle est le PDF distant.
Ce PDF ne doit pas être ouvert directement.

### Risques identifiés

- Vol d'identifiants Stripe si le PDF contient un formulaire ou une redirection
- Téléchargement d'un malware si le PDF exploite une vulnérabilité lecteur PDF
- Page de phishing Stripe via redirection dans le document

### Recommandations

- Ne pas ouvrir le PDF directement
- Analyser l'URL avec urlscan.io et VirusTotal avant toute interaction
- Vérifier la réputation de l'IP `116.98.175.159` sur AbuseIPDB
- Rechercher les mêmes artefacts dans d'autres boîtes de réception (même campagne)
- Conserver le fichier `.eml` pour analyse complète des headers
- Informer l'utilisateur ciblé et lui rappeler de vérifier directement sur stripe.com

---

## Cas 2 — Mail 21

### Verdict : ⚠️ Phishing probable

### Résumé

Le signal principal est l'écart entre le nom affiché dans le champ "De" et l'adresse email réelle.
C'est la technique du **display name spoofing** : l'expéditeur configure son nom d'affichage
pour qu'il ressemble à une personne de confiance, mais l'adresse réelle n'a aucun lien avec elle.

Cette technique est simple, ne nécessite aucun accès au domaine usurpé,
et exploite le fait que les clients de messagerie affichent en priorité le nom plutôt que l'adresse.

### Artefacts collectés

| Artefact | Valeur |
|---|---|
| Fichier email | `mail 21.eml` |
| Display name (nom affiché) | `Erica Allshouse` |
| Adresse email réelle | `irfanullahalaguru@gmail.com` |
| Domaine expéditeur | `gmail.com` |
| Type d'anomalie | Incohérence display name / adresse réelle |
| Pièce jointe | Non renseigné dans le rapport source |
| SPF / DKIM / DMARC | Non analysés dans ce lab |

### Indices suspects observés

- Le nom affiché "Erica Allshouse" ne correspond pas à l'adresse `irfanullahalaguru@gmail.com`
- L'adresse Gmail personnelle est incohérente pour tout échange officiel ou professionnel
- Le display name peut correspondre à quelqu'un connu de la victime — technique d'ingénierie sociale

### Analyse technique

**Display name spoofing**  
La technique consiste à renseigner un nom affiché crédible dans le champ `From`.
Le format est : `From: Erica Allshouse <irfanullahalaguru@gmail.com>`

Certains clients email n'affichent que "Erica Allshouse" — la victime croit recevoir un message
d'une collègue ou d'un contact connu. En répondant, elle envoie sa réponse à l'adresse Gmail réelle.

**Adresse expéditrice**  
Un compte Gmail personnel (`gmail.com`) n'est pas une adresse professionnelle ou institutionnelle.
Son utilisation pour se faire passer pour une personne nommée est un signal clair.

**Headers**  
Une analyse complète des headers (`Received`, `Reply-To`, `Return-Path`) permettrait de confirmer
le chemin parcouru par l'email et de détecter d'autres incohérences. Non disponibles dans ce lab.

### Risques identifiés

- Usurpation d'identité pour tromper la victime sur l'émetteur réel
- Détournement de réponse (la victime répond à un attaquant croyant répondre à un collègue)
- Collecte d'informations sensibles par ingénierie sociale

### Recommandations

- Vérifier systématiquement l'adresse email complète, pas seulement le nom affiché
- Ne jamais répondre à une demande sensible (RIB, mot de passe, données) sans vérification téléphonique
- Configurer le client email pour afficher l'adresse réelle à côté du nom affiché
- Sensibiliser les utilisateurs à la technique du display name spoofing
- Conserver le fichier `.eml` pour analyse complète des headers

---

## Indicateurs à retenir (IOCs)

| Type | Valeur |
|---|---|
| IP | `116.98.175.159` |
| URL neutralisée | `hxxps://cyber-fullstack-assets[.]s3[.]eu-west-3[.]amazonaws[.]com/M02-Email_Security/stripe_report_march2025_secure[.]pdf` |
| Fichier | `stripe_report_march2025_secure.pdf` |
| Adresse email suspecte | `irfanullahalaguru@gmail.com` |
| Display name usurpé | `Erica Allshouse` |

---

## Conclusion générale

Les deux emails présentent des indicateurs clairs de phishing probable.
`mail 33` repose sur l'usurpation de marque (Stripe) et un lien externe hébergé en cloud.
`mail 21` repose sur le display name spoofing, technique simple mais efficace sur des utilisateurs non sensibilisés.

J'aurais mis les deux en quarantaine — les IOCs documentés justifient au minimum un blocage préventif
et une recherche dans les autres boîtes de réception.
