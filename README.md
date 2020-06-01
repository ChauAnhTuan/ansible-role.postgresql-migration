# Ansible Role: PostgreSQL migration

Migrate the PostgreSQL databases to another server for RHEL/CentOS/Debian/Ubuntu.

## Requirements

This role required psycopg2 library.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    databases_dump:
      - name: The name of the database that you want to dump
        login_host: The address of the server that you want to dump
        login_password: The password of the database server
        login_user: The username of the database server
        state: Set value "dump" if you want to dump (The database state)
    postgresql_databases:
      - name: The name of the database that you want to dump
        login_host: The address of the server that you want to dump
        login_password: The password of the database server
        login_user: The username of the database server
	owner: opus
        state: Set value "present" that the database should be created if necessary.
    databases_restore:
      - name: The name of the database that you want to dump
        login_host: The address of the server that you want to dump
        login_password: The password of the database server
        login_user: The username of the database server
        state: Set value "restore" if you want to restore to another server (The database state)

## Example Ansible Playbook
        ---
        - name: Converge
          hosts: localhost
          become: true
          vars:
            databases_dump:
              - name: opusworkflowdb
                login_host: xxxxxxxxxx.cltyxxxyrqbx.us-east-1.rds.amazonaws.com
                login_password: xxxxxxxxxx
                login_user: opus
                state: dump
              - name: qa-opusworkflowdb
                login_host: xxxxxxxxxx.cltyxxxyrqbx.us-east-1.rds.amazonaws.com
                login_password: xxxxxxxxxx
                login_user: opus
                state: dump
            postgresql_databases:
              - name: opusworkflowdb
                login_host: xxxxxxxxxx.cltyxxxyrqbx.us-east-1.rds.amazonaws.com
                login_password: xxxxxxxxxx
                login_user: opus
                owner: opus
                state: present
              - name: qa-opusworkflowdb
                login_host: xxxxxxxxxx.cltyxxxyrqbx.us-east-1.rds.amazonaws.com
                login_password: xxxxxxxxxx
                login_user: opus
                owner: opus
                state: present
            databases_restore:
              - name: opusworkflowdb
                login_host: xxxxxxxxxx.cltyxxxyrqbx.us-east-1.rds.amazonaws.com
                login_password: xxxxxxxxxx
                login_user: opus
                state: restore
              - name: qa-opusworkflowdb
                login_host: xxxxxxxxxx.cltyxxxyrqbx.us-east-1.rds.amazonaws.com
                login_password: xxxxxxxxxx
                login_user: opus
                state: restore
          roles:
            - role: ../ansible-role.postgresql-migratio

## Run Ansible Playbook

        ansible-playbook converge.yml