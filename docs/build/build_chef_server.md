# Chef Serverセットアップ

## 目的

## 前提
## 前提
| ソフトウェア     | バージョン    | 備考         |
|:---------------|:-------------|:------------|
| vagrant        | 1.8.5        |             |
| chef-dk        | 1.4.3        |             |
| chef-server    |              |             |

## 構成
+ セットアップ

### セットアップ
```bash
vagrant up chef_server
vagrant up workstation
vagrant up chef_client
```
#### Chef Server セットアップ
```bash
vagrant ssh chef_server
sudo chef-server-ctl install chef-manage
sudo chef-server-ctl reconfigure
sudo chef-manage-ctl reconfigure 
sudo chef-server-ctl test
```
ユーザー, organization作成
```bash
sudo chef-server-ctl user-create admin firstname lastname your@mail.address password --filename admin.pem
sudo chef-server-ctl org-create chef "Chef" --association admin --filename chef-validator.pem
ls
```
Workstationに鍵を転送
```bash
scp admin.pem vagrant@192.168.33.101:/home/vagrant/.chef/admin.pem
scp chef-validator.pem  vagrant@192.168.33.101:/home/vagrant/.chef/chef-validator.pem
```

#### Chef Workstation セットアップ
```bash
vagrant ssh workstation
knife configure
knife ssl fetch -s https://chef-server/organizations/chef/
knife ssl check
knife client list
knife user list
```
`knif.rb`
```ruby
log_level                :info
log_location             STDOUT
node_name                'admin'
client_key               '/home/vagrant/.chef/admin.pem'
validation_client_name   'chef-validator'
validation_key           '/home/vagrant/chef/chef-validator.pem'
chef_server_url          'https://vagrant.vm/organizations/chef/'
syntax_check_cache_path  '/home/vagrant/.chef/syntax_check_cache'
```
WorkstationからChef Serverにnodeを登録する
```bash
knife bootstrap 192.168.33.102 -N chef-client -x vagrant -P vagrant --sudo
knife node list
knife client show chef-client
```
WorkstationからChef ClientにRun Listを適用
````bash
git clone https://github.com/chef-cookbooks/apt.git
knife cookbook upload apt -o .
knife cookbook list
knife node run_list add chef-client "recipe[apt]"
knife node show chef-client
````

#### Chef Node セットアップ
Chef Clientの準備
```bash
vagrant ssh chef_client
```

#### 稼動確認
Chef ClientからRecipeを実行する
```bash
knife ssh 'hostname:chef-client' 'sudo chef-client' -x vagrant  -P vagrant
```

## 参照
+ [Vagrantで作った環境にChef12 でChef Serverをためしてみた](https://gist.github.com/kazu69/0efcc34d02f5443bf0a8)
+ [Chef Server 環境構築手順](http://qiita.com/kentarok/items/c487490d48621fb503cc)