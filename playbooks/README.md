The two Ansible playbooks create a EC2 instance and install apache on the created instance

1) Playbook to create ec2 instance

In this playbook I have tried to provision ec2 instance using Ansible's 
AWS modules which work through the AWS CLI. I used AMI - image id, VPC - group id to replicate vm's
from an existing stable ec2 instance. 

2) Playbook to install apache, deploy static content and test the URI. 

In this simplified version of playbook, I have tried to install apache web server, deploy the static 
content and test the site. http status return 200 is displayed in the output

Following is the ansible-lint and ansible playbook terminal output for the two playbooks. 

[root@ip-xx ~]# ansible-lint --version
ansible-lint 3.4.12

[root@ip-xx playbooks]# ansible-lint aws_provision_instance.yml
[root@ip-xx playbooks]# 

[root@ip-172-31-45-26 playbooks]# ansible-lint tasks/main2.yml 
[ANSIBLE0010] Package installs should not use latest
tasks/main2.yml:10
Task/Handler: Execute All Installations


[root@ip-xx playbooks]# ansible-playbook aws_provision_instance.yml

PLAY [local] *************************************************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************************************
ok: [ip-xx]

TASK [Basic provisioning of a t2.micro EC2 instances] ********************************************************************************************************************************
changed: [ip-xx]

PLAY RECAP ***************************************************************************************************************************************************************************
ip-xx            : ok=2    changed=1    unreachable=0    failed=0   

[root@ip-xx playbooks]# 


[root@ip-xx tasks]# ansible-playbook aws_apache.yml 

PLAY [aws] ***************************************************************************************************************************************************************************

TASK [Gathering Facts] ***************************************************************************************************************************************************************
ok: [ec2-13-59-114-120.us-east-2.compute.amazonaws.com]

TASK [Execute All Installations] *****************************************************************************************************************************************************
ok: [ec2-13-59-114-120.us-east-2.compute.amazonaws.com] => (item=[u'httpd', u'wget', u'openssl'])

TASK [RestartHTTPD] ******************************************************************************************************************************************************************
changed: [ec2-13-59-114-120.us-east-2.compute.amazonaws.com]

TASK [CopySiteFiles] *****************************************************************************************************************************************************************
changed: [ec2-13-59-114-120.us-east-2.compute.amazonaws.com]

TASK [WaitForSite] *******************************************************************************************************************************************************************
ok: [ec2-13-59-114-120.us-east-2.compute.amazonaws.com]

TASK [TestSite] **********************************************************************************************************************************************************************
ok: [ec2-13-59-114-120.us-east-2.compute.amazonaws.com]

TASK [DisplayOutput] *****************************************************************************************************************************************************************
ok: [ec2-13-59-114-120.us-east-2.compute.amazonaws.com] => {
    "site_result": {
        "accept_ranges": "bytes", 
        "changed": false, 
        "connection": "close", 
        "content_length": "108", 
        "content_type": "text/html; charset=UTF-8", 
        "date": "Mon, 26 Jun 2017 13:58:24 GMT", 
        "etag": "\"4203c-6c-552dd57d5ce31\"", 
        "last_modified": "Mon, 26 Jun 2017 13:58:18 GMT", 
        "msg": "OK (108 bytes)", 
        "redirected": false, 
        "server": "Apache/2.2.32 (Amazon)", 
        "status": 200, 
        "url": "http://localhost"
    }
}

PLAY RECAP ***************************************************************************************************************************************************************************
ec2-13-59-114-120.us-east-2.compute.amazonaws.com : ok=7    changed=2    unreachable=0    failed=0   

[root@ip-172-31-45-26 tasks]# 



