# ansible-mattermost-role

Role designed to be used with postgresql, but if you want to use something else - u need just change connection field in config.json.j2 template
example of playbook 
-------------------

```yaml
---
- hosts: mattermost
  remote_user: ubuntu
  become: true
  become_user: root
  become_method: sudo
  roles:
    - role: nginx
      tags:
        - nginx
    - role: ANXS.postgresql
      tags:
        - postgres
    - role: mattermost
      tags:
        - mattermost
        - awesome_chat

```
Example of host variables
-------------------------
```yaml
postgresql_ext_install_contrib: yes

postgresql_version: 9.3
postgresql_encoding: 'UTF-8'
postgresql_locale: 'en_US.UTF-8'

postgresql_admin_user: "postgres"
postgresql_default_auth_method: "trust"

postgresql_cluster_name: "main"
postgresql_cluster_reset: false

# List of databases to be created (optional)
# Note: for more flexibility with extensions use the postgresql_database_extensions setting.
postgresql_databases:
  - name: mattermost
    owner: mmuser          # optional; specify the owner of the database
    hstore: yes         # flag to install the hstore extension on this database (yes/no)
    uuid_ossp: yes      # flag to install the uuid-ossp extension on this database (yes/no)
    citext: yes         # flag to install the citext extension on this database (yes/no)

# List of users to be created (optional)
postgresql_users:
  - name: mmuser
    pass: mmuser_password
    encrypted: no       # denotes if the password is already encrypted.

# List of user privileges to be applied (optional)
postgresql_user_privileges:
  - name: mmuser                   # user name
    db: mattermost                # database
    priv: "ALL"                 # privilege string format: example: INSERT,UPDATE/table:SELECT/anothertable:ALL
    role_attr_flags: "CREATEDB" # role attribute flags

mattermost_nginx_domain_name: mattermost.example.com
mattermost_use_nginx: yes
mattermost_nginx_ssl: yes
mattermost_nginx_ssl_path: /etc/nginx/ssl
mattermost_nginx_ssl_crt_name: mattermost.crt
mattermost_nginx_ssl_key_name: mattermost.key

```

Requiriments
------------
- ANXS.postgresql
- some nginx role (optional)
