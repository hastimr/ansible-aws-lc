Role Name
=========

This role manage [launch configuration](http://docs.aws.amazon.com/AutoScaling/latest/DeveloperGuide/LaunchConfiguration.html).


Requirements
------------

- Ansible 1.7 or higher.
- Tested on Ubuntu 14.04 and Amazon Linux 7

Role Variables
--------------

| parameter             | required | default | comments |
| --------------------- | -------- | ------- | -------- |
| ec2_ami_image_id                   | yes if  ec2_find_ami_name is empty     |         | The AMI unique identifier to be used for the group.|
| ec2_key_name  |no||The SSH key name to be used for access to managed instances|
| ec2_sg_id  |yes||security group id (or list of ids) to use with the instance|
| ec2_instance_profile_name  |no||Name of the IAM instance profile to use.|
| ec2_instance_type  |no|t2.micro| [instance type](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) to use for the instance.|
| ec2_lc_instance_monitoring  |no|True| |whether instances in group are launched with detailed monitoring.|
| ec2_assign_public_ip  |no|True| Used for Auto Scaling groups that launch instances into an Amazon Virtual Private Cloud. Specifies whether to assign a public IP address to each instance launched in a Amazon VPC.|
| ec2_find_ami_name                   | yes if  ec2_ami_image_id is empty    |        | Image name (ami) to find |
| aws_owner_id                   | no      |   self      | Search AMIs owned by the specified owner. Can specify an AWS account ID, or one of the special IDs 'self', 'amazon' or 'aws-marketplace'. If not specified, all EC2 AMIs in the specified region will be searched.|
|ec2_volumes    | no | | |a list of hash/dictionaries of volumes to add to the new instance; '[{"key":"value", "key":"value"}]'; keys allowed are - device_name (str; required), delete_on_termination (bool; False), device_type (deprecated), ephemeral (str), encrypted (bool; False), snapshot (str), volume_type (str), iops (int) - device_type is deprecated use volume_type, iops must be set when volume_type='io1', ephemeral and snapshot are mutually exclusive.  |
| aws_resource_tags  | yes  |   | | a hash/dictionary of tags to add to the new instance or for starting/stopping instance by tag; '{"key":"value"}' and '{"VREnv":"PROD","VRProject":"sample","VRTeam":"infra", "Name":"ami name"}' |
| vivareal_project_build                   | no      |         | Unique name for lc. |
| user_data  |no| | opaque blob of data which is made available to the ec2 instance. Ch-hostname.sh forced override. ||
| state | no  | present  | register or deregister the instance|
| region                   | no      |         | The AWS region to use. If not specified then the value of the AWS_REGION or EC2_REGION environment variable, if any, is used.  |

Dependencies
------------

When this playbook run sucessfull, register variable *ec2_launch_config_name* that is the name of the created launch configuration. You probably need this variable for role [../aws-asg](aws-asg)


Example Playbook
----------------
    - hosts: localhost
          vars:
            ec2_key_name: master
            ec2_find_ami_name: "ubuntu-docker-base-ami-1*"
            ec2_sg_id: ['sg-1958ae61', 'sg-32f1634b', 'sg-ac9e63d4']
            ec2_lc_user_data: |
                docker pull nginx  
                docker run -d nginx
      roles:
        - { role: aws-lc }



License
-------

BSD

Author:
------------------

Giancarlo Rubio (<gianrubio@gmail.com>)
