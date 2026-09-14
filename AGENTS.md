# Maintaining Billabex skills

Keep each skill in `skills/<name>/SKILL.md` with YAML `name` and `description`.
Use the public Billabex website and developer portal as sources; do not publish private
customer data, credentials, internal URLs or unverified product promises.
Keep skills focused on their stated use case. Publishing a skill does not authorize
customer-account operations. Do not add executable scripts or dependencies unless needed.

## Git identity

For this repository, use `yassine-chabli-billabex` for both commit identity and GitHub
publication. Before committing, check the repository-local Git identity:

```sh
git config --local user.name yassine-chabli-billabex
git config --local user.email 177330449+yassine-chabli-billabex@users.noreply.github.com
git var GIT_AUTHOR_IDENT
git var GIT_COMMITTER_IDENT
```

Before pushing, verify that the credentials used for the push authenticate as
`yassine-chabli-billabex`, and inspect the author and committer of outgoing commits.
Do not fall back to another connected account. If this identity is unavailable, stop
and explain the missing access. Never expose credentials in output or repository files.
