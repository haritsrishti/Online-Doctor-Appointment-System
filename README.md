# Online-Doctor-Appointment-System

## 1. Introduction
Health is the most important aspect of a person’s life, and medical emergencies can arise at any moment. This web application facilitates the process of booking an appointment with a doctor online. Patients can register, view available doctors, book appointments, and even pay for consultations. Similarly, doctors can register and manage their appointments. The system also handles appointment cancellations and notifications in case of slot availability.

## 2. Problem Statement
The goal of this project is to build an online system where patients can book appointments with doctors based on their specialty, availability, and other criteria. The system must also allow doctors to manage their appointments and consultation history. Additionally, a payment gateway will be integrated for consultation charges.

## 3. Features and Requirements

### Functional Requirements
1. Doctor Registration Portal:
Doctors can register with details like name, specialty, and years of experience.

2. Patient Registration Portal:
Patients can register with their personal details.

3. Appointment Booking:
Patients can book appointments with doctors based on their availability.

4. Appointment Limit:
Doctors can accept a maximum of 30 appointments.

5. Cancellation and Notification:
Patients can cancel appointments at least 2 days before the consultation date.
Notification is sent to patients when a canceled appointment slot becomes available.

6. Payment System:
Patients must pay a consultation fee while booking an appointment.

7. Rescheduling:
Doctors can reschedule or cancel appointments and notify the patients.

8. Consultation History:
Both patients and doctors can maintain a history of consultations.

### Non-Functional Requirements

* Security: Ensure that user data is protected, and the payment system is secure.

* Scalability: The system should handle a growing number of users efficiently.

* Performance: The system should respond quickly to booking, cancellation, and notification requests.

## 4. Technology Stack

Backend:
* Java, Spring Boot (JPA, MVC, AOP)
* Spring Security (for authentication)
* Spring Data JPA (for database interaction)

Database:
* MySQL/PostgreSQL
* Frontend (optional):
* HTML/CSS, JavaScript (for UI)

Cloud (optional):
* AWS or GCP for hosting
* Payment Gateway:
* Stripe, Razorpay, or PayPal

Version Control:
* GitHub (for code management)

## 5. System Architecture
### Architecture Overview
The system follows an MVC (Model-View-Controller) architecture, where:

* Model: Represents the application data (entities like Doctor, Patient, Appointment).
* View: Represents the presentation layer (optional, if you use UI).
* Controller: Handles the interaction between the user interface and the backend (via REST APIs).

The project also follows a service-oriented design, where different services (e.g., AppointmentService, DoctorService) handle specific functionalities.

## System Components
### User Management:
DoctorController: Manages doctor registration and appointment schedules.
PatientController: Manages patient registration and appointment bookings.

### Appointment Management:
AppointmentController: Handles all appointment-related functionalities, such as booking, cancellation, and notifications.
Notification Service: Sends email notifications to patients regarding appointment availability.

### Payment Processing:
PaymentController: Processes payment for appointments using a third-party payment gateway.

### Database Layer:
JPA entities for managing persistent data (Doctor, Patient, Appointment, etc.).

### Security:
Spring Security is used for user authentication and authorization.

## 6. High-Level Design (HLD)
The High-Level Design provides an overview of the application's major components and their interactions.

### Components:
* User Interface (UI): Allows patients and doctors to interact with the system. (Optional if using API-based approach).
* RESTful APIs: Provides endpoints for managing appointments, payments, and notifications.
* Database: Stores all data related to doctors, patients, appointments, and payment details.

## 7. Low-Level Design (LLD)
The Low-Level Design includes the detailed structure of the classes, methods, and modules that will be implemented.

### Entities and Relationships:
#### Doctor:

* doctor_id: Integer (Primary Key)
* name: String
* specialty: String
* experience: Integer
* maxAppointments: Integer (Maximum appointments allowed, default 30)
  
#### Patient:

* patient_id: Integer (Primary Key)
* name: String
* email: String
* phone: String
  
#### Appointment:

* appointment_id: Integer (Primary Key)
* doctor_id: Foreign Key (refers to Doctor)
* patient_id: Foreign Key (refers to Patient)
* date: Timestamp
* status: String (Scheduled, Cancelled, etc.)

#### Payment:

* payment_id: Integer (Primary Key)
* appointment_id: Foreign Key (refers to Appointment)
* amount: Double
* status: String (Paid, Failed)

## 8. Database Design
### Tables
### Doctor Table:

CREATE TABLE Doctor (

  doctor_id INT AUTO_INCREMENT PRIMARY KEY,
  
  name VARCHAR(50),
  
  specialty VARCHAR(100),
  
  experience INT,
  
  max_appointments INT DEFAULT 30
  
);

### Patient Table:

CREATE TABLE Patient (

  patient_id INT AUTO_INCREMENT PRIMARY KEY,
  
  name VARCHAR(50),
  
  email VARCHAR(50),
  
  phone VARCHAR(15)
  
);

### Appointment Table:

CREATE TABLE Appointment (

  appointment_id INT AUTO_INCREMENT PRIMARY KEY,
  
  doctor_id INT,
  
  patient_id INT,
  
  date TIMESTAMP,
  
  status VARCHAR(20),
  
  FOREIGN KEY (doctor_id) REFERENCES Doctor(doctor_id),
  
  FOREIGN KEY (patient_id) REFERENCES Patient(patient_id)
  
);

### Payment Table:

CREATE TABLE Payment (

  payment_id INT AUTO_INCREMENT PRIMARY KEY,
  
  appointment_id INT,
  
  amount DOUBLE,
  
  status VARCHAR(20),
  
  FOREIGN KEY (appointment_id) REFERENCES Appointment(appointment_id)
  
);

## 9. API Endpoints
### Doctor API:

* POST /doctors/register: Register a new doctor.
* GET /doctors/{id}/appointments: Get all appointments for a doctor.

### Patient API:

* POST /patients/register: Register a new patient.
* POST /appointments/book: Book an appointment.
* DELETE /appointments/{id}/cancel: Cancel an appointment.

### Appointment API:

* GET /appointments/available: Check available appointments.
* POST /appointments/notify: Notify patients about available slots.

### Payment API:

* POST /payments/process: Process payment for an appointment.

## 10. Workflow
1. User Registration:

   * Doctors and patients register via separate portals.
2. Booking Appointments:

   * Patients can view available doctors and book appointments.
3. Payment:

   * After booking, the patient is redirected to a payment page.
4. Notifications:

   * In case of cancellation, the system sends an email notification to patients who had tried to book an appointment.
5. History:

   * Both doctors and patients can view their consultation history.

## 11. Conclusion
This Online Doctor Appointment System helps bridge the gap between patients and doctors, offering seamless appointment booking, consultation history tracking, and an integrated payment system. The system can scale up with more features and serve as a foundation for a full-fledged healthcare management platform.
