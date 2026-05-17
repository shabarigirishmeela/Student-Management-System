# Student Management System (Java + JDBC + MySQL)

## Project Overview

This is a console-based Student Management System developed using Core Java, JDBC, and MySQL. The project allows users to:

* Add Student
* View Students
* Update Student
* Delete Student

This project is suitable for:

* Resume projects
* Internship interviews
* Java JDBC practice
* College mini project

---

# Technologies Used

* Core Java
* JDBC
* MySQL
* VS Code / IntelliJ

---

# Project Folder Structure

```text
StudentManagementSystem/
│
├── src/
│   ├── DBConnection.java
│   ├── Student.java
│   ├── StudentDAO.java
│   ├── Main.java
│
├── mysql-connector-j.jar
```

---

# Step 1: Create Database in MySQL

```sql
CREATE DATABASE studentdb;

USE studentdb;

CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    rollno VARCHAR(50),
    marks DOUBLE
);
```

---

# Step 2: Download MySQL JDBC Driver

Download:

[https://dev.mysql.com/downloads/connector/j/](https://dev.mysql.com/downloads/connector/j/)

Add the JAR file into your project.

---

# Step 3: DBConnection.java

```java
import java.sql.Connection;
import java.sql.DriverManager;

public class DBConnection {

    private static final String URL = "jdbc:mysql://localhost:3306/studentdb";
    private static final String USER = "root";
    private static final String PASSWORD = "root";

    public static Connection getConnection() {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection con = DriverManager.getConnection(URL, USER, PASSWORD);
            return con;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

---

# Step 4: Student.java

```java
public class Student {

    private int id;
    private String name;
    private String rollno;
    private double marks;

    public Student() {
    }

    public Student(String name, String rollno, double marks) {
        this.name = name;
        this.rollno = rollno;
        this.marks = marks;
    }

    public Student(int id, String name, String rollno, double marks) {
        this.id = id;
        this.name = name;
        this.rollno = rollno;
        this.marks = marks;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getRollno() {
        return rollno;
    }

    public void setRollno(String rollno) {
        this.rollno = rollno;
    }

    public double getMarks() {
        return marks;
    }

    public void setMarks(double marks) {
        this.marks = marks;
    }
}
```

---

# Step 5: StudentDAO.java

```java
import java.sql.*;

public class StudentDAO {

    Connection con = DBConnection.getConnection();

    public void addStudent(Student s) {
        try {
            String query = "INSERT INTO students(name, rollno, marks) VALUES(?,?,?)";

            PreparedStatement ps = con.prepareStatement(query);

            ps.setString(1, s.getName());
            ps.setString(2, s.getRollno());
            ps.setDouble(3, s.getMarks());

            int rows = ps.executeUpdate();

            if (rows > 0) {
                System.out.println("Student Added Successfully");
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void viewStudents() {
        try {
            String query = "SELECT * FROM students";

            Statement st = con.createStatement();

            ResultSet rs = st.executeQuery(query);

            System.out.println("--------------------------------------");
            System.out.println("ID\tNAME\tROLLNO\tMARKS");
            System.out.println("--------------------------------------");

            while (rs.next()) {
                System.out.println(
                        rs.getInt("id") + "\t" +
                        rs.getString("name") + "\t" +
                        rs.getString("rollno") + "\t" +
                        rs.getDouble("marks")
                );
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void updateStudent(int id, double marks) {
        try {
            String query = "UPDATE students SET marks=? WHERE id=?";

            PreparedStatement ps = con.prepareStatement(query);

            ps.setDouble(1, marks);
            ps.setInt(2, id);

            int rows = ps.executeUpdate();

            if (rows > 0) {
                System.out.println("Student Updated Successfully");
            } else {
                System.out.println("Student Not Found");
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public void deleteStudent(int id) {
        try {
            String query = "DELETE FROM students WHERE id=?";

            PreparedStatement ps = con.prepareStatement(query);

            ps.setInt(1, id);

            int rows = ps.executeUpdate();

            if (rows > 0) {
                System.out.println("Student Deleted Successfully");
            } else {
                System.out.println("Student Not Found");
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

# Step 6: Main.java

```java
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        StudentDAO dao = new StudentDAO();

        while (true) {

            System.out.println("\n===== STUDENT MANAGEMENT SYSTEM =====");
            System.out.println("1. Add Student");
            System.out.println("2. View Students");
            System.out.println("3. Update Student Marks");
            System.out.println("4. Delete Student");
            System.out.println("5. Exit");

            System.out.print("Enter Choice: ");
            int choice = sc.nextInt();

            switch (choice) {

                case 1:

                    sc.nextLine();

                    System.out.print("Enter Name: ");
                    String name = sc.nextLine();

                    System.out.print("Enter Roll Number: ");
                    String roll = sc.nextLine();

                    System.out.print("Enter Marks: ");
                    double marks = sc.nextDouble();

                    Student s = new Student(name, roll, marks);

                    dao.addStudent(s);
                    break;

                case 2:
                    dao.viewStudents();
                    break;

                case 3:

                    System.out.print("Enter Student ID: ");
                    int id = sc.nextInt();

                    System.out.print("Enter New Marks: ");
                    double newMarks = sc.nextDouble();

                    dao.updateStudent(id, newMarks);
                    break;

                case 4:

                    System.out.print("Enter Student ID: ");
                    int deleteId = sc.nextInt();

                    dao.deleteStudent(deleteId);
                    break;

                case 5:
                    System.out.println("Thank You");
                    System.exit(0);

                default:
                    System.out.println("Invalid Choice");
            }
        }
    }
}
```

---

# How to Run the Project

## Step 1

Install:

* Java JDK
* MySQL
* VS Code

---

## Step 2

Create database and table using MySQL.

---

## Step 3

Add MySQL Connector JAR file.

---

## Step 4

Run Main.java

---

# Sample Output

```text
===== STUDENT MANAGEMENT SYSTEM =====
1. Add Student
2. View Students
3. Update Student Marks
4. Delete Student
5. Exit

Enter Choice: 1

Enter Name: shabari
Enter Roll Number: 21
Enter Marks: 95

Student Added Successfully
```

---

# Features Added

* JDBC Connectivity
* CRUD Operations
* PreparedStatement
* MySQL Database
* Menu Driven Console Application
* Object Oriented Programming

---

# Interview Points

## Architecture

User → Java Console Application → JDBC → MySQL Database

---

## Why JDBC?

JDBC is used to connect Java applications with relational databases.

---

## Why PreparedStatement?

* Better performance
* Prevents SQL Injection
* Dynamic values support

---

## What is DAO?

DAO stands for Data Access Object.
It contains database-related operations.

---

# Resume Description

Developed a console-based Student Management System using Core Java, JDBC, and MySQL to perform CRUD operations such as adding, updating, viewing, and deleting student records. Implemented database connectivity using JDBC and followed object-oriented programming principles.

---

# README.md

````md
<div align="center">

# 🎓 Student Management System

### Java + JDBC + MySQL Console Application

A simple and efficient console-based Student Management System developed using **Core Java**, **JDBC**, and **MySQL**.

</div>

---

# 🚀 Tech Stack

<div align="center">

| Technology | Usage |
|---|---|
| ☕ Core Java | Application Logic |
| 🔌 JDBC | Database Connectivity |
| 🛢️ MySQL | Database Management |
| 💻 VS Code | Development Environment |

</div>

---

# ✨ Features

✔️ Add Student  
✔️ View Students  
✔️ Update Student Details  
✔️ Delete Student  
✔️ JDBC Connectivity  
✔️ CRUD Operations  
✔️ Object Oriented Programming  

---

# 📁 Project Structure

```text
StudentManagementSystem/
│
├── src/
│   ├── DBConnection.java
│   ├── Student.java
│   ├── StudentDAO.java
│   ├── Main.java
│
├── mysql-connector-j-9.7.0.jar
├── README.md
````

---

# 🔗 JDBC Driver

### Driver Used

```text
com.mysql.cj.jdbc.Driver
```

### Download JDBC Driver

[https://dev.mysql.com/downloads/connector/j/](https://dev.mysql.com/downloads/connector/j/)

### Setup Process

1️⃣ Download Platform Independent ZIP
2️⃣ Extract ZIP File
3️⃣ Copy `mysql-connector-j-9.7.0.jar` into project folder
4️⃣ Add JAR file to Java Classpath

---

# 🛢️ Database Information

| Database  | Table    |
| --------- | -------- |
| studentdb | students |

---

# ⚙️ Project Architecture

```text
User
   ↓
Java Console Application
   ↓
JDBC
   ↓
MySQL Database
```

---

# 📌 Functional Modules

* ➕ Add Student
* 📋 View Students
* ✏️ Update Student
* ❌ Delete Student

---

# 👨‍💻 Developed By

## Shabari Girish

B.Tech CSE Student

```
```
