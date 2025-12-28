+++
title = "Ansible"
weight = 3
+++

# Ansible Integration

The `ansible` command detects `lookup('env', 'VAR')` calls in your Ansible playbooks and verifies those variables exist in your `.env` files.

## Usage

```bash
envcheck ansible playbooks/ --env .env
envcheck ansible . --env .env.prod
```

## What it checks

### Environment lookups
```yaml
- name: Deploy application
  hosts: all
  vars:
    api_key: "{{ lookup('env', 'API_KEY') }}"  # ← Checked
    db_url: "{{ lookup('env', 'DATABASE_URL') }}"
  tasks:
    - name: Configure app
      template:
        src: app.conf.j2
        dest: /etc/app/config
```

## Example Output

```
$ envcheck ansible playbooks/ --env .env

Scanning playbooks/deploy.yml
  W009: lookup('env', 'SLACK_WEBHOOK') not found in .env

Scanning playbooks/backup.yml
  W009: lookup('env', 'S3_BUCKET') not found in .env

Found 2 issues
```

## CI Integration

```yaml
- uses: envcheck/action-envcheck@v1
  with:
    command: ansible
    args: "playbooks/"
    env_file: ".env"
```
