qmailtoaster
============

This is an Ansible role for installing the `qmailtoaster` mail package, based on `qmail`.

For more information about `qmailtoaster`, see http://qmailtoaster.com.

This is a port to Ansible of Eric Broch's install script. At the time of writing this role is UNTESTED and INCOMPLETE.
Any bugs in the role are my fault, not Eric's.

DO NOT USE THIS FOR PRODUCTION (or anything else). NO WARRANTY, EXPRESS OR IMPLIED.

Requirements
------------

TBD

Role Variables
--------------

    mariadb_secure: yes
   
Should the MariaDB database be secured on setup? Leave this as the default value to secure MariaDB, unless you have an extremely good reason not to.

    mariadb_root_password:
    mariadb_vpopmail_password:
    
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