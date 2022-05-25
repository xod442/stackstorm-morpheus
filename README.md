# Morpheus Integration Pack
This pack allows you to integrate with Morpheus

## Configuration
Copy the example configuration in [morpheus.yaml.example](./morpheus.yaml.example) to
`/opt/stackstorm/configs/morpheus.yaml` and edit as required.

It must contain:

```
ipaddress - Your Morpheus Appliance IP address
username - Morpheus User name
password - Morpheus Password
dbuser  - Mongo database username
dbpass - Mongo database password
```

You can also use dynamic values from the datastore. See the
[docs](https://docs.stackstorm.com/reference/pack_configs.html) for more info.

Example configuration:

```yaml
---
  ipaddress: "10.10.10.10"
  username: "Administrator"
  password: "password"
  dbuser: "appUser"
  dbpass: "passwordForAppUser"
```
You can also run `st2 pack config morpheus` and answer the prompts

**Note** : When modifying the configuration in `/opt/stackstorm/configs/` please
           remember to tell StackStorm to load these new values by running
           `st2ctl reload --register-configs`


## Actions

###Action naming conventions:

Individual actions: Action names will have an underscore after the HTTP method
* ``get_volumes``
* ``get_logs``
* ``get_networks``
* ``get_alerts``

Orquestra Workflows: will have a dash.

## Mongo considerations

If your mongo instance does not have auth enabled, then you don't need to
provide a dbuser and dbpass

If the default mongoDB is used and it has authentication enabled, you will
need to create a new user and a database.

This can be done by logging in as admin using the following example.

mongo -u admin -p UkIbDILcNbMhkh3KtN6xfr9h admin  (passwd in /etc/st2/st2.config)

### Then create a new user
db.createUser({user: "appUser",pwd: "passwordForAppUser",roles: [ { role: "readWrite", db: "app_db" } ]})

### Then if necessary you can check the mongo database records by
mongo -u appUser -p passwordForAppUser admin

# Servicenow Tables

![Logs - Event information](/img/morpheus-logs-table.png)
Event fields in service now:
u_category, u_id, u_severity, u_time, u_summary
