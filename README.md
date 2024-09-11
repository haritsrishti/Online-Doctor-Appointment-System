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
* Java, Spring Boot (JPA, MVC)
* Spring Security (for authentication)

Database:
* MySQL

Cloud:
* AWS for hosting

Payment Gateway:
* Stripe, Razorpay

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
## 1. User Table (Common table for all users - Admin, Doctor, and Patient)

User (

  user_id          INT PRIMARY KEY AUTO_INCREMENT,
  
  name             VARCHAR(100) NOT NULL,
  
  email            VARCHAR(100) UNIQUE NOT NULL,
  
  password         VARCHAR(255) NOT NULL,    -- Password should be hashed
  
  role             ENUM('ADMIN', 'DOCTOR', 'PATIENT') NOT NULL,
  
  phone            VARCHAR(15),
  
  created_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  
  updated_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
  
);

## 2. Doctor Table

Doctor (

  doctor_id        INT PRIMARY KEY,        -- Foreign Key to User (user_id)
  
  specialty        VARCHAR(100) NOT NULL,
  
  experience       INT NOT NULL,
  
  max_appointments INT DEFAULT 30,         -- Max 30 appointments allowed per doctor
  
  status           ENUM('PENDING', 'APPROVED', 'REJECTED') DEFAULT 'PENDING',
  
  license_number   VARCHAR(100),           -- License details for admin to verify
  
  FOREIGN KEY (doctor_id) REFERENCES User(user_id) ON DELETE CASCADE
  
);

## 3. Patient Table

Patient (

  patient_id       INT PRIMARY KEY,        -- Foreign Key to User (user_id)
  
  address          VARCHAR(255),
  
  date_of_birth    DATE,
  
  emergency_contact VARCHAR(15),
  
  FOREIGN KEY (patient_id) REFERENCES User(user_id) ON DELETE CASCADE
  
);

## 4. Appointment Table

Appointment (

  appointment_id   INT PRIMARY KEY AUTO_INCREMENT,
  
  doctor_id        INT NOT NULL,           -- Foreign Key to Doctor (doctor_id)
  
  patient_id       INT NOT NULL,           -- Foreign Key to Patient (patient_id)
  
  appointment_date TIMESTAMP NOT NULL,     -- Scheduled date of appointment
  
  status           ENUM('SCHEDULED', 'CANCELLED', 'COMPLETED') DEFAULT 'SCHEDULED',
  
  created_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  
  updated_at       TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  
  FOREIGN KEY (doctor_id) REFERENCES Doctor(doctor_id),
  
  FOREIGN KEY (patient_id) REFERENCES Patient(patient_id)
  
);

## 5. Payment Table

Payment (

  payment_id       INT PRIMARY KEY AUTO_INCREMENT,
  
  appointment_id   INT NOT NULL,           -- Foreign Key to Appointment (appointment_id)
  
  amount           DOUBLE NOT NULL,
  
  status           ENUM('PAID', 'FAILED') DEFAULT 'PAID',
  
  payment_date     TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  
  FOREIGN KEY (appointment_id) REFERENCES Appointment(appointment_id) ON DELETE CASCADE
  
);

## 6. Consultation History Table (Optional)

This table stores the consultation notes between a doctor and patient for each appointment.

Consultation_History (

  history_id       INT PRIMARY KEY AUTO_INCREMENT,
  
  appointment_id   INT NOT NULL,           -- Foreign Key to Appointment (appointment_id)
  
  doctor_notes     TEXT,                   -- Doctor's consultation notes
  
  patient_notes    TEXT,                   -- Patient's feedback or symptoms
  
  consultation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  
  FOREIGN KEY (appointment_id) REFERENCES Appointment(appointment_id) ON DELETE CASCADE
  
);

## 7. Notification Table

To manage notifications when an appointment gets canceled or becomes available.

Notification (

  notification_id  INT PRIMARY KEY AUTO_INCREMENT,
  
  user_id          INT NOT NULL,           -- Foreign Key to User (user_id)
  
  message          VARCHAR(255) NOT NULL,  -- Message to be sent
  
  is_read          BOOLEAN DEFAULT FALSE,  -- If notification is read or not
  
  sent_at          TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  
  FOREIGN KEY (user_id) REFERENCES User(user_id) ON DELETE CASCADE
  
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
