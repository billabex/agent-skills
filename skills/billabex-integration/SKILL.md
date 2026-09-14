---
name: billabex-integration
description: Build or troubleshoot a Billabex REST or MCP integration for importing customer accounts, invoices and credit notes, with stable identifiers and safe retries. Use for ERP or CRM synchronization and import workflows, not product evaluation.
---

# Intégrer des données dans Billabex

## Quand utiliser ce skill

Importer des factures depuis un ERP, synchroniser un CRM ou reprendre un import interrompu.
Exemple : « Importe ces factures dans Billabex sans recréer celles déjà envoyées. »

## Préparer l'intégration

Lire le [contrat OpenAPI](https://developer.billabex.com/openapi.json) et le
[parcours OAuth](https://developer.billabex.com/auth.md). Pour MCP, découvrir les outils et
leurs schémas depuis le serveur configuré ; ne pas traduire un chemin REST en nom d'outil supposé.
Réutiliser une connexion disponible, avec les scopes nécessaires au travail demandé.
Identifier l'organisation cible avant de lire ou modifier ses données.

Lire les guides [sources](https://developer.billabex.com/en/guides/sources/index.md),
[upsert](https://developer.billabex.com/en/guides/upsert/index.md) et
[idempotence](https://developer.billabex.com/en/guides/idempotency/index.md) pour les opérations retenues.
En cas de contradiction entre un exemple de guide et le schéma d'une opération, vérifier ce dernier
avant de construire la requête. Un POST de création n'est pas implicitement un upsert.

## Rapprocher puis écrire

- Présenter le mapping des comptes, identifiants source, montants, devises et échéances sur un
  échantillon. Signaler les données manquantes au lieu de les inventer. Une demande d'analyse
  ou de génération de code n'autorise pas l'import dans un compte réel.
- Le numéro d'une facture est unique par compte, pas par organisation. Utiliser `sourceId`
  pour le rapprochement externe : il est immuable, sensible à la casse et unique par organisation.
  Un doublon refusé lors d'une création demande de retrouver le document, pas de changer sa référence.
- Plusieurs canaux alimentent la même organisation. Ne jamais supprimer les documents d'autres
  sources parce qu'ils sont absents du fichier importé. Ne pas déduire le canal d'un document de
  la source de son compte, ni attribuer un `connectionId` réservé au système.
- Lire `readOnlyReason` avant une modification. Si un connecteur gère encore la ressource,
  corriger dans la source propriétaire ; ne pas délier le compte pour contourner le refus.
- Pour chaque écriture REST, conserver une `Idempotency-Key` et le corps exact de la requête.
  Réutiliser les deux lors d'une reprise, dans la fenêtre publiée. Une nouvelle clé ne résout
  pas un résultat inconnu. Pour MCP, vérifier les annotations et les garanties propres à l'outil.
- Paginer les recherches et respecter `Retry-After`. Après un timeout ou une projection encore
  ancienne, relire de façon bornée ; ne pas recréer immédiatement la ressource.

## Vérifier le résultat

Rendre le nombre de lignes créées, rapprochées, refusées et restant à vérifier, avec leurs
identifiants source et les codes d'erreur utiles. Vérifier un échantillon dans Billabex sans exposer
les jetons ni les données clients dans un dépôt public. Ne pas annoncer un import réussi à partir
d'un simple code généré ou d'un total de requêtes envoyées.
