## Step 1: Create a Custom User Parameter
1.1 - Edit the Zabbix Agent Configuration File: Open the Zabbix agent configuration file, typically located at /etc/zabbix/zabbix_agentd.conf.

```
sudo vi /etc/zabbix/zabbix_agentd.conf
```

1.2 - Add a User Parameter: At the end of the configuration file, add the following line:

```
UserParameter=pg.active.connections, sudo lsof -i :5432 | grep ESTABLISHED | wc -l
```

1.3 - Save and Exit: Save the changes and exit the editor.

## Step 2: Allow Passwordless Sudo
If you want to avoid entering a password when running the command, you may need to allow the Zabbix user to run lsof without a password. Edit the sudoers file:

```
sudo EDITOR=vim visudo
```
2.1 - Add the following line to grant permission:

```
zabbix ALL=(ALL) NOPASSWD: /usr/bin/lsof
```
Make sure to replace /usr/bin/lsof with the path to the lsof command if it's located elsewhere.

## Step 3: Restart Zabbix Agent
After making the changes, restart the Zabbix agent to apply the new configuration:

```
sudo systemctl restart zabbix-agent
```

## Step 4: Create an Item in Zabbix
4.1 - Go to the Zabbix Web Interface.

- Navigate to Configuration > Hosts.

- Select the Host where you want to add the monitoring.

- Click on Items and then Create Item.

- Fill in the details:

  - Name: Active PostgreSQL Connections
  - Type: Zabbix agent
  - Key: pg.active.connections
  - Type of Information: Numeric (unsigned)
  - Update Interval: Set the desired interval (e.g., 60 seconds).

4.2 - Save the Item.

## Step 5: Create a Trigger
5.1 - Go to the Zabbix Web Interface

- Navigate to Configuration > Hosts

- Select the 'Triggers' cell of desired host

- Add a new Trigger

- Fill in the details:

  - Name: PostgreSQL Connections Threshold
  - Severity: High
  - Expression: ``` last(/mk_solutions_master/pg.active.connections)>{$PG_MAX} ```

- Create a Macro on host scope, fill in details:
  - Macro: {$PG_MAX}
  - Value: integer (you must type the ideal value for you)

## Step 6: Create a Graph
6.1 - Navigate to Configuration > Hosts

- Select the 'Gaphs' cell of desired host

- Add a new Graph

- Fill in the details:

  - Name: Active PostgreSQL Connections
  - Graph type: Normal
  - Show triggers: yes (true)
  - Items: Active PostgreSQL Connections

6.2 - Save the Graph
