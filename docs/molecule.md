[Molecule](https://github.com/ansible-community/molecule) is a testing tool for ansible roles.

# [Installation](https://molecule.readthedocs.io/installation/)

```bash
pip install molecule
```

# [CI configuration](https://github.com/marketplace/actions/setup-molecule)

Since gitea supports github actions you can use the `setup-molecule` and `setup-lint` actions. For example:

```yaml
---
name: Molecule

"on":
  pull_request:

env:
  PY_COLORS: "1"
  ANSIBLE_FORCE_COLOR: "1"
jobs:
  lint:
    name: Lint
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the codebase
        uses: actions/checkout@v3

      - name: Setup Lint
        uses: bec-galaxy/setup-lint@{Version}

      - name: Run Lint tests
        run: ansible-lint

  molecule:
    name: Molecule
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - name: Checkout the codebase
        uses: actions/checkout@v3

      - name: Setup Molecule
        uses: bec-galaxy/setup-molecule@{Version}

      - name: Run Molecule tests
        run: molecule test
```

[That action](https://github.com/bec-galaxy/setup-molecule/blob/main/action.yml) installs the latest version of the packages, if you need to check a specific version of the packages you may want to create your own step or your own action.

# Upgrade

## [To v5.0.0](https://github.com/ansible-community/molecule/discussions/3825#discussioncomment-4908366)

They've removed the `lint` command, the reason behind is that there are two different testing methods which are expected to be run in very different ways. Linting should be run per entire repository. Molecule executions are per scenario and one project can have even >100 scenarios. Running lint on each of them would not only slowdown but also increase the maintenance burden on linter configuration and the way is called.

They recommend users to run `ansible-lint` using `pre-commit` with or without `tox. That gives much better control over how/when it is updated.

You can see an example on how to do this in the [CI configuration section](#ci-configuration).

## [To v4.0.0](https://github.com/ansible-community/molecule/discussions/3198)

This version is seen as a clean-up or refactoring release, not expected to require users to change their existing scenarios in order to make use of the new version.

## To v3.0.0

# References

- [Source](https://github.com/ansible-community/molecule)
- [Docs](https://molecule.rtfd.io/)
