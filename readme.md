### Fix postfix admin
```
create index username on mail.admin (username);
create index username on mail.mailbox (username);
create index address on mail.alias (address);
create index domain on mail.domain (domain);
``` 

### Config for roundcube
```
CREATE DATABASE roundcube DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE USER roundcube@'%' IDENTIFIED BY 'password'; 
GRANT ALL PRIVILEGES ON roundcube.* TO roundcube@'%';
FLUSH PRIVILEGES;
``` 
