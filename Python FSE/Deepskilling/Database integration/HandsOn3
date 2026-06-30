HANDS-ON 3 – TASK 1: Subqueries

-- 35. Students enrolled in more courses than average

      SELECT s.student_id,
             s.first_name,
             s.last_name
      FROM students s
      JOIN enrollments e
      ON s.student_id = e.student_id
      GROUP BY s.student_id, s.first_name, s.last_name
      HAVING COUNT(e.course_id) >
      (
          SELECT AVG(course_count)
          FROM
          (
              SELECT COUNT(course_id) AS course_count
              FROM enrollments
              GROUP BY student_id
          ) avg_table
      );

-- 36. Courses where all students received grade 'A'

      SELECT c.course_name
      FROM courses c
      WHERE NOT EXISTS
      (
          SELECT *
          FROM enrollments e
          WHERE e.course_id = c.course_id
          AND e.grade <> 'A'
      );

-- 37. Highest paid professor in each department
      SELECT p.prof_name,
             p.salary,
             p.department_id
      FROM professors p
      WHERE p.salary =
      (
          SELECT MAX(salary)
          FROM professors p2
          WHERE p2.department_id = p.department_id
      );

-- 38. Departments with average salary above 85000
      SELECT dept_name,
             avg_salary
      FROM
      (
          SELECT d.dept_name,
                 AVG(p.salary) AS avg_salary
          FROM departments d
          JOIN professors p
          ON d.department_id = p.department_id
          GROUP BY d.dept_name
      ) dept_avg
      WHERE avg_salary > 85000;

HANDS-ON 3 – TASK 2: Views
  
-- 39. Create Student Enrollment Summary View

      CREATE VIEW vw_student_enrollment_summary AS
      SELECT
          CONCAT(s.first_name,' ',s.last_name) AS student_name,
          d.dept_name,
          COUNT(e.course_id) AS total_courses,
          ROUND(
              AVG(
                  CASE
                      WHEN e.grade='A' THEN 4
                      WHEN e.grade='B' THEN 3
                      WHEN e.grade='C' THEN 2
                      WHEN e.grade='D' THEN 1
                      WHEN e.grade='F' THEN 0
                  END
              ),2
          ) AS GPA
      FROM students s
      LEFT JOIN departments d
      ON s.department_id=d.department_id
      LEFT JOIN enrollments e
      ON s.student_id=e.student_id
      GROUP BY s.student_id,d.dept_name;

-- 40. Create Course Statistics View
        CREATE VIEW vw_course_stats AS
        SELECT
            c.course_name,
            c.course_code,
            COUNT(e.enrollment_id) AS total_enrollments,
            ROUND(
                AVG(
                    CASE
                        WHEN e.grade='A' THEN 4
                        WHEN e.grade='B' THEN 3
                        WHEN e.grade='C' THEN 2
                        WHEN e.grade='D' THEN 1
                        WHEN e.grade='F' THEN 0
                    END
                ),2
            ) AS avg_gpa
        FROM courses c
        LEFT JOIN enrollments e
        ON c.course_id=e.course_id
        GROUP BY c.course_id,c.course_name,c.course_code;

-- 41. Students with GPA above 3.0
        SELECT *
        FROM vw_student_enrollment_summary
        WHERE GPA > 3.0;

-- 42. Attempt Update through View
      UPDATE vw_student_enrollment_summary
      SET GPA = 4
      WHERE student_name = 'Arjun Mehta';

-- 43. Drop Views
      DROP VIEW vw_student_enrollment_summary;
      DROP VIEW vw_course_stats;

-- Recreate View with CHECK OPTION
      CREATE VIEW vw_student_enrollment_summary AS
      SELECT *
      FROM students
      WHERE enrollment_year >= 2022
      WITH CHECK OPTION;

HANDS-ON 3 – TASK 3: Stored Procedures & Transactions

-- 44. Procedure to Enroll Student (MySQL)

      DELIMITER $$
      
      CREATE PROCEDURE sp_enroll_student(
          IN p_student_id INT,
          IN p_course_id INT,
          IN p_enrollment_date DATE
      )
      BEGIN
          IF EXISTS (
              SELECT *
              FROM enrollments
              WHERE student_id = p_student_id
              AND course_id = p_course_id
          ) THEN
              SIGNAL SQLSTATE '45000'
              SET MESSAGE_TEXT='Student already enrolled';
          ELSE
              INSERT INTO enrollments(student_id,course_id,enrollment_date)
              VALUES(p_student_id,p_course_id,p_enrollment_date);
          END IF;
      END $$
      
      DELIMITER ;

-- Call Procedure
        CALL sp_enroll_student(1,3,'2024-01-01');
        

-- 45. Create Transfer Log Table
        CREATE TABLE department_transfer_log(
            log_id INT AUTO_INCREMENT PRIMARY KEY,
            student_id INT,
            old_department INT,
            new_department INT,
            transfer_date DATE
        );

-- Transfer Student Transaction
        START TRANSACTION;
        
        UPDATE students
        SET department_id = 2
        WHERE student_id = 1;
        
        INSERT INTO department_transfer_log
        (student_id,old_department,new_department,transfer_date)
        VALUES(1,1,2,CURDATE());
        
        COMMIT;


-- 46. Rollback Example
        START TRANSACTION;
        
        UPDATE students
        SET department_id = 3
        WHERE student_id = 2;
        
        INSERT INTO department_transfer_log
        (student_id,old_department,new_department,transfer_date)
        VALUES(2,1,99,CURDATE());
        
        ROLLBACK;


-- 47. SAVEPOINT Example
        START TRANSACTION;
        
        INSERT INTO enrollments(student_id,course_id,enrollment_date,grade)
        VALUES(2,2,CURDATE(),'A');
        
        SAVEPOINT sp1;
        
        INSERT INTO enrollments(student_id,course_id,enrollment_date,grade)
        VALUES(999,999,CURDATE(),'A');
        
        ROLLBACK TO sp1;
        
        COMMIT;
