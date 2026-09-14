---
name: billabex-outgoing-messages
description: Prepare and, when authorized, send Billabex SMS and postal letters using preview, credit limits, idempotency and delivery tracking. Use for a specific outgoing SMS or letter, not email drafting or autonomous bulk reminders.
---

# Préparer et suivre un SMS ou courrier Billabex

## Quand utiliser ce skill

Prévisualiser un SMS ou une lettre, en connaître le coût, puis l'envoyer si l'utilisateur l'a
autorisé. Exemple : « Prépare un courrier pour ce compte et montre-moi son coût avant envoi. »
Ce parcours ne concerne pas l'envoi d'emails.

## Préparer

Lire le [guide des messages sortants](https://developer.billabex.com/en/guides/outgoing-messages/index.md)
et le [contrat OpenAPI](https://developer.billabex.com/openapi.json), ou les schémas MCP connectés.
Le [parcours OAuth](https://developer.billabex.com/auth.md) décrit l'accès si nécessaire.

Vérifier l'organisation, le compte, le destinataire, les factures et avoirs concernés ainsi que
l'échéance demandée. Lire les communications et éventuelles pauses pour éviter un message
contradictoire. Copier les noms exactement. Ne pas inventer un téléphone ou une adresse manquante.

## Prévisualiser avant l'envoi

Utiliser `preview-outgoing-message` en MCP, ou
`POST /api/public/v1/accounts/{accountId}/outgoing-message-communications/preview` en REST.
La prévisualisation ne débite pas de crédits. Lire le texte, le coût `credits` et, pour une lettre,
les pièces et le nombre de pages. Le MCP ne renvoie ni PDF base64 ni HTML ; utiliser la
prévisualisation REST avec `includePdf` si une vérification du PDF est nécessaire.

Présenter le destinataire, le canal, le contenu, les pièces et le coût lorsque l'utilisateur
attend une validation. « Prépare » n'autorise pas « envoie ». Une autorisation d'envoi déjà
explicite reste valable pour son périmètre ; ne pas la transformer en autorisation générale.

## Envoyer et suivre

- Réutiliser les paramètres vérifiés avec `send-outgoing-message`, ou le POST REST sur
  `/api/public/v1/accounts/{accountId}/outgoing-message-communications`.
- Fournir une `idempotencyKey` stable pour cette intention d'envoi et `maxCredits` dans la
  limite autorisée. Si le destinataire, le contenu ou l'échéance changent, c'est une nouvelle
  intention à vérifier. Un surcoût n'autorise pas à relever automatiquement le plafond.
- Une reprise garde la même clé. Sérialiser les reprises d'un même envoi. Ne pas créer une
  seconde intention après un timeout ou un `502` : le prestataire peut avoir accepté le premier.
- Lire `get-outgoing-message`, ou le GET REST du même chemin suivi de l'identifiant de
  communication. Consulter `deliveryStatus`, `statusHistory` et `creditsCharged`.
- En cas de résultat inconnu, conserver l'identifiant et la clé, vérifier l'état de façon bornée,
  puis signaler l'incertitude. Des crédits débités ne prouvent pas la livraison.

Rendre le statut observé et son heure de lecture, le coût et l'identifiant de communication.
Distinguer « préparé », « enregistré », « accepté par le prestataire » et « livré » selon les
preuves retournées. Une demande concernant plusieurs destinataires nécessite un périmètre
et un budget propres ; ne pas étendre un envoi individuel à tout le portefeuille.
