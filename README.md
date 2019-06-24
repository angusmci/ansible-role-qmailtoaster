qmailtoaster
============

This is an Ansible role for installing the `qmailtoaster` mail package, based on `qmail`.

For more information about `qmailtoaster`, see http://qmailtoaster.com.

This is a port to Ansible of Eric Broch's install scripts. At the time of writing this role is UNTESTED and INCOMPLETE.
Any bugs in the role are my fault, not Eric's.

Thanks to the maintainers of `qmailtoaster`, Eric Shubert and Eric Broch, for all their hard work in maintaining, updating and extending the `qmailtoaster` system. Thanks are due also to Daniel Bernstein, the original author of `qmail`, and to everyone else who has contributed to the `qmailtoaster` ecosystem.  

BECAUSE THIS ROLE IS STILL UNDER ACTIVE DEVELOPMENT, IT IS NOT RECOMMENDED FOR PRODUCTION SYSTEMS. NO WARRANTY, EXPRESS OR IMPLIED.

Requirements
------------

TBD

Role Variables
--------------

    qmt_mariadb_secure: yes
   
Should the MariaDB database be secured on setup? Leave this as the default value to secure MariaDB, unless you have an extremely good reason not to.

    qmt_use_dspam: yes
   
Should dspam be installed? 

    qmt_use_dspam_domains: no
   
Should dspam be used for individual domains? 

    qmt_dspam_domains
    
List of domains for which dspam should be used.

    qmt_db_root_password:
    qmt_db_vpopmail_password:
    qmt_db_dspam_password:
    
No default values are provided for these variables. You must provide values for these variables or the role will not work. Ideally, these should be defined in an Ansible vault for greater security. 

Dependencies
------------

TBD

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

Ansible role

 * Angus McIntyre -- http://nomadcode.com/
   
Qmailtoaster install script

 * Eric Broch -- http://whitehorsetc.com/