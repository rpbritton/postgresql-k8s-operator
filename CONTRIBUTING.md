# Contributing

## Overview

This documents explains the processes and practices recommended for contributing enhancements to
this operator.

- Generally, before developing enhancements to this charm, you should consider [opening an issue
  ](https://github.com/canonical/postgresql-k8s-operator/issues) explaining your use case.
- If you would like to chat with us about your use-cases or proposed implementation, you can reach
  us at [our public Mattermost channel](https://matrix.to/#/#charmhub-data-platform:ubuntu.com)
  or [Discourse](https://discourse.charmhub.io/).
- Familiarising yourself with the [Charmed Operator Framework](https://ops.readthedocs.io/en/latest/index.html) library
  will help you a lot when working on new features or bug fixes.
- All enhancements require review before being merged. Code review typically examines
  - code quality
  - test coverage
  - user experience for Juju administrators of this charm.
- Please help us out in ensuring easy to review branches by rebasing your pull request branch onto
  the `main` branch. This also avoids merge commits and creates a linear Git commit history.

## Developing

You can create an environment for development with `tox`:

```shell
tox devenv -e integration
source venv/bin/activate
```

### Testing

```shell
tox run -e format          # update your code according to linting rules
tox run -e lint            # code style
tox run -e unit            # unit tests
charmcraft test lxd-vm:    # integration tests
tox                        # runs 'lint' and 'unit' environments
```

## Build charm

Build the charm in this git repository using:

```shell
charmcraftcache pack
```

### Deploy

```bash
# Create a model
juju add-model dev

# Enable DEBUG logging
juju model-config logging-config="<root>=INFO;unit=DEBUG"

# enable Role-Based Access Control on microk8s
microk8s enable rbac

# Deploy the charm
juju deploy ./postgresql-k8s_ubuntu-22.04-amd64.charm --trust \
    --resource postgresql-image=$(yq '(.resources.postgresql-image.upstream-source)' metadata.yaml)
```

## Canonical Contributor Agreement

Canonical welcomes contributions to the MySQL Operator. Please check out our
[contributor agreement](https://ubuntu.com/legal/contributors)if you're
interested in contributing to the solution.
