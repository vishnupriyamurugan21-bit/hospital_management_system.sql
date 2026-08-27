# hospital_management_system.sql

-- HOSPITAL MANAGEMENT SYSTEM-SQL PROJECT
CREATE DATABASE hos_management;
USE hospital_management;

-- 1. CREATE TABLES
CREATE TABLE departments (
    department_id INT PRIMARY KEY AUTO_INCREMENT,
    department_name VARCHAR(100) NOT NULL
);
CREATE TABLE doctors (
    doctor_id INT PRIMARY KEY AUTO_INCREMENT,
    doctor_name VARCHAR(100) NOT NULL,
    specialization VARCHAR(100),
    phone VARCHAR(15),
    email VARCHAR(100),
    department_id INT,
    FOREIGN KEY (department_id)
    REFERENCES departments(department_id)
);
CREATE TABLE patients (
    patient_id INT PRIMARY KEY AUTO_INCREMENT,
    patient_name VARCHAR(100) NOT NULL,
    dob DATE,
    gender VARCHAR(10),
    phone VARCHAR(15),
    address VARCHAR(200)
);
CREATE TABLE appointments (
    appointment_id INT PRIMARY KEY AUTO_INCREMENT,
    patient_id INT,
    doctor_id INT,
    appointment_date DATE,
    appointment_time TIME,
    status VARCHAR(20),

    FOREIGN KEY (patient_id)
    REFERENCES patients(patient_id),

    FOREIGN KEY (doctor_id)
    REFERENCES doctors(doctor_id)
);
CREATE TABLE rooms (
    room_id INT PRIMARY KEY AUTO_INCREMENT,
    room_number VARCHAR(20) UNIQUE,
    room_type VARCHAR(50),
    status VARCHAR(20)
);
CREATE TABLE admissions (
    admission_id INT PRIMARY KEY AUTO_INCREMENT,
    patient_id INT,
    room_id INT,
    admission_date DATE,
    discharge_date DATE,

    FOREIGN KEY (patient_id)
    REFERENCES patients(patient_id),

    FOREIGN KEY (room_id)
    REFERENCES rooms(room_id)
);
CREATE TABLE prescriptions (
    prescription_id INT PRIMARY KEY AUTO_INCREMENT,
    patient_id INT,
    doctor_id INT,
    medicine_name VARCHAR(100),
    dosage VARCHAR(100),
    prescription_date DATE,

    FOREIGN KEY (patient_id)
    REFERENCES patients(patient_id),

    FOREIGN KEY (doctor_id)
    REFERENCES doctors(doctor_id)
);
CREATE TABLE bills (
    bill_id INT PRIMARY KEY AUTO_INCREMENT,
    patient_id INT,
    amount DECIMAL(10,2),
    bill_date DATE,
    payment_status VARCHAR(20),

    FOREIGN KEY (patient_id)
    REFERENCES patients(patient_id)
);
CREATE INDEX idx_patient_name
ON patients(patient_name);

-- 2. INSERT DATA
INSERT INTO departments (department_name)
VALUES
('Cardiology'),
('Neurology'),
('Orthopedics'),
('General Medicine'),
('Pediatrics');
INSERT INTO doctors
(doctor_name, specialization, phone, email, department_id)
VALUES
('Dr. Arun Kumar', 'Cardiologist', '9876543210', 'arun@gmail.com', 1),
('Dr. Priya Devi', 'Neurologist', '9876543211', 'priya@gmail.com', 2),
('Dr. Ravi Kumar', 'Orthopedic', '9876543212', 'ravi@gmail.com', 3),
('Dr. Meena', 'General Physician', '9876543213', 'meena@gmail.com', 4),
('Dr. Kumar', 'Pediatrician', '9876543214', 'kumar@gmail.com', 5);
INSERT INTO patients
(patient_name, dob, gender, phone, address)
VALUES
('Vishnu', '2000-05-12', 'Male', '9000000001', 'Salem'),
('Anitha', '1998-08-20', 'Female', '9000000002', 'Chennai'),
('Rahul', '2002-03-15', 'Male', '9000000003', 'Coimbatore'),
('Priya', '1995-11-10', 'Female', '9000000004', 'Madurai'),
('Arun', '2001-07-25', 'Male', '9000000005', 'Salem'),
('Meena', '1999-02-18', 'Female', '9000000006', 'Chennai');
INSERT INTO appointments
(patient_id, doctor_id, appointment_date, appointment_time, status)
VALUES
(1, 1, '2026-08-25', '10:00:00', 'Scheduled'),
(2, 2, '2026-08-25', '11:00:00', 'Scheduled'),
(3, 3, '2026-08-26', '10:30:00', 'Completed'),
(4, 4, '2026-08-26', '12:00:00', 'Scheduled'),
(5, 1, '2026-08-27', '09:30:00', 'Completed'),
(6, 2, '2026-08-27', '11:30:00', 'Cancelled');
INSERT INTO rooms
(room_number, room_type, status)
VALUES
('101', 'General', 'Available'),
('102', 'General', 'Occupied'),
('201', 'Private', 'Available'),
('202', 'ICU', 'Occupied'),
('203', 'Private', 'Available');
INSERT INTO admissions
(patient_id, room_id, admission_date, discharge_date)
VALUES
(1, 2, '2026-08-20', NULL),
(2, 4, '2026-08-21', NULL),
(3, 1, '2026-08-10', '2026-08-15'),
(4, 3, '2026-08-18', '2026-08-22');
INSERT INTO prescriptions
(patient_id, doctor_id, medicine_name, dosage, prescription_date)
VALUES
(1, 1, 'Aspirin', '100mg', '2026-08-20'),
(2, 2, 'Paracetamol', '500mg', '2026-08-21'),
(3, 3, 'Ibuprofen', '400mg', '2026-08-10'),
(4, 4, 'Amoxicillin', '250mg', '2026-08-18'),
(5, 1, 'Aspirin', '100mg', '2026-08-20');
INSERT INTO bills
(patient_id, amount, bill_date, payment_status)
VALUES
(1, 15000, '2026-08-20', 'Paid'),
(2, 25000, '2026-08-21', 'Pending'),
(3, 10000, '2026-08-15', 'Paid'),
(4, 18000, '2026-08-22', 'Paid'),
(5, 22000, '2026-08-20', 'Pending'),
(6, 12000, '2026-08-23', 'Paid');

-- 3. BASIC SELECT QUERIES
-- 1. Display all patients
SELECT * FROM patients;

-- 2. Display selected coloumn
SELECT patient_id, patient_name, gender
FROM patients;

-- 3. display female patients
SELECT *
FROM patients
WHERE gender = 'Female';

-- 4. display patients from salem
SELECT *
FROM patients
WHERE address = 'Salem';

-- 5.display Sort patients by name
SELECT *
FROM patients
ORDER BY patient_name ASC;

 -- 6.dispaly Sort bills from highest to lowest
SELECT *
FROM bills
ORDER BY amount DESC;

-- 4.LIKE QUEERY
SELECT *
FROM patients
WHERE patient_name LIKE 'A%';
SELECT *
FROM patients
WHERE patient_name LIKE '%a';
SELECT *
FROM patients
WHERE patient_name LIKE '%vi%';

-- 5.AGGREGATE FUNCTION
-- 1.total patients
SELECT COUNT(*) AS total_patients
FROM patients;
 -- 2.total doctors
SELECT COUNT(*) AS total_doctors
FROM doctors;
 -- 3. highest bill
SELECT MAX(amount) AS highest_bill
FROM bills;
-- 4. lowest bill
SELECT MIN(amount) AS lowest_bill
FROM bills;
-- 5. average bill
SELECT AVG(amount) AS average_bill
FROM bills;
-- 6. total bill amount
SELECT SUM(amount) AS total_amount
FROM bills;

-- 6. GROUP BY
-- 1. Number of patients by gender
SELECT
    gender,
    COUNT(*) AS total
FROM patients
GROUP BY gender;
-- 2. Number of doctors in each department
SELECT
    department_id,
    COUNT(*) AS total_doctors
FROM doctors
GROUP BY department_id;

-- 3. Total bill by payment status
SELECT
    payment_status,
    SUM(amount) AS total_amount
FROM bills
GROUP BY payment_status;

-- 7. HAVING
-- 1. Departments having more than one doctor
SELECT
    department_id,
    COUNT(*) AS total_doctors
FROM doctors
GROUP BY department_id
HAVING COUNT(*) > 1;
-- 2. Payment status with total above 30000
SELECT
    payment_status,
    SUM(amount) AS total_amount
FROM bills
GROUP BY payment_status
HAVING SUM(amount) > 30000;

-- 8. INNER JOIN
-- 1. Patient and appointment details
SELECT
    p.patient_name,
    a.appointment_date,
    a.status
FROM patients p
INNER JOIN appointments a
ON p.patient_id = a.patient_id;
-- 2. Patient + Doctor
SELECT
    p.patient_name,
    d.doctor_name,
    d.specialization
FROM appointments a
JOIN patients p
ON a.patient_id = p.patient_id
JOIN doctors d
ON a.doctor_id = d.doctor_id;
-- 3. Doctor + Department
SELECT
    d.doctor_name,
    d.specialization,
    dep.department_name
FROM doctors d
JOIN departments dep
ON d.department_id = dep.department_id;

-- 9. LEFT JOIN
-- 1. All patients including patients without appointments
SELECT
    p.patient_name,
    a.appointment_date,
    a.status
FROM patients p
LEFT JOIN appointments a
ON p.patient_id = a.patient_id;
-- 2. Patients without appointments
SELECT
    p.patient_name
FROM patients p
LEFT JOIN appointments a
ON p.patient_id = a.patient_id
WHERE a.appointment_id IS NULL;

-- 10. RIGHT JOIN
SELECT
    p.patient_name
FROM patients p
right JOIN appointments a
ON p.patient_id = a.patient_id
WHERE a.appointment_id IS NULL;

-- 11. UNION
SELECT patient_name AS person_name, 'Patient' AS person_type
FROM patients

UNION

SELECT doctor_name AS person_name, 'Doctor' AS person_type
FROM doctors;

-- 12. UNION ALL
SELECT patient_name AS person_name, 'Patient' AS person_type
FROM patients

UNION ALL

SELECT doctor_name AS person_name, 'Doctor' AS person_type
FROM doctors;

SELECT *
FROM patients
WHERE patient_name = 'Vishnu';

-- 13. SUB QUERRY
-- 1. Patients who have appointments
SELECT *
FROM patients
WHERE patient_id IN (
    SELECT patient_id
    FROM appointments
);
-- 2. Patients without appointments
SELECT *
FROM patients
WHERE patient_id NOT IN (
    SELECT patient_id
    FROM appointments
);
-- 3. Patients with pending bills
SELECT *
FROM patients
WHERE patient_id IN (
    SELECT patient_id
    FROM bills
    WHERE payment_status = 'Pending'
);
-- 4. Patients currently admitted
SELECT *
FROM patients
WHERE patient_id IN (
    SELECT patient_id
    FROM admissions
    WHERE discharge_date IS NULL
);

-- 14. SUB QUERRY WITH MAX
-- 1. Highest bill
SELECT *
FROM bills
WHERE amount = (
    SELECT MAX(amount)
    FROM bills
);
-- 2. Patient with highest bill
SELECT *
FROM patients
WHERE patient_id = (
    SELECT patient_id
    FROM bills
    WHERE amount = (
        SELECT MAX(amount)
        FROM bills
    )
);

-- 15. SUB QUERRY WITH AVG
-- 1. Bills above average
SELECT *
FROM bills
WHERE amount > (
    SELECT AVG(amount)
    FROM bills
);

-- 16. SUBQUERRY WITH DEPARTMENT
-- 1. Doctors from Cardiology
SELECT *
FROM doctors
WHERE department_id = (
    SELECT department_id
    FROM departments
    WHERE department_name = 'Cardiology'
);

-- 2. Doctors from Neurology
SELECT *
FROM doctors
WHERE department_id = (
    SELECT department_id
    FROM departments
    WHERE department_name = 'Neurology'
);
-- 17. NESTED SUBQERRY
SELECT *
FROM bills
WHERE amount = (
    SELECT MAX(amount)
    FROM bills
    WHERE amount < (
        SELECT MAX(amount)
        FROM bills
    )
);

-- 18. EXISTS
-- 1. Patients who have appointments
SELECT
    p.patient_id,
    p.patient_name
FROM patients p
WHERE EXISTS (
    SELECT 1
    FROM appointments a
    WHERE a.patient_id = p.patient_id
);
-- 2. Patients who have bills
SELECT
    p.patient_name
FROM patients p
WHERE EXISTS (
    SELECT 1
    FROM bills b
    WHERE b.patient_id = p.patient_id
);

-- 19. CORRELATION SUBQUERRY
SELECT
    p.patient_id,
    p.patient_name
FROM patients p
WHERE EXISTS (
    SELECT 1
    FROM bills b
    WHERE b.patient_id = p.patient_id
    AND b.amount > (
        SELECT AVG(b2.amount)
        FROM bills b2
        WHERE b2.patient_id = p.patient_id
    )
);

-- 20. UPDATE QUERRY
UPDATE patients
SET phone = '9999999999'
WHERE patient_id = 1;

UPDATE appointments
SET status = 'Completed'
WHERE appointment_id = 1;

UPDATE bills
SET payment_status = 'Paid'
WHERE bill_id = 2;

-- 20. DELETE QUERRY
DELETE FROM prescriptions
WHERE prescription_id = 5;

DELETE FROM appointments
WHERE appointment_id = 6;

DELETE FROM appointments
WHERE appointment_id = 4;

-- 21. CASE STATEMENT
SELECT
    patient_id,
    amount,
    CASE
        WHEN amount >= 20000 THEN 'High'
        WHEN amount >= 10000 THEN 'Medium'
        ELSE 'Low'
    END AS bill_category
FROM bills;

-- 22. BETWEEN
SELECT *
FROM bills
WHERE amount BETWEEN 10000 AND 20000;

-- 23. DATE FUNCTION
SELECT *
FROM appointments
WHERE appointment_date > '2026-08-25';
SELECT *
FROM appointments
WHERE appointment_date
BETWEEN '2026-08-26' AND '2026-08-27';

-- 24. STRING FUNCTION
-- 1. UPPER
SELECT UPPER(patient_name) AS patient_name
FROM patients;
-- 2. LOWER
SELECT LOWER(patient_name) AS patient_name
FROM patients;
-- 3. LENGTH
SELECT
    patient_name,
    LENGTH(patient_name) AS name_length
FROM patients;
-- 4. CONCAT
SELECT
    CONCAT(patient_name, ' - ', phone) AS patient_details
FROM patients;
-- 5. TRIM
SELECT TRIM(patient_name)
FROM patients;

-- 25. VIEW 
-- 1. VIEW using JOIN
CREATE VIEW hospital_report AS
SELECT
    p.patient_id,
    p.patient_name,
    p.gender,
    d.doctor_name,
    d.specialization,
    dep.department_name,
    a.appointment_date,
    a.status
FROM patients p
LEFT JOIN appointments a
ON p.patient_id = a.patient_id
LEFT JOIN doctors d
ON a.doctor_id = d.doctor_id
LEFT JOIN departments dep
ON d.department_id = dep.department_id;

 -- COMPLETE REPORT --
SELECT
    p.patient_id,
    p.patient_name,
    p.gender,
    d.doctor_name,
    d.specialization,
    dep.department_name,
    a.appointment_date,
    a.status,
    b.amount,
    b.payment_status
FROM patients p
LEFT JOIN appointments a
ON p.patient_id = a.patient_id
LEFT JOIN doctors d
ON a.doctor_id = d.doctor_id
LEFT JOIN departments dep
ON d.department_id = dep.department_id
LEFT JOIN bills b
ON p.patient_id = b.patient_id
ORDER BY p.patient_id;
