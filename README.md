# we will start new readme here

## rebuild container

docker build -t sauliai/signal-server .docker/signal-server/local


## connecting with psql client

    psql -h 172.22.0.2 -U postgres

where 172.22.0.2 is the ip address of signal-server-188-db-1 which you can check using:

    docker inspect signal-server-188-db-1


# postgres stuff

bash-5.1# psql -U postgres -c 'SHOW config_file';
config_file
------------------------------------------
/var/lib/postgresql/data/postgresql.conf
(1 row)

The /var/lib/postgresql/data is mapped to ./db through docker-compose.yml. 
The postgresql service automatically modifies /var/lib/postgresql/data dir to be visible and accessible to the root use only.

To modify configuration files you have to use root account (through sudo for example).



Change password_encryption to md5 in postgresql.conf

Windows: C:\Program Files\PostgreSQL\13\data\postgresql.conf
GNU/Linux:           /etc/postgresql/13/main/postgresql.conf

password_encryption=md5

enter image description here

Change scram-sha-256 to md5 in pg_hba.conf

Windows: C:\Program Files\PostgreSQL\13\data\pg_hba.conf
GNU/Linux:           /etc/postgresql/13/main/pg_hba.conf

host    all             all             0.0.0.0/0               md5

## db install sequence

PostgreSQL Configuration
------------
1. `$ sudo -iu postgres`
2. `[postgres]$ initdb -D /var/lib/postgres/data`
3. `[postgres]$ createdb -U postgres accountsdb`
3. `[postgres]$ createdb -U postgres messagedb`
5.
```
[postgres]$ createuser --interactive
    Enter name of role to add: signal
    Shall the new role be a superuser? (y/n) y
```
6. `[postgres]$ psql`
7. `[postgres]# ALTER USER signal WITH PASSWORD 'YourPassword';`
8. `[postgres]# exit`
9. `[postgres]$ exit`
10. `$ systemctl start postgresql.service`
11. `$ systemctl enable postgresql.service`





# OLD documentation:

Signal-Server
=================

Documentation
-------------

Looking for protocol documentation? Check out the website!

https://signal.org/docs/

Cryptography Notice
------------

This distribution includes cryptographic software. The country in which you currently reside may have restrictions on the import, possession, use, and/or re-export to another country, of encryption software.
BEFORE using any encryption software, please check your country's laws, regulations and policies concerning the import, possession, or use, and re-export of encryption software, to see if this is permitted.
See <http://www.wassenaar.org/> for more information.

The U.S. Government Department of Commerce, Bureau of Industry and Security (BIS), has classified this software as Export Commodity Control Number (ECCN) 5D002.C.1, which includes information security software using or performing cryptographic functions with asymmetric algorithms.
The form and manner of this distribution makes it eligible for export under the License Exception ENC Technology Software Unrestricted (TSU) exception (see the BIS Export Administration Regulations, Section 740.13) for both object code and source code.

License
---------------------

Copyright 2013-2016 Open Whisper Systems

Licensed under the AGPLv3: https://www.gnu.org/licenses/agpl-3.0.html
