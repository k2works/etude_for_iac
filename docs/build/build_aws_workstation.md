# AWS環境セットアップ

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
cd /vagrant
bundle
rake VPC:create_standard_vpc  
rake EC2:create_key_pair       
cp etude_for_iac.pem ~/.ssh/
chmod 0600 ~/.ssh/etude_for_iac.pem                          
```
以下の.envファイルを作成して読み込む
```bash
cd /vagrant
touch .env
source .env
```

`.env`
```text
export AWS_ACCESS_KEY_ID=XXXXXXX
export AWS_SECRET_ACCESS_KEY=XXXXXXX
export AWS_DEFAULT_REGION=XXXXXX
export VPCID=XXXXXXX
export SGID=XXXXXXX
```

セキュリティグループの作成
```bash
aws ec2 create-security-group --vpc-id $VPCID \
                              --group-name etude-for-iac \
                              --description "Enable HTTP access via port 80 and SSH access via port 22."
```
```bash
aws ec2 authorize-security-group-ingress --group-id $SGID --cidr 0.0.0.0/0 --port 22 --protocol tcp                              
aws ec2 authorize-security-group-ingress --group-id $SGID --cidr 0.0.0.0/0 --port 80 --protocol tcp
```

```bash
cd /vagrant/learn_chef/infrastructure_automation/chef-reop/cookbooks/learn_chef_apache2/
kitchen list
kitchen create
```