---
name: billabex-task-triage
description: Review and handle Billabex account tasks that require human input, including missing contacts and announced-payment reviews. Use for task inbox triage and applying a user decision, not for autonomous payment confirmation or generic portfolio analytics.
---

# Traiter les tâches Billabex

## Quand utiliser ce skill

Trier les tâches ouvertes, expliquer pourquoi un compte attend une décision ou appliquer une
réponse déjà donnée par l'utilisateur. Exemple : « Quelles tâches ont besoin de moi aujourd'hui ? »

## Reconstituer la demande

Lire le [guide des tâches](https://developer.billabex.com/en/guides/account-tasks/index.md)
et le [contrat OpenAPI](https://developer.billabex.com/openapi.json), ou les schémas des outils MCP
connectés. Le [parcours OAuth](https://developer.billabex.com/auth.md) décrit l'accès si nécessaire.
Identifier l'organisation ; pour REST, la recherche transversale utilise
`/api/public/v1/account-tasks/search`, et la liste d'un compte `/api/public/v1/account-tasks`.
Lire toutes les pages utiles et le fil complet des tâches retenues, pas seulement leur titre.

Rapprocher la tâche de son compte, des factures et communications pertinentes, ainsi que des
pauses et déclarations de paiement. Distinguer l'utilisateur de Billabex, qui prend la décision,
du contact du débiteur. Restituer la question précise, les faits disponibles et ce qui manque.

## Appliquer une décision

- Une demande de tri ou de résumé reste en lecture seule. Une instruction explicite peut
  autoriser une réponse ; respecter son périmètre sans redemander une autorisation déjà donnée.
- Lire le type de tâche et le schéma courant de son interaction. Ne pas inventer de réponse
  structurée, de contact ou de preuve pour satisfaire la validation.
- Avant d'agir, relire l'état : une autre personne peut avoir répondu. Utiliser l'idempotence
  publiée pour les écritures REST et relire le résultat après l'opération.
- Fermer, annuler ou mettre une tâche de côté ne résout pas le paiement ni la pause sous-jacente.
  Vérifier séparément le registre des paiements annoncés et l'état des relances.

## Paiement annoncé

Une déclaration, même avec `proofDocumentAttached`, ne prouve pas que l'argent est reçu.
Lire le registre `/api/public/v1/accounts/{accountId}/payment-declarations` et sa décision
actuelle. Ne jamais choisir `PaymentReceived` sur la seule foi d'une promesse ou d'une facture soldée.

Le guide distingue notamment `InvoicesSettled`, `PaymentReceived` et `ResumeAuthorized`.
Appliquer seulement la résolution correspondant aux éléments et à la décision de l'utilisateur,
avec une note explicite. Une décision humaine déjà prise ne se remplace pas arbitrairement :
si l'API retourne `app-dunning.payment-declaration.already-resolved`, restituer la décision fournie.
L'exception documentée concerne `InvoicesSettled` décidée automatiquement par `accounting`.

## Restitution

Pour chaque tâche : état, question, réponse appliquée ou encore attendue, et effet vérifié sur
le compte. Si la réponse est enregistrée mais ses effets restent non vérifiés, le dire.
Une tâche fermée n'est pas à elle seule la preuve que les relances ont repris.
