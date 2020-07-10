Role Name
=========

Role to install and configure Shibboleth service provider.

Optionally this role allows `apache` proxy and provisioning certificates and idp metadata.

Requirements
------------

Ansible version >= 2.7
Tested under `debian 10`

Role Variables
--------------

```yaml

# defaults file for shibboleth-sp

#packages tested under debian 10
shibbolethsp_packages:
  - python-lxml
  - shibboleth-sp2-common
  - shibboleth-sp2-utils
  - libapache2-mod-shib

shibbolethsp_state: latest
shibbolethsp_installation_path: /etc/shibboleth

################################
## shibboleth2.xml parameters ##
################################

#you can uncomment and add as var what you consider
#but entityid and discoveryurl are needed
shibbolethsp_shibboleth2xml:
  applicationDefaults:
    #entityid: "https://example.com"
    #discoveryurl: "https://" 
    #remote_user: "eppn subject-id pairwise-id persistent-id"
    #encryption: "false"    
    #signing: "false"
    #sessions:
      #signing: "front"
      #lifetime: 28800
      #timeout: 3600
      #handlerssl: "true"
      #cookieprops: "https"
    sso:
      #entityid: "https://login.example.com/idp/shibboleth"
    #metadata_generator:
      #location: "/Metadata"
      #signing: "false"
      #organization_name: "org name"
      #organization_display_name: "org name"
      #organization_url: "organization.com"
      #surname: "organization surname"
      #email: "organization@email.com"
    #metadata_provider:
      #path: "{{ idp_metadata.dir }}/{{ idp_metadata.name }}"
      #type: "XML"
      #validate: "true"

################################
## attribute-map parameters ####
################################

#you may need add this if you are using extra attributes
#shibbolethsp_custom_attributes: 
#  - name: "urn:oid:2.5.4.3"
#    id: "cn"
#  - name: "urn:oid:0.9.2342.19200300.100.1.1"
#    id: uid
#  - name: "urn:oid:0.9.2342.19200300.100.1.3"
#    id: mail

################################
###certificates parameters ##
################################

shibbolethsp_with_certificates: false
shibbolethsp_signing_cert: ""
shibbolethsp_signing_key: ""
shibbolethsp_encrypt_cert: ""
shibbolethsp_encrypt_key: ""

################################
## apache parameters ###########
################################

shibbolethsp_with_apache: false
shibbolethsp_apache_packages:
    - ssl
    - socache_shmcb

#you want to add this with your own data
#shibbolethsp_server_name: "example.org"
#shibbolethsp_server_alias: "example.org, example.com"
#shibbolethsp_server_admin: "webmaster@localhost"

shibbolethsp_document_root: "/var/www/"
shibbolethsp_certificate: "/etc/ssl/localcerts/apache.pem"
shibbolethsp_key: "/etc/ssl/localcerts/apache.key"
shibbolethsp_chain: "/etc/ssl/localcerts/fullchain.pem"
#shibbolethsp_require_attr: "example-attr ~ staff@.* faculty@.*"

##################################
## idp configuration parameters ##
##################################

#add this if you want to copy your idp metadata
shibbolethsp_idp_configuration: false
#idp_metadata:
#  - name: idp-metadata.xml
#    src: my-app/idp-metadata.yaml.j2
#    dir: "{{ shibbolethsp_installation_path }}"
#    user: myuser
#    group: mygroup

################################

```

Dependencies
------------



Example Playbook
----------------

```yaml

- hosts: servers
  roles:
   - role: udelarinterior.shibboleth_sp
     vars:
       shibbolethsp_shibboleth2xml:
         applicationDefaults:
         entityid: "https://mysp.com"
         discoveryurl: "https://myidp.com/idp/shibboleth" 
         sso:
             entityid: "https://myidp.com/idp/shibboleth"
         metadata_generator:
             organization_name: "myorg name"
             organization_display_name: "myorg display name"
             organization_url: "organizationurl.com"
             surname: "organization surname"
             email: "organization@email.com"
       shibbolethsp_custom_attributes: 
       - name: "urn:oid:0.9.2342.19200300.100.1.1"
         id: uid
       - name: "urn:oid:0.9.2342.19200300.100.1.3"
         id: mail

```

License
-------

(c) Universidad de la República (UdelaR), Red de Unidades Informáticas de la UdelaR en el Interior. Licenced under GPL-v3.

Author Information
------------------

[@dkmarce](https://github.com/dkmarce)
[@UdelaRInterior](https://github.com/UdelaRInterior)
https://proyectos.interior.edu.uy/