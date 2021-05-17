# To-Do list

1. Finish tutorial
2. write EMA app based on this tutorial

# Testing 

## To spin up docker container after finishing the tutorial in terminal

```
docker-compose -f docker-compose.prod.yml down -v
docker-compose -f docker-compose.prod.yml up -d --build
docker-compose -f docker-compose.prod.yml exec web python manage.py create_db
docker-compose -f docker-compose.prod.yml exec web python manage.py seed_db
```

## visit localhost:5000 to find hello world endpoint

## To test the docker container database user table was created

```
$ docker-compose exec db psql --username=hello_flask --dbname=hello_flask_prod

psql (13.2)
Type "help" for help.

hello_flask_prod=# \l
                                        List of databases
      Name       |    Owner    | Encoding |  Collate   |   Ctype    |      Access privileges
-----------------+-------------+----------+------------+------------+-----------------------------
 hello_flask_prod | hello_flask | UTF8     | en_US.utf8 | en_US.utf8 |
 postgres        | hello_flask | UTF8     | en_US.utf8 | en_US.utf8 |
 template0       | hello_flask | UTF8     | en_US.utf8 | en_US.utf8 | =c/hello_flask             +
                 |             |          |            |            | hello_flask=CTc/hello_flask
 template1       | hello_flask | UTF8     | en_US.utf8 | en_US.utf8 | =c/hello_flask             +
                 |             |          |            |            | hello_flask=CTc/hello_flask
(4 rows)

hello_flask_prod=# \c hello_flask_prod
You are now connected to database "hello_flask_prod" as user "hello_flask".

hello_flask_prod=# \dt
          List of relations
 Schema | Name  | Type  |    Owner
--------+-------+-------+-------------
 public | users | table | hello_flask
(1 row)

hello_flask_prod=# \q
```

## To check that persistent volume was created 
```
$ docker volume inspect flask-on-docker_postgres_data

[
    {
        "CreatedAt": "2021-05-05T18:30:38Z",
        "Driver": "local",
        "Labels": {
            "com.docker.compose.project": "flask-on-docker",
            "com.docker.compose.version": "1.29.0",
            "com.docker.compose.volume": "postgres_data"
        },
        "Mountpoint": "/var/lib/docker/volumes/flask-on-docker_postgres_data/_data",
        "Name": "flask-on-docker_postgres_data",
        "Options": null,
        "Scope": "local"
    }
]
```

### To check to see if database was properly seeded
```
$ docker-compose exec db psql --username=hello_flask --dbname=hello_flask_prod

psql (13.2)
Type "help" for help.

hello_flask_prod=# \c hello_flask_prod
You are now connected to database "hello_flask_prod" as user "hello_flask".

hello_flask_prod=# select * from users;
 id |        email        | active
----+---------------------+--------
  1 | michael@mherman.org | t
(1 row)

hello_flask_prod=# \q
```

