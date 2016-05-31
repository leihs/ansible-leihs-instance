Creates a leihs instance.

You need to install erasme.rbenv from Ansible Galaxy before this works. Try this:

    ansible-galaxy install erasme.rbenv

Really only makes sense as part of a larger playbook that takes care of all the other dependencies, such as [that playbook](https://github.com/psy-q/leihs-setup-ansible).

## Example

```yaml
---
- { role: leihs-instance,
    deploy_to: "/home/leihs/test/current",
    version: "3.30.1",
    ruby_version: "2.1.5",
    user: leihs,
    authorized_for_deployment:
    ["files/ssh_keys/rca.pub",
     "files/ssh_keys/fs.pub",
     "files/ssh_keys/nimaai.pub"],
    mysql_user: leihs,
    mysql_password: "hNM8f7ej2sjjnlk02hd",
    mysql_database: "leihs_prod",
    secret_token: abc123,
    logrotate_filename: "leihs.local",
    test_instance: True
  }
```


## Variables

### General

 * **deploy_to**: The directory leihs will be deployed to. If the directory doesn't exist, it is created. Inside this directory, a subdirectory called *public* is created as usual for Rails applications. That *public* directory is the only one that should be exposed via web server. Required.
 * **version**: The version of leihs to deploy. Any valid git tag or branch can be used. Also see the [releases](https://github.com/zhdk/leihs/releases) list. Required.
 * **ruby_version**: The version of Ruby to use to run this version of leihs. Default: `2.1.5`. Optional but recommended.
 * **user**: The system user on the server under which leihs should run. Default: `leihs`. Optional but recommended.
 * **authorized_for_deployment**: Array of paths to SSH key files of the people who are supposed to be authorized for deploying leihs to this server. If you're not sure what to add here, just add your own key, presuming that it already has root access to the target server. The path is relative to the main playbook that you use the leihs-instance role in. Required.
 * **logrotate_filename**: The filename under /etc/logrotate.d where the logrotate configuration for this instance should be stored. The file `log/production.log` will be rotated this way. You need to give the filename to prevent filename clashes with existing files. Required.
 * **scout_config**: Path to a Scout monitoring configuration file on the target host. Optional.
 * **demo_instance**: Whether to seed this instance with demo data and install a cronjob that empties the database every night at 4:15 and reseeds it. Useful for demonstration purposes. Optional. Default: `false`.
 * **test_instance**: Whether this is a test instance. On test instances, the currently deployed version is displayed publicly. Optional. Default: `false`.


### Database connection

 * **local_database**: If true, all the necessary MySQL server packages will be installed on the same server you are applying this role to, a database, users and permissions will be created. If false, a `mysql_host` option is instead expected and no user creation will be attempted. Default: `true`.
 * **database_flavor**: One of `mysql` (default), `mariadb5` or `mariadb10`. Onlu used if `local_database` is true.
 * **mysql_user**: Username as which this leihs instance connects to the MySQL database. Required.
 * **mysql_password**: MySQL password for the user `mysql_user`. Required.
 * **mysql_database**: Database name for the MySQL database. Required.
 * **mysql_host**: Hostname of the MySQL server. Only used if `local_database` is false. Required if `local_database` is false.

 ### LDAP

* **ldap_hostname**: Hostname of the LDAP server.
* **ldap_port**: Port to contact the LDAP server at.
* **ldap_master_bind_dn**: Master user to bind as.
* **ldap_master_bind_password**: Master user password.
* **ldap_base_dn**: Base DN that all users have to be part of.
* **ldap_unique_id_field**: Name of the field to use as UID.
* **ldap_search_field**: Which field to search.
* **ldap_admin_dn**: Users with this DN automatically get admin on leihs. Optional.
