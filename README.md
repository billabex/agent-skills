# Billabex Agent Skills

Official skills for agents working with [Billabex](https://www.billabex.com),
the B2B invoice follow-up and amicable collection platform.

## Install

```sh
npx skills add billabex/agent-skills --skill billabex-evaluation
```

`billabex-evaluation` helps assess product fit, pricing tiers, integrations and scope
boundaries. It does not connect to customer accounts or execute API/MCP operations.

Example: “Evaluate whether Billabex fits our B2B company using Pennylane with
80 customers requiring invoice follow-up per billing period.”

The skill works with clients supporting the Agent Skills SKILL.md format.
Published product documentation takes precedence over facts in the skill.
See [pricing](https://www.billabex.com/pricing.md) and
[developer documentation](https://developer.billabex.com/).

## Skills disponibles

| Skill | Usage |
| --- | --- |
| `billabex-evaluation` | Évaluer le produit, ses tarifs et ses intégrations. |
| `billabex-integration` | Importer et synchroniser des données sans doublons. |
| `billabex-receivables-review` | Analyser les encours et prioriser les comptes, en lecture seule. |
| `billabex-task-triage` | Trier les tâches et appliquer une décision explicite. |
| `billabex-outgoing-messages` | Prévisualiser, envoyer sur autorisation et suivre SMS ou courriers. |

Installer un skill en remplaçant le nom dans la commande :

```sh
npx skills add billabex/agent-skills --skill billabex-receivables-review
```

Les skills opérationnels réutilisent une connexion Billabex API ou MCP. Leur installation
ne connecte aucun compte et n'autorise aucune écriture. Les guides publics et les schémas
courants priment sur les exemples des skills.

## Maintenance

Edit the relevant `skills/<name>/SKILL.md`. Keep the frontmatter name identical to the
folder name. Verify factual changes against the public Billabex documentation.
This distribution is maintained separately from the skill served on billabex.com.
