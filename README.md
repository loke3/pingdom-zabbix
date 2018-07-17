Main contributer of this project is Mark Mercado <mamercad@umflint.edu>

I just ammended and created this project to include transcational checks into zabbix

pingdom-zabbix_d_to_z.py - This will pull Checks information from pingdom and send to zabbix.
pingdom-zabbix_v_to_z.py - This will pull data (status & respose time, etc) for respective checks and send to zabbix.
pingdom-zabbixtxn_d_to_z.py - This will pull transcational check information from pingdom and send to zabbix.
pingdom-zabbixtxn_v_to_Z.py - This will pull transcational data(status) information from pingdom and send to zabbix.


#### Overview

This script brings Pingdom checks into Zabbix. It uses Pingdom's API to fetch
the checks and statuses (currently, 'status' and 'lastresponsetime'), and then
produces `zabbix_sender` commands to load the data into Zabbix. Currently, we
just `cron` the script based on the granularity we're interested in.

#### Pingdom

For its API, Pingdom uses a combination of HTTP authentication (your Pingdom
credentials) and an HTTP header (`App-Key`). By way of the Pingdom interface,
you can create your application key.

### Zabbix

For Zabbix, we need a host and a template. We've created a host named 'Pingdom'
and a template named 'Template Pingdom'. The Pingdom template uses Zabbix
low-level discovery to create the items dynamically (`key1` in the configuration
file). Based on the discovered items (i.e., your Pingdom checks), it'll create
two items (based on `key2` and `key3`), Pingdom `status` and `lastresponsetime`.
The script turns Pingdom's `status` from 'up' to 1 and 'down' to 0 (could do it
with Zabbix value maps, but I didn't feel like it at the time).

#### Usage

Rename `pingdom-zabbix.ini.sample` to `pingdom-zabbix.ini` and update
accordingly.

#### Python modules

```python
import json
import re
import requests
import subprocess
import ConfigParser
```
