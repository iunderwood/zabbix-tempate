# Baytech by SNMP

## Description

This template is designed to monitor power distribution products from Bay Technical Associates: http://www.baytech.net

## Overview

## Macros Used

| Macro           | Default Value | Type     | Description                              |
|-----------------|---------------|----------|------------------------------------------|
| {$OL_POWER_DF}  | 0.1           | float    | Outlet Power Differential Factor         |
| {$OL_POWER_MIN} | 50            | unsigned | Minimum wattage to alert a difference on |
| {$POWER_WINDOW} | 30m           | char     | Minutes to hold voltage alerts           |
| {$VOLT_HIGH}    | 260           | unsigned | General High Voltage Threshold           |
| {$VOLT_LOW}     | 100           | unsigned | General Low Voltage Threshold            |

## Template Links

The following templates are required.

| Template Name       | Repository Link                                                                              |
|---------------------|----------------------------------------------------------------------------------------------|
| Basic SNMP and ICMP | https://github.com/iunderwood/zabbix-template/tree/master/template_module_basic_icmpsnmp/6.4 |
