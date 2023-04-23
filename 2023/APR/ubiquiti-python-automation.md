# Ubiquiti Automation using Python
In my free time, I have begun exploring network automation, using Python. Since I own some business-grade Ubiquiti networking gear, I thought I'd give target Ubiquiti first.

# Python Libraries
I am using the following python libraries to write my automation scripts:
* pyunifi
* json

# Script Examples
### Automated Access Point Restart
Here is some base code to the following script I wrote which automates restarting a UAP, without having to jump into the unifi portal.

```
# Compiled by Kaleb McGaugh Mohr
# Fri 21 Apr 2023

from pyunifi.controller import Controller
# !! FOR UDM PRO ONLY
# controller = Controller('[public_ip]', 'unifi_cloud_email_username', 'password', ssl_verify = False, port=443, version='UDMP-unifiOS')

# FOR CLOUDKEY GEN AND UNIFI DREAM MACHINE OR ALL OTHER OS CONSOLES
# controller = Controller('[public_ip]', 'unifi_cloud_email_username', 'password', ssl_verify = False)

def restartAccessPoint():
    controller.restart_ap_name('[Access Point Name]')
    print("Access point has been restarted. Please confirm in the OS console.")

restartAccessPoint()
```

### List Active Clients
The following base code is for a script I wrote that lists active clients.

```
import json
from pyunifi.controller import Controller


# for cloudkey, udr, and all other os consoles
# controller = Controller('[os_console_public_ip]', '[cloud_login_email_username]', '[password]', ssl_verify = False)

# for udm pro
# controller = Controller('[os_console_public_ip]', '[cloud_login_email_username]', '[password]', ssl_verify = False, port = 443, version='UDMP-unifiOS')

def getClients():
    clients = controller.get_clients()
    print(json.dumps(clients, indent=4))


getClients()
```

### Reliability Check
The following base code is for a script I wrote that performs a reliability check, with an option to restart a specific access point, prompting for the name of the access point chosen to be restarted.

```
# Kaleb Mohr, CCNA.
# Compiled on April 23rd, 2023

from pyunifi.controller import Controller
import json
import time

# !! for UDM pro only
# controller = Controller('[os_console_public_ip]', '[unifi_email_username]', '[unifi_password]', ssl_verify = False, port = 443, version='UDMP-unifiOS')

## for all other unifi os consoles
# controller = Controller('[os_console_public_ip]', '[unifi_email_username], '[unifi_password]', ssl_verify = False)

def restartAccessPoint():
    get_ap_name = input('AP name: ')
    time.sleep(2)
    print('Restarting AP...')
    controller.restart_ap_name(get_ap_name)

def reliabilityCheck():
    print('Getting Unifi Alerts...')
    time.sleep(2)
    alerts = controller.get_alerts()
    print(json.dumps(alerts, indent=2))
    print('Please carefully review the above alerts.')
    time.sleep(3)
    restart_opt = input('Would you like to restart any APs? (y/n): ')
    print(restart_opt)
    if restart_opt == 'y':
        restartAccessPoint()
    elif restart_opt == 'n':
        print('Not taking action.')
    else:
        print('You specified an invalid option. Please rerun the script and try again.')

reliabilityCheck()
```
# Self-Testing
If you want to self-test or demo the code, install `pyunifi` library in python3. Run `pip3 install pyunifi` in your terminal to install the library. Then you can copy and paste my code and change the controller parameters and other options to fit your needs. Go <a href="https://github.com/finish06/pyunifi">here</a> for the pyunifi API.
