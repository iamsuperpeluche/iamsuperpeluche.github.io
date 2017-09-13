---
layout: post
title: PostgreSQL Roles and Login Privileges
---

I'm currently working on a project with another developer and because the
database is hosted on my server I needed to create a new role with a new
password and most importantly restrict their access to my other databases (the
ones I use for my personal projects). I trust them to not do anything malicious
because they're friends but what if you're involved in a project where you don't
know all the collaborators that well ? Better safe than sorry.

In this short post, I will detail how I managed to solve both of the above mentioned
problems.

## 1. Give a user a password

To Add or Change a Postgresql user's password, use the following statement:

```sql
ALTER ROLE rolename WITH ENCRYPTED PASSWORD 'new_password_in_single_quotes';
```

Because of the way psql handles login with roles, you want to specify the
database name as well as the host and port (optional)

```bash
psql -U rolename -d database_name -h localhost_or_ip -p port -W

# The -W flag will prompt the user to enter their password
```

## 2. Setup Login Privileges

To filter which users can login to a given database, run the following SQL
commands:

```sql
REVOKE CONNECT ON DATABASE db_name FROM PUBLIC:
```
This will revoke login for all users who inherit the `PUBLIC` role. (Which, to
my knowledge, is pretty much everyone)

To allow login for a given user, execute this statement:

```sql
GRANT CONNECT ON DATABASE db_name TO role_name;
```

If you want this setting to apply to all future databases:

```sql
REVOKE CONNECT ON DATABASE template1 FROM PUBLIC:
```
_Note: template1 is the default template used for creating new databases_

## Conclusion

I hope this was helpful to you in some way. Now you know how to change (or add)
a PostgreSQL user password and how to limit a Postgresql user's access to your
databases using privileges.
If I missed something or if you have any questions / remarks etc, give me a
shout on twitter, I'm [@zabanaa](https://twitter.com/zabanaa_)