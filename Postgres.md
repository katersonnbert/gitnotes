Postgres
========


# Install postgres (Linux)

	apt-get install postgresql

- Postgres will be installed to `/etc/postgresql/[version]/`
- It sets up a superuser "postgres"
- It already sets up a couple of demons that are listening to requests
- Check where the demons are listening:
	    `netstat -l`    ... shows all listening demons by their name
    or
	    `netstat -lt`   ... shows all listening demons by their Local address

- Postgres listens by default on port 5432 e.g. 127.0.0.1:5432
- To check how one can connect to the database, open `/etc/postgresql/[version]/main/pg_hba.conf`


# Create user and database

- First open the postgres terminal using the superuser "postgres"

        sudo -u postgres psql
- Now we are connected to the postgres terminal. To look up which commands are available:

	    \?
- Check which databases are currently available:

        \l ... should print the main "postgres" db and two templates
- Check which roles (~users) are currently available:

	    \d  ... should return no relations
- First we need to connect to the main database "postgres" ... This database contains all information about installed roles and databases. Here we also add new users and databases

	    \c postgres
- Now we are connected to database "postgres" with superuser "postgres"
- Now we create a normal user for our first database

	    CREATE ROLE play WITH login password 'play'; ... This creates user "play" and this user can login to databases using password "play".
- If we check the available roles:

	    \du   ... we will see the users "postgres" and "play" with their attributes.
- Now we create our first database:

	    CREATE database playground OWNER play;    ... this does exactly what it looks like.
- If we check our available databases with `\l` we will see, that "playground" has been added with owner "play"
- We can log out of the postgres terminal by using `\q`

- We can log into our new database with our new user:

	    psql -U play -h localhost playground

- Here we can create a couple of tables e.g. "swingset" with columns "plank" and "tire".
- Now we can list, which tables are available using `\d`
- We can also display the table setup using `\d swingset`
