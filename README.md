# zabbix-template

This is a collection of Zabbix templates used to monitor different things I have had the opportunity to work on and with
 in my own line of professional work.

## Modules

### template_module_basic_icmpsnmp

This module is drawn from several of the included templates inside Zabbix with a specific focus on the most basic ICMP and SNMP communication capabilities.  This module was created to be used as a base for other templates so any updates or improvements to the module will take effect across all other templates which use this one,

## Power

### template_net_baytech_snmpv2

Requirements: template_module_basic_icmpsnmp

This template is used to monitor PDUs from BayTech via SNMP.  This monitors and gathers statistics from circuits, outlets, redundant power switches, and remote power contollers.
