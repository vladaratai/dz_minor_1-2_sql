# dz_minor_1-2_sql
#Первое задание:
#Есть пользователи (id, имя), которые ставят друг другу лайки. Сделать таблицы для хранения всей этой информации.

%pip install psycopg2

import psycopg2
from psycopg2 import Error
from psycopg2.extras import NamedTupleCursor
def execute_query(query, fetch_result=False):
    try:
        connection = psycopg2.connect(
                        database="postgres", 
                        user='postgres',
                        password='hfnfneq2', 
                        host='localhost', 
                        port='5432'
                    )
        
        connection.autocommit = True
        cursor = connection.cursor(cursor_factory=NamedTupleCursor)
        cursor.execute(query)
        if fetch_result:
            return cursor.fetchall()
    except (Exception, Error) as error:
        print("Error while connecting to PostgreSQL", error)
    finally:
        if (connection):
            cursor.close()
            connection.close()


import psycopg2 as pg_driver

db = pg_driver.connect(
                        database="postgres", 
                        user='postgres',
                        password='hfnfneq2', 
                        host='localhost', 
                        port='5432'
                    )



def execute_queries(db, sql_commands):
    db.autocommit = True
    with db.cursor() as cursor:
        for sql_command in sql_commands:
            print(sql_command)
            cursor.execute(sql_command)


sql_commands = ["DROP TABLE IF EXISTS users;",
                "DROP TABLE IF EXISTS likes;",
                """CREATE TABLE users (
                         user_id    INT       NOT NULL,
                         name    TEXT       NOT NULL,
                         created    TIMESTAMP NOT NULL
                );
                """,
                """CREATE TABLE likes (
                         user_id    INT       NOT NULL,
                         created    TIMESTAMP NOT NULL,
                         user_id_give    INT       NOT NULL
                  );
                """]


execute_queries(db, sql_commands)


import psycopg2
from psycopg2 import Error
from psycopg2.extras import NamedTupleCursor

def execute_query(query, fetch_result=False):
    try:
        connection = pg_driver.connect(
                        database="postgres", 
                        user='postgres',
                        password='hfnfneq2', 
                        host='localhost', 
                        port='5432'
                    );
        
        connection.autocommit = True
        cursor = connection.cursor(cursor_factory=NamedTupleCursor)
        cursor.execute(query)
        if fetch_result:
            return cursor.fetchall()
    except (Exception, Error) as error:
        print("Error while connecting to PostgreSQL", error)
    finally:
        if (connection):
            cursor.close()
            connection.close()
row_count_hist = execute_query("select count(*) from users", fetch_result=True)
row_count_payment = execute_query("select count(*) from likes", fetch_result=True)

print(row_count_hist)
print(row_count_payment)

import psycopg2
from psycopg2 import Error
from psycopg2.extras import NamedTupleCursor

def execute_query(query, fetch_result=False):
    try:
        connection = pg_driver.connect(
                        database="postgres", 
                        user='postgres',
                        password='hfnfneq2', 
                        host='localhost', 
                        port='5432'
                    );
        
        connection.autocommit = True
        cursor = connection.cursor(cursor_factory=NamedTupleCursor)
        cursor.execute(query)
        if fetch_result:
            return cursor.fetchall()
    except (Exception, Error) as error:
        print("Error while connecting to PostgreSQL", error)
    finally:
        if (connection):
            cursor.close()
            connection.close()
row_count_hist = execute_query("select count(*) from users", fetch_result=True)
row_count_payment = execute_query("select count(*) from likes", fetch_result=True)

print(row_count_hist)
print(row_count_payment)

query = """ INSERT INTO users (user_id, created, name) 
            VALUES 
                 (1, to_timestamp('21-05-2021 12:35:11', 'dd-mm-yyyy hh24:mi:ss'), 'Petya'),
                 (2, to_timestamp('13-02-2013 13:35:11', 'dd-mm-yyyy hh24:mi:ss'), 'Dima'),
                 (3, to_timestamp('14-03-2002 05:35:11', 'dd-mm-yyyy hh24:mi:ss'), 'Kirill'),
                 (4, to_timestamp('15-05-2011 14:35:11', 'dd-mm-yyyy hh24:mi:ss'), 'Anna'),
                 (5, to_timestamp('28-08-2013 17:35:11', 'dd-mm-yyyy hh24:mi:ss'), 'Nick')
                 
            
        """

execute_query(query)

query = """ INSERT INTO likes (user_id, created, user_id_give) 
            VALUES 
                 (1, to_timestamp('21-05-2021 12:35:11', 'dd-mm-yyyy hh24:mi:ss'), 5),
                 (2, to_timestamp('13-02-2013 13:35:11', 'dd-mm-yyyy hh24:mi:ss'), 4),
                 (3, to_timestamp('14-03-2002 05:35:11', 'dd-mm-yyyy hh24:mi:ss'), 3),
                 (4, to_timestamp('15-05-2011 14:35:11', 'dd-mm-yyyy hh24:mi:ss'), 2),
                 (5, to_timestamp('28-08-2013 17:35:11', 'dd-mm-yyyy hh24:mi:ss'), 1)
                 
            
        """

execute_query(query)


     

query = """ INSERT INTO likes (user_id, created, user_id_give) 
            VALUES 
                 (1, to_timestamp('21-05-2021 12:35:11', 'dd-mm-yyyy hh24:mi:ss'), 4),
                 (1, to_timestamp('13-02-2013 13:35:11', 'dd-mm-yyyy hh24:mi:ss'), 5),
                 (2, to_timestamp('13-02-2013 13:35:11', 'dd-mm-yyyy hh24:mi:ss'), 5)
                 

                 
            
        """

execute_query(query)

query = """ INSERT INTO users (user_id, created, name) 
            VALUES 
                 (1, to_timestamp('21-05-2021 12:35:11', 'dd-mm-yyyy hh24:mi:ss'), 'Petya')
 
        """

execute_query(query)

query = """ INSERT INTO likes (user_id, created, user_id_give) 
            VALUES 
                 (1, to_timestamp('21-05-2021 12:35:11', 'dd-mm-yyyy hh24:mi:ss'), 4)
            
        """

execute_query(query)

#Далее запросы

query = '''
           select u.user_id,
                  u.name, 
                  count(l.user_id) as put_likes,
                  count(l.user_id_give) as given_likes 
            from users u
            left join likes l on u.user_id = l.user_id
            group by u.user_id, u.name
            
            '''
all_rows = execute_query(query, fetch_result=True)
for row, value in enumerate(all_rows):
    print(row, value)

query = '''
           select u.user_id,
                  u.name, 
                  count(l.user_id_give) as mutual_likes 
            from users u
            left join likes l on u.user_id = l.user_id
            where u.user_id = l.user_id_give
            group by u.user_id, u.name
            
            '''
all_rows = execute_query(query, fetch_result=True)
for row, value in enumerate(all_rows):
    print(row, value)

query = '''
           select u.user_id,
                  u.name, 
                  count(l.user_id_give) as given_likes
            from users u
            left join likes l on u.user_id = l.user_id_give
            group by u.user_id, u.name
            order by given_likes desc
            limit 5;
            
            '''
all_rows = execute_query(query, fetch_result=True)
for row, value in enumerate(all_rows):
    print(row, value)

#Второе задание
#В воображаемой социальной сети есть Пользователи (id, имя), Фото (id, название, автор) и Комментарии К Фото (id, текст, автор, к какому Фото относится). Необходимо добавить возможность для Пользователей ставить лайки другим Пользователям, Фото или Комментариям к Фото.

import psycopg2 as pg_driver

db = pg_driver.connect(
                        database="postgres", 
                        user='postgres',
                        password='hfnfneq2', 
                        host='localhost', 
                        port='5432'
                    )



def execute_queries(db, sql_commands):
    db.autocommit = True
    with db.cursor() as cursor:
        for sql_command in sql_commands:
            print(sql_command)
            cursor.execute(sql_command)


sql_commands = ["DROP TABLE IF EXISTS users;",
                "DROP TABLE IF EXISTS likes;",
                "DROP TABLE IF EXISTS photos;",
                "DROP TABLE IF EXISTS comments;",
                "DROP TABLE IF EXISTS types",
                
              
                """CREATE TABLE users (
                         user_id    INT       NOT NULL,
                         name       TEXT       NOT NULL, 
                         type_id       TEXT NOT NULL
                    );
                """
                """CREATE TABLE photos (
                         photo_id    INT       NOT NULL,
                         title    TEXT NOT NULL,
                         user_id    INT       NOT NULL,
                         type_id TEXT NOT NULL
                  );
                """
               
                """CREATE TABLE comments (
                         comment_id INT       NOT NULL,
                         user_id    INT       NOT NULL,
                         comment    TEXT NOT NULL,
                         photo_id INT NOT NULL,
                         type_id INT NOT NULL
                  );
                """
                
                """CREATE TABLE types (
                         type_id    INT       NOT NULL,
                         type    TEXT NOT NULL
                  );
                """
               
                """CREATE TABLE likes (
                         user_id    INT       NOT NULL,
                         type_id    TEXT NOT NULL,
                         to_user_id INT NOT NULL,
                         to_photo_id INT NOT NULL,
                         comment_id INT NOT NULL
                  );
                """]

                

execute_queries(db, sql_commands)

import psycopg2
from psycopg2 import Error
from psycopg2.extras import NamedTupleCursor

def execute_query(query, fetch_result=False):
    try:
        connection = psycopg2.connect(
                                database="postgres", 
                                user='postgres',
                                password='hfnfneq2', 
                                host='localhost', 
                                port='5432'
                     )
        connection.autocommit = True
        cursor = connection.cursor(cursor_factory=NamedTupleCursor)
        cursor.execute(query)
        if fetch_result:
            return cursor.fetchall()
    except (Exception, Error) as error:
        print("Error while connecting to PostgreSQL", error)
    finally:
        if (connection):
            cursor.close()
            connection.close()

import psycopg2
from psycopg2 import Error
from psycopg2.extras import NamedTupleCursor

def execute_query(query, fetch_result=False):
    try:
        connection = pg_driver.connect(
                        database="postgres", 
                        user='postgres',
                        password='hfnfneq2', 
                        host='localhost', 
                        port='5432'
                    );
        
        connection.autocommit = True
        cursor = connection.cursor(cursor_factory=NamedTupleCursor)
        cursor.execute(query)
        if fetch_result:
            return cursor.fetchall()
    except (Exception, Error) as error:
        print("Error while connecting to PostgreSQL", error)
    finally:
        if (connection):
            cursor.close()
            connection.close()
row_count_hist = execute_query("select count(*) from users", fetch_result=True)
row_count_payment = execute_query("select count(*) from likes", fetch_result=True)

print(row_count_hist)
print(row_count_payment)

query = """ INSERT INTO users (user_id, name, type_id) 
            VALUES 
                 (1, 'Katya', 101),
                 (2, 'Larisa', 101),
                 (3, 'Lena', 101),
                 (4, 'Nick', 101),
                 (5, 'Edward', 101), 
                 (6, 'Shawn', 101), 
                 (7, 'Liza', 101)
                 
            
        """
execute_query(query)

query = """ INSERT INTO photos (photo_id, title, user_id, type_id) 
            VALUES 
                 (21, 'IMG_1243.jpg', 1, 202),
                 (22, 'IMG_3421.jpg', 2, 202),
                 (23, 'IMG_1495.jpg', 3, 202),
                 (24, 'IMG_0923.jpg', 4, 202),
                 (25, 'IMG_5764.jpg', 5, 202)
                 
            
        """

execute_query(query)

query = """ INSERT INTO comments (comment_id, user_id, comment, photo_id, type_id) 
            VALUES 
                 (71, 1, 'Love it', 8, 303),
                 (72, 1, 'Where is it?', 9, 303),
                 (73, 7, 'OMG', 10, 303),
                 (74, 2, 'Beautiful!', 11, 303),
                 (75, 6, 'Love Norway!', 12, 303)
                 
            
        """
execute_query(query)

query = """ INSERT INTO types (type_id, type) 
            VALUES 
                 (101, 'user'),
                 (202, 'photo'),
                 (303, 'comment')
                 
            
        """
execute_query(query)

query = """ INSERT INTO likes (user_id, type_id, to_user_id, to_photo_id, comment_id) 
            VALUES 
                 (1, 202, 7, 21, 71),
                 (2, 101, 4, 25, 75),
                 (3, 303, 2, 24, 73),
                 (4, 101, 7, 21, 71),
                 (5, 202, 3, 21, 71)
                 
        """
execute_query(query)
