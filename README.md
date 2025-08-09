# Healthcare-SQL-Project
This project designs and implements a basic healthcare database to help a small clinic efficiently manage patients, doctors, appointments, and billing.
By organizing critical information into well-structured SQL tables, the clinic can quickly retrieve data for operational planning, financial tracking, and patient care coordination.

# Project Goal
Objective: Create a database for a clinic to track patients, doctors, appointments, and billing.
End Result: Be able to run queries like:

- List all upcoming appointments for a specific doctor
- Find patients with unpaid bills
- Count the number of appointments per month

# Step 1 – Define the Scope & Entities
We’ll need four main tables:
- Patients – Stores patient info
- Doctors – Stores doctor info and specialization
- Appointments – Tracks visit dates and which doctor/patient is involved
- Billing – Tracks payments for each appointment

# Step 2 – Design the Database Schema
<img width="506" height="718" alt="image" src="https://github.com/user-attachments/assets/0c944324-39b1-407a-82bf-21d946edc636" />


# Step 3 – Create the Database & Tables

<pre lang="markdown"> 

-- Create database
CREATE DATABASE clinic_db;
USE clinic_db;

-- Patients
CREATE TABLE patients (
    patient_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    dob DATE,
    phone VARCHAR(15),
    email VARCHAR(100)
);

-- Doctors
CREATE TABLE doctors (
    doctor_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    specialization VARCHAR(100)
);

-- Appointments
CREATE TABLE appointments (
    appointment_id INT PRIMARY KEY AUTO_INCREMENT,
    patient_id INT,
    doctor_id INT,
    appointment_date DATETIME,
    reason VARCHAR(255),
    FOREIGN KEY (patient_id) REFERENCES patients(patient_id),
    FOREIGN KEY (doctor_id) REFERENCES doctors(doctor_id)
);

-- Billing
CREATE TABLE billing (
    bill_id INT PRIMARY KEY AUTO_INCREMENT,
    appointment_id INT,
    amount DECIMAL(10,2),
    paid BOOLEAN,
    FOREIGN KEY (appointment_id) REFERENCES appointments(appointment_id)
);

 </pre>

# Step 4 – Insert Sample Data

<pre lang="markdown"> 
  
-- Patients
INSERT INTO patients (first_name, last_name, dob, phone, email)
VALUES
('John', 'Doe', '1985-03-15', '555-1234', 'john.doe@email.com'),
('Jane', 'Smith', '1990-07-21', '555-5678', 'jane.smith@email.com');

-- Doctors
INSERT INTO doctors (first_name, last_name, specialization)
VALUES
('Alice', 'Brown', 'Cardiology'),
('Bob', 'Green', 'Dermatology');

-- Appointments
INSERT INTO appointments (patient_id, doctor_id, appointment_date, reason)
VALUES
(1, 1, '2025-08-15 09:00:00', 'Chest pain consultation'),
(2, 2, '2025-08-16 14:00:00', 'Skin rash checkup');

-- Billing
INSERT INTO billing (appointment_id, amount, paid)
VALUES
(1, 200.00, FALSE),
(2, 150.00, TRUE);
 </pre>

# Step 5 – Run Useful Queries

**1. Upcoming appointments for a doctor**
- Helps the clinic prepare for the day/week by knowing which patients a doctor will see.
- Ensures that doctors have advance notice of cases, so they can review patient history before the visit.
<pre lang="markdown"> 

SELECT a.appointment_id, p.first_name, p.last_name, a.appointment_date, a.reason
FROM appointments a
JOIN patients p ON a.patient_id = p.patient_id
WHERE a.doctor_id = 1
ORDER BY a.appointment_date;
 </pre>
 
**2. List unpaid bills**
- Quickly identifies patients/accounts with outstanding balances so follow-ups can be made.
- For example, if the clinic sees that 10% of bills are unpaid after 30 days, they can implement payment reminders

<pre lang="markdown"> 
SELECT b.bill_id, p.first_name, p.last_name, b.amount
FROM billing b
JOIN appointments a ON b.appointment_id = a.appointment_id
JOIN patients p ON a.patient_id = p.patient_id
WHERE b.paid = FALSE;
 </pre>
 
**3. Count appointments per month**
- Shows how busy the clinic is month-to-month.
- If August is always a busy month, the clinic can hire temporary staff.
- More appointments typically means more income; knowing peak months helps with budgeting.

<pre lang="markdown"> 
SELECT MONTH(appointment_date) AS month, COUNT(*) AS total_appointments
FROM appointments
GROUP BY MONTH(appointment_date)
ORDER BY month;
 </pre>





















 





