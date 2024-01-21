<!--
Copyright (c) Ansible Project
GNU General Public License v3.0+ (see LICENSES/GPL-3.0-or-later.txt or https://www.gnu.org/licenses/gpl-3.0.txt)
SPDX-License-Identifier: GPL-3.0-or-later
-->

# GitHub Action for testing importing an Ansible collection with the Galaxy importer

[![Linting](https://github.com/ansible-community/github-action-test-galaxy-import/actions/workflows/linting.yml/badge.svg)](https://github.com/ansible-community/github-action-test-galaxy-import/actions/workflows/linting.yml)
[![Tests](https://github.com/ansible-community/github-action-test-galaxy-import/actions/workflows/tests.yml/badge.svg)](https://github.com/ansible-community/github-action-test-galaxy-import/actions/workflows/tests.yml)
[![REUSE](https://github.com/ansible-community/github-action-test-galaxy-import/actions/workflows/reuse.yml/badge.svg)](https://github.com/ansible-community/github-action-test-galaxy-import/actions/workflows/reuse.yml)

A composite GitHub Action that allows to test importing a built Ansible collection with the [Galaxy importer](https://github.com/ansible/galaxy-importer) in GitHub Actions CI/CD workflows.

While this action can be used directly in workflows, we suggest to use the [bundled shared workflow](#bundled-shared-workflow).

This action is covered by the [Ansible Code of Conduct](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html).

## Usage

To use the action, add the following step to your workflow file (for example `.github/workflows/galaxy-import.yml`):

```yaml
- name: Test collection import
  uses: ansible-community/github-action-test-galaxy-import@main
  with:
    artifact-path: /path/to/foo-bar-1.0.0.tar.gz
```

## Options

The follow options can be provided to this GitHub Action.

### `artifact-path`

Path to the collection's build artifact (tarball).

**(REQUIRED)**

### `python-version`

The Python version to use.

Note that 3.12 currently does not work due to https://github.com/madpah/requirements-parser/issues/88.

**(DEFAULT: `3.11`)**

### `ansible-core-version`

The branch of tag name of ansible-core to install.
This is assumed to exist in https://github.com/ansible/ansible.

**(DEFAULT: `stable-2.16`)**

### `collection-requirements-path`

Path to a Galaxy requirements file. If present, these collections will be installed.

### `importer-config-path`

Path to a Galaxy importer configuration file.
See https://github.com/ansible/galaxy-importer#configuration for more information.

## Bundled shared workflow

This GitHub Action bundles a shared workflow, [test-galaxy-import.yml](https://github.com/ansible-community/github-action-test-galaxy-import/blob/main/.github/workflows/test-galaxy-import.yml), which allows you to build a collection and test importing it with the Galaxy importer.

```yaml
name: import-galaxy
'on':
  # Run CI against all pushes (direct commits, also merged PRs) to main and the stable-* branches, and all Pull Requests
  push:
    branches:
      - main
      - stable-*
  pull_request:

jobs:
  import-galaxy:
    permissions:
      contents: read
    name: Import collection with Galaxy importer
    uses: ansible-community/github-action-test-galaxy-import/.github/workflows/test-galaxy-import.yml@main
```

If your collection's root (where `galaxy.yml` is) is in a subdirectory of the repository, use the `subdirectory` option:

```yaml
jobs:
  import-galaxy:
    permissions:
      contents: read
    name: Import collection with Galaxy importer
    uses: ansible-community/github-action-test-galaxy-import/.github/workflows/test-galaxy-import.yml@main
    with:
      subdirectory: ansible_collections/foo/bar
```

Please check out the workflow for other supported options.

## License

This action is primarily licensed and distributed as a whole under the GNU General Public License v3.0 or later.

See [LICENSES/GPL-3.0-or-later.txt](https://github.com/ansible-community/github-action-test-galaxy-import/blob/main/COPYING) for the full text.

All files have a machine readable `SDPX-License-Identifier:` comment denoting its respective license(s) or an equivalent entry in an accompanying `.license` file. This conforms to the [REUSE specification](https://reuse.software/spec/).
