# cisco_imc_cookbook README
***

This cookbook configures the Cisco IMC Servers(standalone).

## Install
***
Follow the steps outlined below to install the cookbook   
  1. Move the cookbook under chef-repo folder on your workstation.
  2. Create your recipes, sample recipes are provided in the cookbook. 
     It is a good practice to set the cisco imc accounts in the data bag of chef, 
     because you will need to mention the data bag item name in the auth_data parameter of the recipe.
  2. Run the command on the workstation from under the chef-repo folder.   
    * knife cookbook upload cisco-imc-chef
  3. Add recipe to the run-list of the chef node and run chef-client.

## Setting the Cisco IMC Accounts in Chef
***
Follow the steps outlined below to setup the Cisco IMC Accounts in chef.
1. Create a data bag in chef with the name "cisco_imc_accounts". This data bag name is configurable in the default.rb attribute file.
2. Now, create an item under the data bag. 
3. Edit each item and add json data as follows.   
  
* {   
    * "user": "admin",   
    * "password": "********",   
    * "port": "80",
    * "secure":false
* }
* Note: You can create multiple such data bag items, depending upon the number of account you have.

## uninstall
***
Follow the steps outlined below to un-install the cookbook.
  1. Run the command on chef workstation from under the chef-repo folder.   
    * knife cookbook delete cisco-imc-chef   

## Requirements
***

### Platforms
***

- CentOS Release 6.5

### Ruby
***

- Ruby v2.2.x or later

### Chef
***

- Chef 12.0 or later

### Cookbooks
***

- None

### Cisco IMC Versions
***

- Cisco IMC v2.0(3) or later


## Attributes
***

* default[:databag][:name]    =  "cisco_imc_accounts"

***

<table>
  <tr>
    <td><tt>[:databag][:name]</tt></td>
    <td>String</td>
  </tr>
</table>

## Usage
***

### Recipe connecting to single Cisco IMC Server.
***
Note: auth_data should be set to the data bag item name that should be used to authenticate with the IMC Server.
* cisco_imc_user_config 'cisco_imc_user_config' do  
    * ipaddress "10.105.219.15"
    * auth_data "imc_account"
    * account_status 'active'  
    * username 'testuser'   
    * password '*******'   
    * priv "read-only"   
    * action :create   
* end

### Recipe connecting to multiple Cisco IMC Server from a chef client
***
Note: Assuming imc_servers contains a list of imc_servers to be configured from a chef client node. 
* for imc_server in imc_servers   
    * cisco_imc_ntp_config "cisco_imc_ntp_config-#{imc_server}" do   
      * ipaddress imc_server
      * auth_data "imc_account"
      * ntp_state 'yes'   
      * ntp_servers [{:id => 2, :ip => "10.105.219.95"}, {:id => 3, :ip => "10.105.219.106"}]   
      * action :enable   
    * end 
* end   


## Contributing
***

TBD

## License
***
Copyright 2017 Cisco Systems, Inc.

Licensed under the Apache License, Version 2.0 (the "License");   
you may not use this file except in compliance with the License.   
You may obtain a copy of the License at   

    http://www.apache.org/licenses/LICENSE-2.0   

Unless required by applicable law or agreed to in writing, software   
distributed under the License is distributed on an "AS IS" BASIS,   
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.   
See the License for the specific language governing permissions and   
limitations under the License.   

## Authors
***
Name: Amit Mandal   
email: amimanda@cisco.com   