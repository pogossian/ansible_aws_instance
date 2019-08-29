## Automatically creating instances in AWS for web applications end load balancers

*Ansible playbook should be ran from same VPC*


1) Clone project

```
git clone https://github.com/pogossian/ansible_aws_instance.git
```

2) Go to project directory, remove old .git repository and initialize a new one.

```
cd ansible_aws_instance/
rm -rf .git
git init
```

3) Change variables in aws_vars.yml

  *nginx template was created for ubuntu 18, due not recommended to change image, keypair should be ansible instance key, from which playbook will run, then specify group_id, instance_type and region*

4) Then we’ll need to store the AWS keys. Since they are sensitive data, we should use Ansible vault for this

```
ansible-vault create aws_keys.yml
```

Once open, add the following to it:

```
aws_access_key: AKIAJLHNMCBOITV643UA
aws_secret_key: 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
```

5) And finally change sunbnets and number of in instances instances.yml

  *first name in instances.yml its apps, second load balancers, also you may changed thems*


6) Run playbook

```
ansible-playbook --ask-vault-pass playbook.yml --extra-vars "@instances.yml"
```


Playbook will check and create all necessary instances.

'App' don't have public ip, so be sure they have nat gateway and can connect to repository for installing software.

Ultimately on all instances would be with installed nginx, and 'web' instances will proxy pass 'app' if they in a same availability zone.
