# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is an Ansible role that installs and configures [supervisord](http://supervisord.org/) on Debian/Ubuntu hosts. It depends on a sibling `apt` role (referenced in `meta/main.yml`) to handle package installation.

## Linting

```bash
ansible-lint
```

## Architecture

The role has two distinct usage modes, selected by which variables the caller defines:

- **Single program** (`supervisor_program` defined): deploys `conf.d/<program>.conf` from `program.conf.j2`
- **Group** (`supervisor_group` defined): deploys `conf.d/group.<group>.conf` from `group.conf.j2`, grouping programs listed in `supervisor_programs`

Both modes always deploy the main `supervisord.conf` (from `supervisord.conf.j2`) and notify the `supervisor restart` handler on change.

### Variable convention in templates

All optional supervisord settings follow a consistent pattern in the Jinja2 templates: if the corresponding variable is defined, the setting is emitted; otherwise the line is commented out. This means the only way to activate a setting is to define its variable — there are no boolean toggles, just presence/absence of the variable.

### Key defaults (`defaults/main.yml`)

| Variable | Default |
|---|---|
| `supervisor_packages` | `[supervisor]` |
| `supervisor_minfds` | `40960` |

All other `supervisor_*` variables have no default and must be set by the caller.
