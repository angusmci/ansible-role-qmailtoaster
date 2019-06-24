# qmailtoaster

This is an Ansible role for installing the `qmailtoaster` mail package, based on `qmail`.

For more information about `qmailtoaster`, see http://qmailtoaster.com.

This is a port to Ansible of Eric Broch's install scripts. At the time of writing this role is UNTESTED and INCOMPLETE.
Any bugs in the role are my fault, not Eric's.

Thanks to the maintainers of `qmailtoaster`, Eric Shubert and Eric Broch, for all their hard work in maintaining, updating and extending the `qmailtoaster` system. Thanks are due also to Daniel Bernstein, the original author of `qmail`, and to everyone else who has contributed to the `qmailtoaster` ecosystem.  

BECAUSE THIS ROLE IS STILL UNDER ACTIVE DEVELOPMENT, IT IS NOT RECOMMENDED FOR PRODUCTION SYSTEMS. NO WARRANTY, EXPRESS OR IMPLIED.

## Requirements

TBD

## Role Variables

The behavior of the role is controlled by various role variables. These are used to specify options (such as
whether to install a particular optional package or not, which version of qmailtoaster to install, and so
on). They are also used to specify database passwords.

### Passwords

The default `vars/main.yml` file in the role does not provide definitions for the database passwords used
in the role. Therefore, if you simply run the role without specifying these passwords, the role will fail
with an error. This is intentional: for the security of your system, you should provide good, non-default
passwords.

The standard `qmailtoaster` install scripts specify rather weak default passwords for the
MariaDB users `vpopmail` and `dspam`. This arguably represents a possible small security risk. For
greater security, this role requires you to specify your own passwords for each of these.

When choosing passwords, you are recommended to avoid using certain characters, specifically punctuation
that has special meaning to the shell (e.g. '&', '|', ' ' or '!') or in SQL (e.g. semicolons and various types 
of quotes). This is because some parts of the install process require the passwords to be passed to the shell
or to MariaDB, and Ansible isn't always entirely helpful in the way that it handles quoted strings. Using 
special characters can cause problems. Using alphanumerics and a restricted set of punctuation characters
(e.g. '=', '-', '_', '+', '.', ',') should be safe.

### Variables 

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

## Dependencies

TBD

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

## License

BSD

## Author Information

Ansible role

 * Angus McIntyre -- http://nomadcode.com/
   
Qmailtoaster install script

 * Eric Broch -- http://whitehorsetc.com/
 * Eric Shubert