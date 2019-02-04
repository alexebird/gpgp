GPGP
====


```bash
# GPGP_PATH
# - The path where gpgp should look.
# - Inside GPGP_PATH, the expected structure is:
# - "${GPGP_PATH}"
#    ├── public/    # public gpg keys
#    └── roles/     # gpgp roles
export GPGP_PATH="${DAVINCI_HOME}/infra"
```

```bash
# GPGP_EMAIL_DOMAINS
# - One or more email domains which are to be managed by gpgp. This could probably be made unnecessary with a small amount of effort.
# - '|' separated.
export GPGP_EMAIL_DOMAINS='cool-co.com|foobar.com'
```

```bash
# GPGP_PUB_KEY_ID_BLACKLIST
# - patterns separated by a pipe which should not be deleted by gpgp import
export GPGP_PUB_KEY_ID_BLACKLIST='DE4DBEEF'
```

### TODO docs

1. auto-roles
1. \_manual role
  - This means that gpgp will ignore the directory and the user should manually manage the encryption of the files therein.

"gpg Plus"

### Setup

```
# options go in in .bashrc

# whitelist of email domains of public gpg keys.
# for multiple, separate with a '|'.
export GPGP_EMAIL_DOMAINS='foobar.com'
```

For the person provisioning new team member's gpg keys:

```
# import the new key on your system.
# edit the key, and trust the key ultimately.
gpg --edit-key <key_id>
> trust
> 5
> quit

# then export the ownertrust file to the repo, and commit.
gpg --export-ownertrust > ${GPGP_PATH}/gpg/ownertrust.txt

# on subsequent runs of `gpgp import`, the ownertrust file will be imported
# and the new key will be trusted.
gpg --import-ownertrust < ${GPGP_PATH}/gpg/ownertrust.txt
```

### Roles

### Secrets

`gpgp` gives you source-of-truth secret management.

```
secrets            <--- this dir should be a git repo.
├── dev
│   ├── FOO.gpg
│   └── gpgp-role  <--- each gpgp-role file should contain exactly one role.
├── prod
│   ├── BAR.gpg
│   ├── data
│   │   └── QUUX.gpg
│   ├── FOO.gpg
│   └── gpgp-role
├── misc
│   └── FOO        <--- misc doesn't have a gpgp-role file in any parent directory,
└── staging             so the gpgp will abort with an error.
    ├── FOO.gpg
    └── gpgp-role
```
