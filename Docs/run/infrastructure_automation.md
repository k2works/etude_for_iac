# Infrastructure Automation

## Configure a resource
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
   
## Configure a package and service
1. Ensure the apt cache is up to date   
   

## 参照
+ [Configure a resource](https://learn.chef.io/modules/learn-the-basics/ubuntu/virtualbox/configure-a-resource#/)