# Basic SNMP and ICMP

## Description

Baseline SNMP and ICMP monitoring of virtually any network system.

## Overview

This was built as a module that other templates can include.

None of this collection is my own, but rather it is a springboard for other template types that I am building out.

While the convenience of choosing a single template is a good move on the part of the Zabbix team, the approach does not take advantage of the modular capabilities and dependencies that Zabbix has built in.

## Dependent Modules

The following are known modules with rely on this one:

| Template                      | Description                                |
|-------------------------------|--------------------------------------------|
| template_power_baytech_snmpv2 | Baytech RPDU monitoring support via SNMPv2 |

## Macros Used

| Macro             | Default Value | Description                                                            |
|-------------------|---------------|------------------------------------------------------------------------|
| {$ICMP_LOSS_WARN} | 20            | Percentage threshold where an ICMP loss warning is raised.             |
| {$ICMP_TIME_WARN} | 0.15          | Time in seconds where an ICMP response time warning is raised.         |
| {$PING_TIMEOUT}   | 3             | Number of pings to lose before raising an alarm.                       |
| {$SNMP_TIMEOUT}   | 5m            | Amount of time no SNMP data is retrievable before an alarm is raised.  |

## Items Collected

IHT: Interval, History, Trends

| Name                    | Description                                               | Type            | Key & Additional Info                                            |
|-------------------------|-----------------------------------------------------------|-----------------|------------------------------------------------------------------|
| ICMP Loss               | This is a measure of ICMP loss in percentage.             | Simple Check    | icmppingloss<br/>IHT: 1 minute, 90 days, 365 days                |
| ICMP Ping               | Boolean measure of ICMP response.  0: Failure, 1: Success | Simple Check    | icmpping<br/>IHT: 1 minute, 90 days, 365 days                    |
| ICMP Response Time      | This is a measure of ICMP response time in seconds.       | Simple Check    | icmppingsec<br/>IHT: 1 minute, 90 days, 365 days                 |
| SNMP Agent Availability | Availability of SNMP checks on the host.                  | Zabbix Internal | zabbix[host,snmp,available]<br/>IHT: 1 minute, 90 days, 365 days |
| System Contact          | SNMP defined contact                                      | SNMP Agent      | system.contact[sysContact.0]<br/>IHT: 15 minutes, 2 weeks, N/A   |
| System Description      | SNMP defined description                                  | SNMP Agent      | system.descr[SysDescr.0]<br/>IHT: 15 minutes, 2 weeks, N/A       |
| System Location         | SNMP defined location                                     | SNMP Agent      | system.location[susLocation.0]<br/>IHT: 15 minutes, 2 weeks, N/A |
| System Name             | SNMP system name                                          | SNMP Agent      | system.name[sysName.0]<br/>IHT: 15 minutes, 2 weeks, N/A         |
| System Object ID        | Manufacturer system identity                              | SNMP Agent      | system.objectid[sysObjectID.0]<br/>IHT: 15 minutes, 2 weeks, N/A |

Triggers made on system items collected above are outside the scope of this module as the data may be used or manipulated by another module or template.

## Triggers

| Name                    | Description                                                                                                          | Expression                                                                                                    | Priority | Dependency                          |
|-------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|----------|-------------------------------------|
| ICMP Unreachable        | ICMP has been unavailable for the last number of measurements.                                                       | max(/Basic SNMP and ICMP/icmpping, {$PING_TIMEOUT})=0                                                         | High     |                                     |
| High ICMP Loss          | This will trigger a warning when ICMP loss exceeds the percentage value specified in the {$ICMP_LOSS_WARN} macro.    | min(/Basic SNMP and ICMP/icmppingloss,5m)<100 and min(/Basic SNMP and ICMP/icmppingloss,5m)>{$ICMP_LOSS_WARN} | Warning  | ICMP Unreachable                    |
| High ICMP Response Time | This will trigger a warning when ICMP response time exceeds the time value specified in the {$ICMP_TIME_WARN} macro. | avg(/Basic SNMP and ICMP/icmppingsec,5m)>{$ICMP_TIME_WARN}                                                    | Warning  | ICMP Unreachable<br/>High ICMP Loss |
| No SNMP Data Collection | SNMP is not available for polling. Please check device connectivity and SNMP settings.                               | max(/Basic SNMP and ICMP/zabbix[host,snmp,available],{$SNMP_TIMEOUT})=0                                       | Warning  | ICMP Unreachable                    |