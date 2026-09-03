# EXP_03 - Entity Student and CRUD Operations Using Spring Boot Hibernate Configuration

## AIM

To develop a Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate).

## ALGORITHM

### 1. Create Spring Boot Project

Add dependencies:

* Spring Web
* Spring Data JPA
* H2 Database or MySQL
* Spring Boot DevTools

### 2. Configure application.properties

* Define database connection
* Enable Hibernate auto DDL

### 3. Create Student Entity Class

* Annotate with `@Entity`
* Define fields with `@Id`, `@GeneratedValue`, etc.

### 4. Create StudentRepository

Extend `JpaRepository<Student, Long>` for CRUD methods.

### 5. Create StudentController

Handle HTTP methods:

| HTTP Method | Endpoint         | Operation         |
| ----------- | ---------------- | ----------------- |
| POST        | `/students`      | Add student       |
| GET         | `/students`      | Get all students  |
| GET         | `/students/{id}` | Get student by ID |
| PUT         | `/students/{id}` | Update student    |
| DELETE      | `/students/{id}` | Delete student    |

## PROGRAM CODE

### pom.xml

```xml
<dependencies>
    <!-- Spring Boot Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring Boot JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- H2 Database (In-memory) -->
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

### application.properties

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
```

### Student.java

```java
package com.example.ex3.model;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String department;
    private int age;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDepartment() {
        return department;
    }

    public void setDepartment(String department) {
        this.department = department;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

### StudentRepository.java

```java
package com.example.ex3.repository;

import com.example.ex3.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Long> {

}
```

### StudentController.java

```java
package com.example.ex3.controller;

import com.example.ex3.model.Student;
import com.example.ex3.repository.StudentRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/students")
public class StudentController {

    @Autowired
    private StudentRepository studentRepository;

    @PostMapping
    public Student addStudent(@RequestBody Student student) {
        return studentRepository.save(student);
    }

    @GetMapping
    public List<Student> getAllStudents() {
        return studentRepository.findAll();
    }

    @GetMapping("/{id}")
    public Optional<Student> getStudent(@PathVariable Long id) {
        return studentRepository.findById(id);
    }

    @PutMapping("/{id}")
    public Student updateStudent(
            @PathVariable Long id,
            @RequestBody Student studentDetails) {

        Student student = studentRepository.findById(id).orElseThrow();

        student.setName(studentDetails.getName());
        student.setDepartment(studentDetails.getDepartment());
        student.setAge(studentDetails.getAge());

        return studentRepository.save(student);
    }

    @DeleteMapping("/{id}")
    public String deleteStudent(@PathVariable Long id) {

        studentRepository.deleteById(id);

        return "Student with ID " + id + " deleted successfully!";
    }
}
```

### DemoApplication.java

```java
package com.example.ex3;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Ex3Application {

    public static void main(String[] args) {
        SpringApplication.run(Ex3Application.class, args);
    }

}
```

### OUTPUT

#### POST /students
<img width="959" height="599" alt="image" src="https://github.com/user-attachments/assets/94cd27ee-af1d-45d3-9307-79a4ab8335c8" />

#### GET /students
<img width="959" height="599" alt="image" src="https://github.com/user-attachments/assets/6e93097c-4a0d-47a8-854f-d735baa5a9c3" />


#### GET /students/:id
<img width="959" height="598" alt="image" src="https://github.com/user-attachments/assets/d01d08d8-0a36-4f61-a997-7c407c1509b8" />


#### PUT /students/:id
<img width="959" height="599" alt="image" src="https://github.com/user-attachments/assets/bbf722c8-88b2-4a31-8c53-98e96ed4295f" />


#### DELETE /students/:id
<img width="959" height="599" alt="image" src="https://github.com/user-attachments/assets/18e3e135-dbe7-4d73-a0c2-579df7e277ea" />

### RESULT 
Thus, the Spring Boot application was successfully developed to perform CRUD (Create, Read, Update, Delete) operations on the Student entity using Spring Data JPA (Hibernate).





