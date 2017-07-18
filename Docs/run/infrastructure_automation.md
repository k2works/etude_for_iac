# Infrastructure Automation
## Learn the Chef basics
### Configure a resource
1. Set up your working directory

1. Create the MOTD file
   ```bash
   chef-client --local-mode hello.rb
   ```
1. Update the MOTD file's contents
   ```bash
   chef-client --local-mode hello.rb
   ```
1. Ensure the MOTD file's contents are not changed by anyone else

1. Delete the MOTD file
   ```bash
   chef-client --local-mode goodbye.rb
   ```
   
### Configure a package and service
1. Ensure the apt cache is up to date   
   
1. Install the Apache package
   ```bash
   sudo chef-client --local-mode webserver.rb
   ```   

1. Start and enable the Apache service
   ```bash
   sudo chef-client --local-mode webserver.rb
   ```      

1. Add a home page
   ```bash
   sudo chef-client --local-mode webserver.rb
   ```
   
1. Confirm your web site is running
   ```bash
   curl localhost
   ```         

### Make your recipe more manageable
1. Create a cookbook
   ```bash
   mkdir cookbooks
   chef generate cookbook cookbooks/learn_chef_apache2
   ```

1. Create a template
   ```bash
   chef generate template cookbooks/learn_chef_apache2 index.html
   ```
   
1. Update the recipe to reference the HTML template

1. Run the cookbook
   ```bash
   sudo chef-client --local-mode --runlist 'recipe[learn_chef_apache2]'
   curl localhost
   ```

## Manage a node with Chef server
### Upload a cookbook to Chef server
1. Upload your cookbook to the Chef server
   ```bash
   cd /vagrant/learn_chef/infrastructure_automation/chef-reop/cookbooks/   
   knife cookbook upload learn_chef_apache2 -o .
   knife cookbook list
   ```
   
### Bootstrap your node
1. Run the bootstrap command
   ```bash
   knife bootstrap 192.168.33.102 -N chef-client -x vagrant -P vagrant --sudo
   knife node list
   knife client show chef-client
   ```   
   
## 参照
+ [Configure a resource](https://learn.chef.io/modules/learn-the-basics/ubuntu/virtualbox/configure-a-resource#/)