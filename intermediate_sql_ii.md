## Intermediate SQL Workshop


* Run `psql`
* You may need to run `CREATE DATABASE intermediate_sql_ii;`
* Close the current connection and connect to the DB we just created: `\c intermediate_sql_ii;`
* To exit this shell, you can run `CTRL` `d`

#### Create tables

Let's create some tables in the database.
```
CREATE TABLE students(id SERIAL, name TEXT);
CREATE TABLE classes(id SERIAL, name TEXT, teacher_id INT);
CREATE TABLE teachers(id SERIAL, name TEXT, room_number INT);
CREATE TABLE enrollments(id SERIAL, student_id INT, class_id INT, grade INT);
```

#### Add data

Let's insert some students.

```
INSERT INTO students (name)
VALUES ('Penelope'),
       ('Peter'),
       ('Pepe'),
       ('Parth'),
       ('Priscilla'),
       ('Pablo'),
       ('Puja'),
       ('Patricia'),
       ('Piper'),
       ('Paula'),
       ('Pamela'),
       ('Paige'),
       ('Peggy'),
       ('Pedro'),
       ('Phoebe'),
       ('Pajak'),
       ('Parker'),
       ('Priyal'),
       ('Paxton'),
       ('Patrick');
```

Let's add some teachers.

```
INSERT INTO teachers (name, room_number)
VALUES ('Phillips', 456),
       ('Vandergrift', 120),
       ('Mauch', 101),
       ('Patel', 320),
       ('Marquez', 560),
       ('Boykin', 200),
       ('Phlop', 333),
       ('Pendergrass', 222),
       ('Palomo', 323),
       ('Altshuler', 543),
       ('Aleman', 187),
       ('Ashley', 432),
       ('Bonacci', 399),
       ('Brazukas', 287),
       ('Brockington', 299),
       ('Brizuela', 376),
       ('Burkhart', 199),
       ('Choi', 463),
       ('Shah', 354),
       ('Dimaggio', 251);
```

Let's add some classes.

```
INSERT INTO classes (name, teacher_id)
VALUES ('Cooking Pasta', 1),
       ('Yoga', 1),
       ('How to Guitar', 2),
       ('Gym', 3),
       ('Football', 4),
       ('Calculus', 5),
       ('Fruit', 6),
       ('Social Studies', 7),
       ('English', 8),
       ('Programming', 9),
       ('Singing', 10),
       ('Fashion', 11);
```

Lastly, let's add some enrollments!

```
INSERT INTO enrollments (student_id, class_id, grade)
VALUES (1, 1, 60),
       (2, 2, 70),
       (2, 4, 100),
       (3, 2, 74),
       (4, 3, 82),
       (5, 3, 45),
       (5, 4, 50),
       (7, 11, 62),
       (7, 10, 76),
       (7, 9, 81),
       (7, 8, 91),
       (8, 8, 84),
       (9, 8, 88),
       (9, 7, 83),
       (10, 7, 93),
       (10, 5, 95),
       (11, 5, 95),
       (11, 11, 80),
       (11, 6, 95),
       (11, 1, 94),
       (11, 2, 60),
       (12, 6, 55),
       (13, 7, 97),
       (14, 10, 86),
       (15, 9, 77),
       (15, 4, 93),
       (15, 1, 73),
       (16, 2, 79),
       (16, 6, 73),
       (17, 7, 86),
       (17, 8, 91),
       (17, 9, 93),
       (18, 10, 94),
       (19, 4, 84),
       (20, 1, 85),
       (20, 11, 89),
       (20, 3, 98);
```
* list all tables to see all that's been created:`\dt`
  #=> List of relations
  Schema |    Name     | Type  |    Owner
  --------+-------------+-------+--------------
  public | classes     | table | autumnmartin
  public | enrollments | table | autumnmartin
  public | students    | table | autumnmartin
  public | teachers    | table | autumnmartin
### Practice!!

* List all the students and their classes
  ```sql
  SELECT students.name AS student_name, classes.name AS class_name
    FROM students
    INNER JOIN enrollments
    ON students.id = enrollments.student_id
    INNER JOIN classes
    ON enrollments.class_id = classes.id;
```
* List all the students and their classes and rename the columns to "student" and "class"
  ```sql
  SELECT students.name AS student, classes.name AS class
    FROM students
    INNER JOIN enrollments
    ON students.id = enrollments.student_id
    INNER JOIN classes
    ON enrollments.class_id = classes.id;
  ```
* List all the students and their average grade
  ```sql
  SELECT students.name AS student, avg(enrollments.grade) AS average_grade
    FROM students
    INNER JOIN enrollments
    ON students.id = enrollments.student_id
    GROUP BY students.name;

      student  |    average_grade
    -----------+---------------------
     Penelope  | 60.0000000000000000
     Pedro     | 86.0000000000000000
     Piper     | 85.5000000000000000
     Patrick   | 90.6666666666666667
     Paula     | 94.0000000000000000
     Peter     | 85.0000000000000000
     Paige     | 55.0000000000000000
     Paxton    | 84.0000000000000000
     Pepe      | 74.0000000000000000
     Pamela    | 84.8000000000000000
     Parker    | 90.0000000000000000
     Patricia  | 84.0000000000000000
     Priscilla | 47.5000000000000000
     Priyal    | 94.0000000000000000
     Parth     | 82.0000000000000000
     Pajak     | 76.0000000000000000
     Puja      | 77.5000000000000000
     Peggy     | 97.0000000000000000
     Phoebe    | 81.0000000000000000
     (19 rows)
  ```
* List all the students and a count of how many classes they are currently enrolled in
  ```sql
  SELECT students.name AS student, count(enrollments.class_id) AS number_of_classes
    FROM students
    INNER JOIN enrollments
    ON students.id = enrollments.student_id
    GROUP BY students.name;

      student  | number_of_classes
    -----------+-------------------
     Penelope  |                 1
     Pedro     |                 1
     Piper     |                 2
     Patrick   |                 3
     Paula     |                 2
     Peter     |                 2
     Paige     |                 1
     Paxton    |                 1
     Pepe      |                 1
     Pamela    |                 5
     Parker    |                 3
     Patricia  |                 1
     Priscilla |                 2
     Priyal    |                 1
     Parth     |                 1
     Pajak     |                 2
     Puja      |                 4
     Peggy     |                 1
     Phoebe    |                 3
    (19 rows)
  ```

* List all the students and their class count IF they are in more than 2 classes
  ```sql
  SELECT students.name AS student, count(enrollments.class_id) AS number_of_classes
    FROM students
    INNER JOIN enrollments
    ON students.id = enrollments.student_id
    GROUP BY students.name
    HAVING count(enrollments.class_id) > 2;

    student  | number_of_classes
    ---------+-------------------
     Patrick |                 3
     Pamela  |                 5
     Parker  |                 3
     Puja    |                 4
     Phoebe  |                 3
    (5 rows)
  ```
* List all the teachers for each student
  * students (id) -> enrollments (student_id, class_id) -> classes (id, teacher_id) -> teachers (id)
```sql
SELECT students.name AS student, teachers.name AS teachers
  FROM students
  INNER JOIN enrollments
  ON students.id = enrollments.student_id
  INNER JOIN classes
  ON enrollments.class_id = classes.id
  INNER JOIN teachers
  ON classes.teacher_id = teachers.id
  GROUP BY students.name, teachers.name
  ORDER BY students.name;

    student  |  teachers

  -----------+-------------
   Paige     | Marguez
   Pajak     | Marguez
   Pajak     | Phillips
   Pamela    | Altshuler
   Pamela    | Phillips
   Pamela    | Patel
   ...
   Priyal    | Palomo
   Puja      | Phlop
   Puja      | Pendergrass
   Puja      | Altshuler
   Puja      | Palomo
    (36 rows)
```
* List all the teachers for each student grouped by each student
```sql
SELECT students.name AS student, string_agg(teachers.name, '|') AS teachers
  FROM students
  INNER JOIN enrollments
  ON students.id = enrollments.student_id
  INNER JOIN classes
  ON enrollments.class_id = classes.id
  INNER JOIN teachers
  ON classes.teacher_id = teachers.id
  GROUP BY students.name
  ORDER BY students.name ASC;

  student   |                 teachers
  -----------+-------------------------------------------
  Paige     | Marguez
  Pajak     | Phillips|Marguez
  Pamela    | Phillips|Phillips|Patel|Marguez|Altshuler
  Parker    | Boykin|Phlop|Pendergrass
  Parth     | Vandergrift
  Patricia  | Phlop
  Patrick   | Phillips|Vandergrift|Altshuler
  Paula     | Patel|Boykin
  Paxton    | Mauch
  Pedro     | Palomo
  Peggy     | Boykin
  Penelope  | Phillips
  Pepe      | Phillips
  Peter     | Phillips|Mauch
  Phoebe    | Phillips|Mauch|Pendergrass
  Piper     | Boykin|Phlop
  Priscilla | Vandergrift|Mauch
  Priyal    | Palomo
  Puja      | Phlop|Pendergrass|Palomo|Altshuler
  (19 rows)
  ```
* Find the average grade for a each class
```sql
SELECT classes.name AS class, avg(enrollments.grade) AS average_grade
  FROM classes
  INNER JOIN enrollments
  ON classes.id = enrollments.class_id
  GROUP BY classes.name;

  class          |    average_grade
  ----------------+---------------------
  English        | 83.6666666666666667
  Yoga           | 70.7500000000000000
  Programming    | 85.3333333333333333
  How to Guitar  | 75.0000000000000000
  Social Studies | 88.5000000000000000
  Calculus       | 74.3333333333333333
  Singing        | 77.0000000000000000
  Football       | 95.0000000000000000
  Cooking Pasta  | 78.0000000000000000
  Fruit          | 89.7500000000000000
  Gym            | 81.7500000000000000
  (11 rows)
```
* List students' name and their grade IF their grade is lower than the average.
```sql

  SELECT students.name AS student, enrollments.grade AS grade, classes.name AS class
    FROM students
    INNER JOIN enrollments
    ON students.id = enrollments.student_id
    INNER JOIN classes
    ON enrollments.class_id = classes.id
    WHERE enrollments.grade <
      (SELECT avg(enrollments.grade)
        FROM enrollments)
    ORDER BY classes.name;

      name    | grade |     name
   -----------+-------+---------------
    Pajak     |    73 | Calculus
    Paige     |    55 | Calculus
    Penelope  |    60 | Cooking Pasta
    Phoebe    |    73 | Cooking Pasta
    Phoebe    |    77 | English
    Puja      |    81 | English
    Priscilla |    50 | Gym
    Priscilla |    45 | How to Guitar
    Puja      |    76 | Programming
    Pamela    |    80 | Singing
    Puja      |    62 | Singing
    Pamela    |    60 | Yoga
    Pepe      |    74 | Yoga
    Peter     |    70 | Yoga
    Pajak     |    79 | Yoga
   (15 rows)
```
