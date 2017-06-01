# Windows Ansible Playbooks

This is a collection of Ansible playbooks/roles that I wrote (with the exception of one role called from Ansible Galaxy that I wrote a playbook around).  My main motivation for putting these together was for integration into the provisioning process in Red Hat CloudForms/ManageIQ, but they can certainly be used as standalone playbooks with Ansible Core or Ansible Tower.

## WinUpdate

This role is pretty self explanatory - this will install windows updates on the host you run it against.  Be advised that it will take a VERY LONG TIME to do this on a host with a clean install of Windows that has had no prior updates installed, so take that into consideration

You can use the extra variable "win_updat_cat_names", with choices in an array, like so: 

win_update_cat_names: ['CriticalUpdates','SecurityUpdates']

Valid choices are:
- Application
- Connectors
- CriticalUpdates
- DefinitionUpdates
- DeveloperKits
- FeaturePacks
- Guidance
- SecurityUpdates
- ServicePacks
- Tools
- UpdateRollups
- Updates

## AD Join

This role  will join the host to an Active Directory domain. It is assumed DNS is already pointing at the appropriate DNS servers to able to resolve the domain.  

Extra variables required are: domain (example.com), user (example\administrator or administrator@example.com are both valid) and password (password for the account used to join. It will be passed to powershell as an encrypted string, but make sure you take appropriate measures to protect the password within Ansible Tower using vault).

## sqlplay.yml

This requires the bahaula.win_sqlexpress2014 role from Ansible Galaxy.  This role downloads, extracts & installs SQL Express 2014.  Be advised this will take quite a while depending on network speed and the specs of the host - it took about 35 minutes from job instantiation to completion on a windows VM with 1 CPU & 1 GB of RAM for reference.  There is an "instance" variable that is defined as "SQLEXPRESS", which can be overridden with extra vars.

## iisplay.yml

This is a simple playbook to enable IIS on a host.  I'll eventually develop this into a role and provide more configuration options. Current, it accepts "hostname" and "appool" variables. Ihave also added a 5 second pause between enabling the Web-Server role and importing the WebAdministration module to mitigate a possible race condition where the installed powershell modules hasn't refreshed after installation in the previous step.  
