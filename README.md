# AAP-Builder

```
echo "# AAP-Builder" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/alpha-wolf-jin/AAP-Builder.git
git config --global credential.helper 'cache --timeout 7200'
git push -u origin main

git add . ; git commit -a -m "update README" ; git push -u origin main
```

https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html-single/ansible_builder_guide/index

https://docs.google.com/document/d/1qUO4o51tnp3RmrwAlfRE_Sm46L_ugtWynd1NotpiY2k/edit#heading=h.h46x7cgr87ug

# Create customized execute image

Add the tzdata rpm 

```
# pip3 install ansible-builder

# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

192.168.122.31	aap-ctrl-01.example.com	aap-ctrl-01
192.168.122.32	aap-db-01.example.com	aap-db-01
192.168.122.33	aap-hub-01.example.com	aap-hub-01
192.168.122.34	aap-exec-01.example.com	aap-exec-01

[root@aap-ctrl-01 lab]# podman login -u=admin -p=redhat aap-hub-01.example.com --tls-verify=false
Login Succeeded!

# cat ansible.cfg 
# 

# cat bindep.txt 
tzdata [platform:rpm]

# cat execution-environment.yml 
version: 1


ansible_config: 'ansible.cfg' 

dependencies: 
  system: bindep.txt

# ansible-builder build -t utc_ee_image
Running command:
  podman build -f context/Containerfile -t utc_ee_image context


# podman images
REPOSITORY                                TAG         IMAGE ID      CREATED        SIZE
localhost/utc_ee_image                    latest      f5bb2d1bd3ab  5 minutes ago  994 MB

# podman tag f5bb2d1bd3ab aap-hub-01.example.com/utc_ee_image

# podman images
REPOSITORY                                TAG         IMAGE ID      CREATED         SIZE
localhost/utc_ee_image                    latest      f5bb2d1bd3ab  11 minutes ago  994 MB
aap-hub-01.example.com/utc_ee_image       latest      f5bb2d1bd3ab  11 minutes ago  994 MB

# podman push aap-hub-01.example.com/utc_ee_image:latest --tls-verify=false


```

### Click the resouce group `Execution Environment` under `Administration`

Configure the new execution image pointing to the newly create customized image in autohub

![Once Login Azure portal](images/aap-api-01.png)
