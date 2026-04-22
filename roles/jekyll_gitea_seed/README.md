# jekyll_gitea_seed

Host-level role that bootstraps a Jekyll source repository into Gitea.

## Responsibility

This role owns:

- ensuring the target repo exists in host-level Gitea
- seeding the repo from an external Git template if the repo is empty
- generating a deploy keypair for the cluster Jekyll app
- registering the public key as a read-only deploy key on the repo

This role does not own:

- cluster deployment of Jekyll
- git-sync sidecar configuration
- site serving or ingress
