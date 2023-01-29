ansible.cfg to use dynamic hosts from AWS
Steps:
go to ansible.cfg added enable_plugins = aws_ec2
create inventory_aws_ec2.yaml
set inventory = inventory_aws_ec2.yaml to ansible.cfg
set remote_user = ec2-user and private_key_file = ~?/.ssh/id_rsa (globally configuration)
updated playbook hosts = aws_ec2

show results from inventory plugin:
ansible-inventory -i inventory_aws_ec2.yaml

Execute:
ansible-playbook docker-Ec2.yaml

Target specific hosts using filter
<<inventory_aws_ec2.yaml
filters:
  instance-state-name: running
  tag:Name: dev*
<<

documentation: https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html#options

Create Dynamic Group using keyed_groups
<<inventory_aws_ec2.yaml
keyed_groups:
    - key: tags
      prefix: tag
    - key: instance_type
      prefix: instance_type  
<<
documentation: https://docs.ansible.com/ansible/latest/collections/amazon/aws/aws_ec2_inventory.html#parameter-filters
