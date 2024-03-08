# 1、This is a table containing part of the original data
| assignment_id | student_id | due_date | professor | assignment_topic            | classroom | grade | relevant_reading     | professor_email      |
|---------------|------------|----------|-----------|-----------------------------|-----------|-------|----------------------|----------------------|
| 1             | 1          | 23.02.21 | Melvin    | Data normalization          | WWH 101   | 80    | Deumlich Chapter 3   | l.melvin@foo.edu    |
| 2             | 7          | 18.11.21 | Logston   | Single table queries       | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu   |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization          | WWH 101   | 75    | Deumlich Chapter 3   | l.melvin@foo.edu    |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas           | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu   |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87      | i.nevarez@foo.edu   |
| ...             | ...          | ...  | ...    | ...  | ...    | ...    | ...       | ...    |

# 2、The description of what makes this data set not compliant with 4NF
From the given data,It does not conform to 4NF due to multivalued dependencies. Here are the details: 

1）Professor can teach multiple sections of the same course.

2）Different sections of the same course may meet in different classrooms, even if the professor is the same.

3）Professors give assignments with due dates specific to the section of the course.

4）Professors give readings that are relevant and helpful to a given assignment.

Based on these dependencies, we can split the data into the following tables:
**Professors Table:**

**professor_id (Primary Key)**

| professor_id | professor_name | professor_email   |
|--------------|----------------|-------------------|
| 1            | Melvin         | l.melvin@foo.edu  |
| 2            | Logston        | e.logston@foo.edu |
| 3            | Nevarez        | i.nevarez@foo.edu |

**courses Table:**

**course_id (Primary Key)**
| course_id | course_name          |
|-----------|----------------------|
| 1         | Data normalization  |
| 2         | Single table queries |
| 3         | Python and pandas    |
| 4         | Spreadsheet aggregate functions |

**sections Table:**

**section_id (Primary Key)**

**course_id (Foreign Key referencing Courses Table)**

**professor_id (Foreign Key referencing Professors Table)**
| section_id | course_id | professor_id | classroom |
|------------|-----------|--------------|-----------|
| 1          | 1         | 1            | WWH 101   |
| 2          | 2         | 2            | 60FA 314  |
| 3          | 3         | 2            | 60FA 314  |
| 4          | 4         | 3            | WWH 201   |

**Assignments Table**

**assignment_id (Primary Key)**

**section_id (Foreign Key referencing Sections Table)**
| assignment_id | section_id | assignment_topic             | due_date |
|---------------|------------|------------------------------|----------|
| 1             | 1          | Data normalization           | 23.02.21 |
| 2             | 2          | Single table queries         | 18.11.21 |
| 3             | 1          | Data normalization           | 23.02.21 |
| 4             | 3          | Python and pandas            | 05.05.21 |
| 5             | 4          | Spreadsheet aggregate functions | 04.07.21 |

**Readings Table**

**reading_id (Primary Key)**

**assignment_id (Foreign Key referencing Assignments Table)**
| reading_id | assignment_id | relevant_reading |
|------------|---------------|------------------|
| 1          | 1             | Chapter 3        |
| 2          | 2             | Chapter 11       |
| 3          | 3             | Chapter 3        |
| 4          | 4             | Chapter 14       |
| 5          | 5             | Page 87          |

**Students Grades Table:**
| student_id | assignment_id | grade |
|------------|---------------|-------|
| 1          | 1             | 80    |
| 7          | 2             | 25    |
| 4          | 3             | 75    |
| 2          | 4             | 92    |
| 2          | 5             | 65    |
