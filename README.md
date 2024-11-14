Project Overview:

This project is a relational database design for a hospital management system. It organizes and manages data for patient information, medical histories, doctor schedules, appointments, and diagnoses, helping streamline hospital operations and improve data accessibility.

Features:
1. Patient Management: Store and retrieve patient data, including contact information and medical history.
2. Doctor Management: Record details of doctors, including schedule management and diagnoses.
3. Appointment System: Schedule and manage patient appointments by automatically tracking status, times, and patient visit details.
4. Medical History and Diagnoses: Track patient medical history and diagnoses provided by doctors.
5. Scheduling and Maintenance: Manage doctor schedules and ensure efficient use of hospital resources.

Database Structure:

The database is organized into multiple tables with the following primary entities:

1. Patients: Stores patient information.
2. Doctors: Records doctor data and their associated schedules.
3. Appointments: Tracks appointments, linking patients with doctors.
4. Medical History: Manages patient health records and allows doctors to view or add new diagnoses.

DROP SCHEMA IF EXISTS hospital_management CASCADE;
CREATE SCHEMA IF NOT EXISTS hospital_management;

DROP TABLE IF EXISTS online_retail_app.user_login;
CREATE TABLE IF NOT EXISTS online_retail_app.user_login (
	user_id TEXT PRIMARY KEY,
    user_password TEXT,
    first_name TEXT,
	last_name TEXT,
	sign_up_on DATE,
	email_id TEXT
);

DROP TABLE IF EXISTS hospital_management.patient;
CREATE TABLE IF NOT EXISTS hospital_management.patient (
    email VARCHAR(50) PRIMARY KEY,
    password varchar(30) NOT NULL,
    name VARCHAR(50) NOT NULL,
    address varchar(60) NOT NULL,
    gender VARCHAR(20) NOT NULL
);

DROP TABLE IF EXISTS hospital_management.medical_history;
CREATE TABLE IF NOT EXISTS hospital_management.medical_history (
    medical_history_id int PRIMARY KEY,
    date DATE NOT NULL,
    conditions VARCHAR(100) NOT NULL,
    surgeries VARCHAR(100) NOT NULL,
    medication VARCHAR(100) NOT NULL
);

DROP TABLE IF EXISTS hospital_management.doctor;
CREATE TABLE IF NOT EXISTS hospital_management.doctor (
    email VARCHAR(50) PRIMARY KEY,
    gender varchar(20) NOT NULL,
    password varchar(30) NOT NULL,
    name VARCHAR(50) NOT NULL
);

DROP TABLE IF EXISTS hospital_management.appointment;
CREATE TABLE IF NOT EXISTS hospital_management.appointment (
    appointment_id int PRIMARY KEY,
    date DATE NOT NULL,
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    status varchar(15) NOT NULL
);

DROP TABLE IF EXISTS hospital_management.patient_visits;
CREATE TABLE IF NOT EXISTS hospital_management.patient_visits (
    patient VARCHAR(50) NOT NULL,
    appt SERIAL,
    concerns varchar(40) NOT NULL,
    symptoms varchar(40) NOT NULL,
    FOREIGN KEY (patient) REFERENCES hospital_management.patient (email),
    FOREIGN KEY (appt) REFERENCES hospital_management.appointment (appointment_id),
    PRIMARY KEY (patient, appt)
);

DROP TABLE IF EXISTS hospital_management.schedule;
CREATE TABLE IF NOT EXISTS hospital_management.schedule (
    schedule_id SERIAL UNIQUE,
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    break_time TIME NOT NULL,
    day varchar(20) NOT NULL,
    PRIMARY KEY (schedule_id, start_time, end_time, break_time, day)
);

DROP TABLE IF EXISTS hospital_management.patients_history;
CREATE TABLE IF NOT EXISTS hospital_management.patients_history (
    patient VARCHAR(50) NOT NULL,
    history SERIAL,
    FOREIGN KEY (patient) REFERENCES hospital_management.patient (email),
    FOREIGN KEY (history) REFERENCES hospital_management.medical_history (medical_history_id),
    PRIMARY KEY (history)
);

DROP TABLE IF EXISTS hospital_management.diagnose;
CREATE TABLE IF NOT EXISTS hospital_management.diagnose (
    appt SERIAL,
    doctor VARCHAR(50) NOT NULL,
    diagnosis varchar(40) NOT NULL,
    prescription VARCHAR(50) NOT NULL,
    FOREIGN KEY (appt) REFERENCES hospital_management.appointment (appointment_id),
    FOREIGN KEY (doctor) REFERENCES hospital_management.doctor (email),
    PRIMARY KEY (appt, doctor)
);

DROP TABLE IF EXISTS hospital_management.doctor_schedules;
CREATE TABLE IF NOT EXISTS hospital_management.doctor_schedules (
    sched SERIAL,
    doctor VARCHAR(50) NOT NULL,
    FOREIGN KEY (sched) REFERENCES hospital_management.schedule (schedule_id),
    FOREIGN KEY (doctor) REFERENCES hospital_management.doctor (email),
    PRIMARY KEY (sched, doctor)
);

DROP TABLE IF EXISTS hospital_management.doctor_view_history;
CREATE TABLE IF NOT EXISTS hospital_management.doctor_view_history (
    history SERIAL,
    doctor VARCHAR(50) NOT NULL,
    FOREIGN KEY (doctor) REFERENCES hospital_management.doctor (email),
    FOREIGN KEY (history) REFERENCES hospital_management.medical_history (medical_history_id),
    PRIMARY KEY (history, doctor)
);
