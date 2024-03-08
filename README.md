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

## **Professors Table:**

**professor_id (Primary Key)**

| professor_id | professor_name | professor_email   |
|--------------|----------------|-------------------|
| 1            | Melvin         | l.melvin@foo.edu  |
| 2            | Logston        | e.logston@foo.edu |
| 3            | Nevarez        | i.nevarez@foo.edu |

_This table holds information about professors, including their unique identifier (professor_id), name (professor_name), and email (professor_email)._

_By separating professors into their own table, we eliminate the potential for repeating groups or multi-valued dependencies related to professor attributes._

## **courses Table:**

**course_id (Primary Key)**
| course_id | course_name          |
|-----------|----------------------|
| 1         | Data normalization  |
| 2         | Single table queries |
| 3         | Python and pandas    |
| 4         | Spreadsheet aggregate functions |

_This table stores information about the courses offered at the university, including their unique identifier (course_id) and name (course_name)._

_Separating courses into their own table ensures that course-related attributes are stored independently, avoiding duplication and ensuring data integrity._

## **sections Table:**

**section_id (Primary Key)**

**course_id (Foreign Key referencing Courses Table)**

**professor_id (Foreign Key referencing Professors Table)**
| section_id | course_id | professor_id | classroom |
|------------|-----------|--------------|-----------|
| 1          | 1         | 1            | WWH 101   |
| 2          | 2         | 2            | 60FA 314  |
| 3          | 3         | 2            | 60FA 314  |
| 4          | 4         | 3            | WWH 201   |

_This table represents sections of courses, including attributes such as section_id, the course_id they belong to (foreign key referencing Courses Table), the professor teaching the section (foreign key referencing Professors Table), and the classroom where the section meets._

_By creating a separate table for sections, we capture the relationship between courses, professors, and classrooms, allowing for flexibility in assigning professors to multiple sections of the same course._

## **Assignments Table**

**assignment_id (Primary Key)**

**section_id (Foreign Key referencing Sections Table)**
| assignment_id | section_id | assignment_topic             | due_date |
|---------------|------------|------------------------------|----------|
| 1             | 1          | Data normalization           | 23.02.21 |
| 2             | 2          | Single table queries         | 18.11.21 |
| 3             | 1          | Data normalization           | 23.02.21 |
| 4             | 3          | Python and pandas            | 05.05.21 |
| 5             | 4          | Spreadsheet aggregate functions | 04.07.21 |

_This table contains information about assignments given in each section, with attributes like assignment_id, the section_id they belong to (foreign key referencing Sections Table), the assignment topic, and the due date_

_Separating assignments into their own table allows us to track assignments specific to each section, ensuring that assignment-related data is stored independently of other entities._

### **Readings Table**

**reading_id (Primary Key)**

**assignment_id (Foreign Key referencing Assignments Table)**
| reading_id | assignment_id | relevant_reading |
|------------|---------------|------------------|
| 1          | 1             | Chapter 3        |
| 2          | 2             | Chapter 11       |
| 3          | 3             | Chapter 3        |
| 4          | 4             | Chapter 14       |
| 5          | 5             | Page 87          |

_This table stores readings relevant to each assignment, with attributes including reading_id, the assignment_id they belong to (foreign key referencing Assignments Table), and the relevant reading material._

_By creating a separate table for readings, we remove the multi-valued dependency between assignments and relevant readings, ensuring that each reading is associated with a single assignment._

### **Students Grades Table:**

**（student_id, assignment_id）(Primary Key)**

**assignment_id(Foreign Key referencing Assignments Table)**
| student_id | assignment_id | grade |
|------------|---------------|-------|
| 1          | 1             | 80    |
| 7          | 2             | 25    |
| 4          | 3             | 75    |
| 2          | 4             | 92    |
| 2          | 5             | 65    |

_This table maintains information about students' grades for each assignment, with attributes including student_id, assignment_id, and grade._

_This table captures the relationship between students and assignments, allowing for the storage of grade-related data independently of other entities._

## 3、The ER diagram(s) of the 4NF-compliant version of the data set
[Here is the E-R diagram](/images/E—R.svg)