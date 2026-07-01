Task 1: Define Models and Run Migrations

Step 1: Open courses/models.py

Replace everything with:

from django.db import models

class Department(models.Model):
    name = models.CharField(max_length=100)
    head_of_dept = models.CharField(max_length=100)
    budget = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return self.name


class Course(models.Model):
    name = models.CharField(max_length=100)
    code = models.CharField(max_length=20, unique=True)
    credits = models.IntegerField()
    department = models.ForeignKey(
        Department,
        on_delete=models.CASCADE
    )

    def __str__(self):
        return self.name


class Student(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    email = models.EmailField(unique=True)
    department = models.ForeignKey(
        Department,
        on_delete=models.CASCADE
    )
    enrollment_year = models.IntegerField()

    def __str__(self):
        return f"{self.first_name} {self.last_name}"


class Enrollment(models.Model):
    student = models.ForeignKey(
        Student,
        on_delete=models.CASCADE
    )
    course = models.ForeignKey(
        Course,
        on_delete=models.CASCADE
    )
    enrollment_date = models.DateField()
    grade = models.CharField(
        max_length=5,
        null=True,
        blank=True
    )

    class Meta:
        unique_together = [['student', 'course']]

    def __str__(self):
        return f"{self.student} - {self.course}"

Step 2: Create Migration
python manage.py makemigrations

Expected:

Migrations for 'courses':
  courses/migrations/0001_initial.py

  Step 3: Apply Migration

python manage.py migrate

Expected:

Applying courses.0001_initial... OK

Step 4: Verify Migrations
python manage.py showmigrations

You should see:

courses
[X] 0001_initial

OUTPUT:
<img width="1817" height="1008" alt="image" src="https://github.com/user-attachments/assets/72e0fa72-e4ca-488f-a057-f498f0430bc8" />

## TASK: 2: Django ORM Queries

Step 1: Open Django Shell
python manage.py shell

Step 2: Create Departments
from courses.models import *

cs = Department.objects.create(
    name="Computer Science",
    head_of_dept="Dr Kumar",
    budget=100000
)

it = Department.objects.create(
    name="Information Technology",
    head_of_dept="Dr Ravi",
    budget=80000
)
Step 3: Create Courses
      Course.objects.create(name="Python", code="PY101", credits=4, department=cs)
      Course.objects.create(name="Django", code="DJ102", credits=3, department=cs)
      Course.objects.create(name="Java", code="JV103", credits=4, department=it)
      Course.objects.create(name="DBMS", code="DB104", credits=3, department=it)
      Step 4: Create Students
      Student.objects.create(
          first_name="John",
          last_name="David",
          email="john@gmail.com",
          department=cs,
          enrollment_year=2024
      )

Student.objects.create(
    first_name="Priya",
    last_name="M",
    email="priya@gmail.com",
    department=it,
    enrollment_year=2024
)

Create remaining students similarly.

Step 5: Query Courses
    Course.objects.filter(
        department__name="Computer Science"
    )

Step 6: Count Courses
      from django.db.models import Count
      
      Department.objects.annotate(
          course_count=Count('course')
      )

Step 7: Update Budget
    from django.db.models import F
    
    Department.objects.update(
        budget=F('budget') * 1.1
    )
Step 8: exit

OUTPUT:
<img width="773" height="252" alt="image" src="https://github.com/user-attachments/assets/eae5d05e-5145-4582-8cfb-20f01b11a92c" />
<img width="655" height="227" alt="image" src="https://github.com/user-attachments/assets/f7ab198c-c2da-4807-8b2d-b3466df8bbf9" />
<img width="682" height="247" alt="image" src="https://github.com/user-attachments/assets/163cfdcb-8429-4f1c-b057-dd149022337d" />
<img width="677" height="280" alt="image" src="https://github.com/user-attachments/assets/370437fb-d5f0-452d-91aa-75fce1a13a7e" />
<img width="568" height="162" alt="image" src="https://github.com/user-attachments/assets/63409364-b888-4233-97cb-960ee503ddf2" />

## TASK 3
