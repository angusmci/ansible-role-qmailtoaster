# qmailtoaster

This is an Ansible role for installing the `qmailtoaster` mail package, based on `qmail`.

For more information about `qmailtoaster`, see http://qmailtoaster.com.

This is a port to Ansible of Eric Broch's install scripts. At the time of writing this role is UNTESTED and INCOMPLETE.

Any bugs in the role are my fault, not Eric's.

Thanks above all to the present and past maintainers of `qmailtoaster`, Eric Broch and Eric Shubert, for all their hard work in maintaining, updating and extending the `qmailtoaster` system. Thanks also to other members of the `qmailtoaster` community, especially Remo Mattei, for their help and comments, and special thanks to Remi Collet for help with installing his PHP repos. Thanks are due also to Daniel Bernstein, the original author of `qmail`, and to everyone else who has contributed to the `qmailtoaster` ecosystem.

BECAUSE THIS ROLE IS STILL UNDER ACTIVE DEVELOPMENT, IT IS NOT RECOMMENDED FOR PRODUCTION SYSTEMS. NO WARRANTY, EXPRESS OR IMPLIED.

## Requirements

TBD

## Configuration Variables

The behavior of the role is controlled by various role variables. These are used to specify options (such as whether to install a particular optional package or not, which version of qmailtoaster to install, and so on). They are also used to specify database passwords.

### Password variables

The default `vars/main.yml` file in the role does not provide definitions for the database passwords used in the role. Therefore, if you simply run the role without specifying these passwords, the role will fail with an error. This is intentional: for the security of your system, you should provide good, non-default passwords.

The standard `qmailtoaster` install scripts specify rather weak default passwords for the MariaDB `vpopmail` user. This arguably represents a possible small security risk. For greater security, this role requires you to specify your own passwords.

When choosing passwords, you are recommended to avoid using certain characters, specifically punctuation that has special meaning to the shell (e.g. '&', '|', ' ' or '!') or in SQL (e.g. semicolons and various types of quotes). This is because some parts of the install process require the passwords to be passed to the shell or to MariaDB, and Ansible isn't always entirely helpful in the way that it handles quoted strings. Using special characters can cause problems. Using alphanumerics and a restricted set of punctuation characters (e.g. '=', '-', '\_', '+', '.', ',') should be safe.

    qmt_db_root_password:
    qmt_db_vpopmail_password:

### General QMailToaster configuration

    qmt_mariadb_secure: yes

Should the MariaDB database be secured on setup? Leave this as the default value to secure MariaDB, unless you have an extremely good reason not to.

    qmt_install_vpopmaild: no

Should `vpopmaild` be installed? The `vpopmaild` daemon is required for Roundcube, so if you install Roundcube, set this to `yes`.

### Roundcube configuration

    qmt_install_roundcube: no

Should the Roundcube webmail client be installed?

    qmt_roundcube_des_key: "Change_this_24Byte_Str!!"

If you install Roundcube, you must specify a DES key. This key must be a string containing exactly 24 characters. Do not simply use the default value, or your Roundcube installation will be insecure.

### Rainloop configuration

    qmt_install_rainloop: no

Should the Rainloop webmail client be installed?

    qmt_rainloop_license: community

If Rainloop is installed, should it be the `community` or `standard` version?

    qmt_rainloop_domains:

Which domains should Rainloop handle? If you install Rainloop, this variable should be a list of objects that specify the name of a domain, and the name of the hosts that handle SMTP and IMAP for that domain. For example:

    qmt_rainloop_domains:
      - domain: domain1.com
    	imap_host: imap.domain1.com
    	smtp_host: smtp.domain1.com
      - domain: domain2.com
    	imap_host: mail.domain2.com
    	smtp_host: mail.domain2.com

### PHP configuration

    qmt_timezone: "America/New_York"

What timezone should be used for this server? This must be chosen from the list of [PHP supported timezones](https://www.php.net/manual/en/timezones.php).

    qmt_attachment_size_limit: 10

Maximum size of attachments that can be uploaded for sending through any webmail UI on this server.

### SSL Configuration

    qmt_use_httpd_ssl: no

Should the server be set up to use a provided SSL certificate in place of the automatically-generated self-signed certificate? You probably want to get a proper server certificate, and set this option to `yes`.

    qmt_httpd_ssl_certificate_file: server.crt

Name of the SSL certificate key file. This file should be placed in `/etc/pki/tls/private/`.

    qmt_httpd_ssl_certificate_key_file: server.key

Name of the SSL certificate authority file. This file should be placed in `/etc/pki/tls/certs/`.

    qmt_httpd_ssl_certificate_authority_file: server-ca-bundle.crt

Name of the SSL certificate bundle file. This file should be placed in `/etc/pki/tls/private/`.

### Domain configuration

Most of this role is intended to be as 'unopinionated' as possible, or at least no more opinionated than QMailToaster. It is intended to follow the standard scripts on the [QMailToaster](http://qmailtoaster.org/) website closely, so that it can be used to build a basic QMailToaster server.

If you wish, however, you can use this role to configure domains and users on your servers. This is the point at which the role becomes very opinionated indeed, and more than a little complicated.

Domain configuration allows you to specify the desired domains and users as a nested configuration structure. The structure specifies how to create the '[role accounts](http://www.faqs.org/rfcs/rfc2142.html)' (such as 'postmaster', 'webmaster' 'info', 'support', 'sales' etc) for each domain, what aliases to add, and which mailboxes to create, and how mail to those various addresses should be handled.

Let's start with role accounts, which are the standard accounts that are either required -- such as 'postmaster' -- or frequently present -- such as 'sales' or 'info'. To make the configuration less complex, role accounts are organized into five groups: business, network, owner, services, webmaster. For example, the 'owner' accounts include 'admin', and 'billing'. All accounts belonging to a particular group will be handled the same way, so if the configuration said that mail to the 'owner' group should be deleted, then any mail sent to either 'admin' or 'billing' will be deleted; if the configuration says that mail for the 'network' group should go to 'fred@yourdomain.com', then Fred will get any mail sent to 'abuse', 'security' or 'noc' (the standard members of the 'network' group). If you don't like this, you can set the action for the group to 'none', and define aliases individually.

Actions specify how mail should be handled. We'll see actions used with both role groups and aliases. Possible actions are:

none
: No .qmail files will be created
delete
: Mail sent to this alias or group will be deleted unread.
deliver
: Mail sent to this alias or group will be delivered to a local mailbox.
forward
: Mail sent to this alias or group will be forwarded to a remote address.
filter
: Mail sent to this alias or group will be filtered using `procmail` before delivery.
_... more text will follow here ..._

## Dependencies

TBD

## Example Playbook

Here is an example of invoking the role.

    - name: install qmailtoaster
      include_role:
    	name: ansible-role-qmailtoaster
      vars:
    	qmt_version: 1-7
    	qmt_secure_mariadb: yes
    	qmt_install_vpopmaild: yes
    	qmt_install_roundcube: yes
    	qmt_install_rainloop: yes
    	qmt_db_root_password: "{{ my_password1 }}"
    	qmt_db_vpopmail_password: "{{ my_password2 }}"
    	qmt_db_roundcube_password: "{{ my_password4 }}"
    	qmt_roundcube_des_key: "an.3x4mpl3.DES.key.12345"
    	qmt_rainloop_license: community
    	qmt_rainloop_domains:
    	  - domain: example.com
    		imap_host: server1.example.com
    		smtp_host: server2.example.com
    	  - domain: example.net
    		imap_host: mail.example.net
    		smtp_host: mail.example.net
    	qmt_use_httpd_ssl: yes
    	qmt_timezone: America/New_York
    	qmt_rainloop_admin_password: "{{ my_password5 }}"
    	qmt_attachment_size_limit: 25
    	qmt_domains:
    	  - name: example.com
    		password: "{{ my_password6 }}"
    		default:
    		  action: delete
    		business:
    		  action: filter
    		  user: fred
    		  host: example.com
    		owner:
    		  action: filter
    		  user: bob
    		  host: example.com
    		network:
    		  action: filter
    		  user: jane
    		  host: example.com
    		services:
    		  action: filter
    		  user: elizabeth
    		  host: example.com
    		webmaster:
    		  action: filter
    		  user: fred
    		  host: example.com
    	  - name: example.net
    		password: "{{ my_password6 }}"
    		default:
    		  action: delete
    		business:
    		  action: none
    		owner:
    		  action: filter
    		  user: bob
    		  host: example.com
    		network:
    		  action: filter
    		  user: jane
    		  host: example.com
    		services:
    		  action: filter
    		  user: elizabeth
    		  host: example.com
    		webmaster:
    		  action: none

## License

BSD

## Author Information

Ansible role

- Angus McIntyre -- http://nomadcode.com/

Qmailtoaster install script

- Eric Broch -- http://whitehorsetc.com/
- Eric Shubert
