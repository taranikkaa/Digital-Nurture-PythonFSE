HANDS-ON 2 – TASK 1: Insert, Update and Delete Data

-- 15. Insert Sample Data
-- Use the INSERT statements provided in the Common Schema section.

-- 16. Insert Two Additional Students
      INSERT INTO students
      (first_name, last_name, email, date_of_birth, department_id, enrollment_year)
      VALUES
      ('Rahul', 'Sharma', 'rahul.sharma@college.edu', '2004-06-15', 1, 2023),
      ('Neha', 'Gupta', 'neha.gupta@college.edu', '2003-12-10', 2, 2022);

-- 17. Update Grade
      UPDATE enrollments
      SET grade = 'B'
      WHERE student_id = 5
      AND course_id = 1;

-- 18. Preview and Delete NULL Grades
      SELECT *
      FROM enrollments
      WHERE grade IS NULL;
      
      DELETE FROM enrollments
      WHERE grade IS NULL;

-- 19. Verify Row Counts
      SELECT COUNT(*) AS total_students
      FROM students;
      SELECT COUNT(*) AS total_enrollments
      FROM enrollments;

HANDS-ON 2 – TASK 2
   Single Table Queries
  
-- 20. Students Enrolled in 2022
    SELECT *
    FROM students
    WHERE enrollment_year = 2022
    ORDER BY last_name ASC;

-- 21. Courses with More Than 3 Credits
    SELECT *
    FROM courses
    WHERE credits > 3
    ORDER BY credits DESC;

-- 22. Professors with Salary Between 80000 and 95000
    SELECT *
    FROM professors
    WHERE salary BETWEEN 80000 AND 95000;

-- 23. Students with Email Ending in @college.edu
    SELECT *
    FROM students
    WHERE email LIKE '%@college.edu';

-- 24. Count Students per Enrollment Year
    SELECT enrollment_year,
           COUNT(*) AS total_students
    FROM students
    GROUP BY enrollment_year;


HANDS-ON 2 – TASK 3
   Multi-Table Joins
  

-- 25. Student Name with Department Name
    SELECT CONCAT(s.first_name,' ',s.last_name) AS student_name,
           d.dept_name
    FROM students s
    JOIN departments d
    ON s.department_id = d.department_id;

-- 26. Enrollment with Student Name and Course Name
    SELECT CONCAT(s.first_name,' ',s.last_name) AS student_name,
           c.course_name,
           e.grade
    FROM enrollments e
    JOIN students s
    ON e.student_id = s.student_id
    JOIN courses c
    ON e.course_id = c.course_id;

-- 27. Students Not Enrolled in Any Course
    SELECT s.student_id,
           s.first_name,
           s.last_name
    FROM students s
    LEFT JOIN enrollments e
    ON s.student_id = e.student_id
    WHERE e.student_id IS NULL;

-- 28. Courses with Number of Students Enrolled
    SELECT c.course_name,
           COUNT(e.student_id) AS enrolled_students
    FROM courses c
    LEFT JOIN enrollments e
    ON c.course_id = e.course_id
    GROUP BY c.course_id, c.course_name;

-- 29. Departments with Professors and Salaries
    SELECT d.dept_name,
           p.prof_name,
           p.salary
    FROM departments d
    LEFT JOIN professors p
    ON d.department_id = p.department_id;

HANDS-ON 2 – TASK 4
   Aggregations and Grouping
   

-- 30. Total Enrollments per Course
    SELECT c.course_name,
           COUNT(e.enrollment_id) AS enrollment_count
    FROM courses c
    LEFT JOIN enrollments e
    ON c.course_id = e.course_id
    GROUP BY c.course_id, c.course_name;

-- 31. Average Salary of Professors per Department
    SELECT d.dept_name,
           ROUND(AVG(p.salary),2) AS average_salary
    FROM departments d
    LEFT JOIN professors p
    ON d.department_id = p.department_id
    GROUP BY d.department_id, d.dept_name;

-- 32. Departments with Budget Greater Than 600000
    SELECT dept_name,
           budget
    FROM departments
    WHERE budget > 600000;

-- 33. Grade Distribution for CS101
    SELECT e.grade,
           COUNT(*) AS grade_count
    FROM enrollments e
    JOIN courses c
    ON e.course_id = c.course_id
    WHERE c.course_code = 'CS101'
    GROUP BY e.grade;

-- 34. Departments Having More Than 2 Students
    SELECT d.dept_name,
           COUNT(s.student_id) AS total_students
    FROM departments d
    JOIN students s
    ON d.department_id = s.department_id
    GROUP BY d.department_id, d.dept_name
    HAVING COUNT(s.student_id) > 2;
