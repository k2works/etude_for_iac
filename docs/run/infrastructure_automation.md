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
   knife bootstrap 192.168.33.102 -N chef-client -x vagrant -P vagrant --sudo --run-list 'recipe[learn_chef_apache2]'
   knife node list
   knife client show chef-client
   knife node run_list add chef-client "recipe[learn_chef_apache2]"
   knife node show chef-client
   knife ssh 'hostname:chef-client' 'sudo chef-client' -x vagrant  -P vagrant
   ```   
   
1. Confirm the result      
   ````bash
   curl 192.168.33.102
   ````   

### Update your node's configuration
1. Add template code to your HTML

1. Update your cookbook's version metadata

1. Upload your cookbook to the Chef server
   ```bash
   knife cookbook upload learn_chef_apache2 -o .
   ```
   
1. Run the cookbook on your node
   ```bash
   knife ssh 'hostname:chef-client' 'sudo chef-client' -x vagrant  -P vagrant
   ```   
1. Verify the result
   ````bash
   curl 192.168.33.102
   ````   

### Resolve a failed chef-client run
1. Assign an owner to the home page

1. Apply the changes to your node
   ```bash
   knife cookbook upload learn_chef_apache2 -o .
   knife ssh 'hostname:chef-client' 'sudo chef-client' -x vagrant  -P vagrant
   ```
   
1. Resolve the failure
   ```bash
   knife cookbook upload learn_chef_apache2 -o .
   knife ssh 'hostname:chef-client' 'sudo chef-client' -x vagrant  -P vagrant
   curl 192.168.33.102
   ```  
   
### Run chef-client periodically
1. Get the chef-client cookbook
   ```bash
   cd /vagrant/learn_chef/infrastructure_automation/chef-reop/cookbooks/learn_chef_apache2$
   berks install
   SSL_CERT_FILE='~/.chef/trusted_certs/vagrant_vm.crt' berks upload
   ```    
   
1. Create a role
   ```bash
   mkdir roles
   touch roles/web.json
   knife role from file roles/web.json
   knife role list
   knife role show web
   knife node run_list set chef-client "role[web]"
   knife node show chef-client --run-list 
   ```     

1. Run chef-clien
   ```bash
   knife ssh 'hostname:chef-client' 'sudo chef-client' -x vagrant  -P vagrant
   knife status 'role:web' --run-list
   ```    

1. Next steps
   ```bash
   berks install
   SSL_CERT_FILE='~/.chef/trusted_certs/vagrant_vm.crt' berks upload
   cd ..
   knife cookbook upload learn_chef_apache2 -o .
   ```    
   
1. Delete the node from the Chef server
   ```bash
   knife node delete chef-client --yes
   knife client delete chef-client --yes
   ```     

1. Delete your cookbook from the Chef serve
   ```bash
   knife cookbook delete learn_chef_apache2 --all --yes
   ```   
     
1. Delete the role from the Chef server
   ```bash
   knife role delete web --yes
   ```     
   
## Get started with Test Kitchen
### Get started with Test Kitchen with Ubuntu on Amazon Web Services
1. [Get started with Test Kitchen with Ubuntu on Amazon Web Services](../build/build_aws_workstation.md) 

### Apply a cookbook locally
1. Get the learn_chef_apache2 cookbook from GitHub
   ```bash
   cd /vagrant/learn_chef/infrastructure_automation/chef-reop/cookbooks
   git clone https://github.com/learn-chef/learn_chef_apache2.git learn_chef_apache2_aws
   cd learn_chef_apache2_aws
   ```
   
1. Create the Test Kitchen configuration file   

1. Create the Test Kitchen instance
   ```bash
   kitchen list
   kitchen create
   kitchen list
   ```
   
1. Apply the learn_chef_apache2 cookbook to your Test Kitchen instance
   ```bash
   kitchen converge
   echo $?
   kitchen list
   kitchen converge
   ```   

1. Verify that your Test Kitchen instance is configured as expected
   ```bash
   kitchen exec -c 'wget -qO- localhost'
   ```   
   
1. Delete the Test Kitchen instance
   ```bash
   kitchen destroy
   kitchen list
   ```   
   
### Resolve a failure
1. Assign an owner to the home page

1. Apply the changes to your test instance
   ```bash
   kitchen converge
   echo $?
   ```
   
1. Resolve the failure
   ```bash
   kitchen converge
   kitchen exec -c 'wget -qO- localhost'
   kitchen exec -c 'stat /var/www/html/index.html'
   ```
      
### Appendix: Collaborative development
1. Use dynamic configuration    
   
## 参照
+ [Configure a resource](https://learn.chef.io/modules/learn-the-basics/ubuntu/virtualbox/configure-a-resource#/)