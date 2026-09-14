---
name: billabex-receivables-review
description: Analyze a Billabex receivables portfolio using aging balances and account details. Use for overdue-invoice prioritization, currency-separated exposure and finance review reports. Read-only; not for sending reminders or recording payments.
---

# Analyser les encours Billabex

## Quand utiliser ce skill

Préparer une revue des impayés ou identifier les comptes à examiner en priorité.
Exemple : « Quels clients concentrent nos encours de plus de 90 jours ? »

## Lire les données

Utiliser la connexion Billabex disponible. Sinon, suivre le
[parcours OAuth](https://developer.billabex.com/auth.md) sans demander de coller un jeton dans le chat.
Lire le [guide de balance âgée](https://developer.billabex.com/en/guides/aging-balance/index.md)
et le [contrat](https://developer.billabex.com/openapi.json) avant de choisir les filtres.
Confirmer l'organisation et le périmètre temporel du rapport. La balance courante n'est pas
une photographie historique ; ne pas prétendre reconstituer une date passée avec elle seule.

- En MCP : `get-aging-balance`, puis `list-aging-balance-by-account`, avec `mcp:read`.
- En REST : `/api/public/v1/aging-balance`, puis `/api/public/v1/aging-balance/accounts`,
  avec `organizationId` et une permission `accounts:read`. Le détail exige `currencyCode`.
- Parcourir les pages nécessaires avec `first` et `after`. Repartir sans curseur si la devise,
  les filtres, le tri ou la taille de page changent. Indiquer tout périmètre partiel.

## Interpréter sans fausser les montants

La balance est nette des avoirs non alloués, appliqués aux montants les plus anciens du même
compte et de la même devise. Ne pas soustraire ces avoirs une seconde fois. Les comptes passés
en perte sont exclus. Séparer les devises : aucun taux de conversion n'est implicite.

Distinguer `notDue`, `d0_30`, `d31_60`, `d61_90` et `d90Plus`. Un même compte peut apparaître
dans plusieurs tranches : additionner leurs compteurs ne donne pas un nombre de clients distincts.
`overdueRate` vaut 100 pour 100 %, pas 1. Un encours ne suffit pas à calculer un DSO fiable.

Pour expliquer une priorité, lire le contexte des comptes concernés : factures, avoirs,
communications, pauses, tâches et paiements annoncés. Une promesse ou une preuve fournie par
le débiteur n'est pas une confirmation bancaire. Ne pas recommander une nouvelle relance
sans tenir compte d'un litige, d'une pause ou d'une décision attendue.

## Livrable

Présenter les montants par devise et tranche, les comptes prioritaires, le motif de priorité
et les informations manquantes. Nommer la date de lecture et les exclusions. Proposer les
prochaines actions sans envoyer de message, modifier un solde ni reprendre les relances :
ce skill porte sur l'analyse, pas sur leur exécution.
