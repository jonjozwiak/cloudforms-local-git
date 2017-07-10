# cloudforms-local-git
Playbook to enable serving local git repositories on a CloudForms appliance.  It is expected that the CloudForms appliance is already deployed and configured for embedded Ansible prior to running this playbook. 

## Usage
Once base appliance configuration is done and the embedded ansible role is enabled, do the following: 

```
ssh root@cloudforms_appliance
cd /tmp
git clone https://github.com/jonjozwiak/cloudforms-local-git.git
cd cloudforms-local-git
vi main.yml
# Update vars as you see fit.  For example: 
    #sslpassphrase: "changeme" 	# Change this to your own passphrase
    # Change the SSL cert info to your own details
      #certcountry: "US"                   # 2 letter code
      #certstate: "North Carolina"         # State/Province
      #certcity: "Raleigh"                 # Locality/city
      #certcompany: "ManageIQ"             # Organization Name
      #certouname: "Licensing"             # Org Unit Name / Section
      #certemail: "jon@example.com"        # Email Address

# Run the playbook
ansible-playbook main.yml

# Once complete, reboot the appliance for the web server configuration to take effect
systemctl status evmserverd 
systemctl status httpd

# NOTE: HTTP logs are at /var/www/miq/vmdb/log/apache

```

Cloudforms will prompt you to accept a new cert on connection to the Web UI.  

You can place git repos in /var/git on the CloudForms appliance (assuming you haven't changed directory names).  
You can access them read-only at https://cloudforms_appliance:8443/RepoName

``` git clone https://cloudforms_appliance:8443/Ansible_Playbooks ```

