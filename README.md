# Zabbix Templates

## DNS
Monitoring Bind9 queries: [Template Bind9](https://github.com/TiagoAntonio019/zabbix_templates/blob/main/bind9_queries.yaml)

## Postgresql
How to monitor Postgresql (active connections counter): [Integration with PG](https://github.com/TiagoAntonio019/zabbix_templates/blob/main/Active%20PostgreSQL%20Connections.md)

## Mikrotik SFP port
How to monitor SFP ports: [Sinal SFP Mikrotik](https://github.com/TiagoAntonio019/zabbix_templates/blob/main/Sinal_SFP_Mikrotik.yaml)
### Attention
You shall create a global variable for this template!

1. Navigate to Administration > General > Regular Expressions
2. Press the button 'New Regular Expression'
3. Fill in the detail:
   - Name: EXCLUSAO_ETHER-SINAL SFP
   - Expressions: You'll create 2 expressions
   
     | Expression Type | Expression |
     |-----------------|------------|
     | Result is TRUE  | ^sfp       |
     | Result is FALSE | ^NULLO$    |
4. All done!
