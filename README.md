TASK 2
Hospital Management System

import java.util.*;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

// Main Hospital Management System Class
public class HospitalManagementSystem {
    private PatientManager patientManager;
    private AppointmentManager appointmentManager;
    private EHRManager ehrManager;
    private BillingManager billingManager;
    private InventoryManager inventoryManager;
    private StaffManager staffManager;
    private Scanner scanner;

    public HospitalManagementSystem() {
        this.patientManager = new PatientManager();
        this.appointmentManager = new AppointmentManager();
        this.ehrManager = new EHRManager();
        this.billingManager = new BillingManager();
        this.inventoryManager = new InventoryManager();
        this.staffManager = new StaffManager();
        this.scanner = new Scanner(System.in);
        
        // Initialize with sample data
        initializeSampleData();
    }

    public static void main(String[] args) {
        HospitalManagementSystem hospital = new HospitalManagementSystem();
        hospital.start();
    }

    private void initializeSampleData() {
        // Add sample staff
        staffManager.addStaff(new Staff("DR001", "Dr. Smith", "Doctor", "Cardiology", 150000));
        staffManager.addStaff(new Staff("NR001", "Nurse Johnson", "Nurse", "Emergency", 50000));
        
        // Add sample inventory
        inventoryManager.addItem(new InventoryItem("MED001", "Paracetamol", 1000, 5.0));
        inventoryManager.addItem(new InventoryItem("EQ001", "Stethoscope", 50, 200.0));
        
        // Add sample patient
        Patient patient = new Patient("John Doe", 35, "Male", "555-1234", "123 Main St", "Emergency Contact: Jane Doe");
        patientManager.addPatient(patient);
    }

    public void start() {
        System.out.println("=== HOSPITAL MANAGEMENT SYSTEM ===");
        
        while (true) {
            displayMainMenu();
            int choice = getChoice();
            
            switch (choice) {
                case 1:
                    patientMenu();
                    break;
                case 2:
                    appointmentMenu();
                    break;
                case 3:
                    ehrMenu();
                    break;
                case 4:
                    billingMenu();
                    break;
                case 5:
                    inventoryMenu();
                    break;
                case 6:
                    staffMenu();
                    break;
                case 7:
                    generateReports();
                    break;
                case 8:
                    System.out.println("Thank you for using Hospital Management System!");
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }

    private void displayMainMenu() {
        System.out.println("\n========== MAIN MENU ==========");
        System.out.println("1. Patient Management");
        System.out.println("2. Appointment Scheduling");
        System.out.println("3. Electronic Health Records");
        System.out.println("4. Billing & Invoicing");
        System.out.println("5. Inventory Management");
        System.out.println("6. Staff Management");
        System.out.println("7. Generate Reports");
        System.out.println("8. Exit");
        System.out.print("Enter your choice: ");
    }

    private int getChoice() {
        try {
            return scanner.nextInt();
        } catch (InputMismatchException e) {
            scanner.nextLine(); // Clear invalid input
            return -1;
        }
    }

    // Patient Management Menu
    private void patientMenu() {
        while (true) {
            System.out.println("\n========== PATIENT MANAGEMENT ==========");
            System.out.println("1. Register New Patient");
            System.out.println("2. View All Patients");
            System.out.println("3. Search Patient");
            System.out.println("4. Update Patient Information");
            System.out.println("5. Back to Main Menu");
            System.out.print("Enter your choice: ");
            
            int choice = getChoice();
            scanner.nextLine(); // Consume newline
            
            switch (choice) {
                case 1:
                    registerNewPatient();
                    break;
                case 2:
                    patientManager.displayAllPatients();
                    break;
                case 3:
                    searchPatient();
                    break;
                case 4:
                    updatePatient();
                    break;
                case 5:
                    return;
                default:
                    System.out.println("Invalid choice.");
            }
        }
    }

    private void registerNewPatient() {
        System.out.println("\n--- Register New Patient ---");
        System.out.print("Name: ");
        String name = scanner.nextLine();
        System.out.print("Age: ");
        int age = scanner.nextInt();
        scanner.nextLine();
        System.out.print("Gender: ");
        String gender = scanner.nextLine();
        System.out.print("Phone: ");
        String phone = scanner.nextLine();
        System.out.print("Address: ");
        String address = scanner.nextLine();
        System.out.print("Emergency Contact: ");
        String emergencyContact = scanner.nextLine();
        
        Patient patient = new Patient(name, age, gender, phone, address, emergencyContact);
        patientManager.addPatient(patient);
        System.out.println("Patient registered successfully! Patient ID: " + patient.getPatientId());
    }

    private void searchPatient() {
        System.out.print("Enter Patient ID or Name: ");
        String searchTerm = scanner.nextLine();
        Patient patient = patientManager.searchPatient(searchTerm);
        if (patient != null) {
            patient.displayInfo();
        } else {
            System.out.println("Patient not found.");
        }
    }

    private void updatePatient() {
        System.out.print("Enter Patient ID: ");
        String patientId = scanner.nextLine();
        patientManager.updatePatient(patientId, scanner);
    }

    // Appointment Management Menu
    private void appointmentMenu() {
        while (true) {
            System.out.println("\n========== APPOINTMENT SCHEDULING ==========");
            System.out.println("1. Schedule New Appointment");
            System.out.println("2. View All Appointments");
            System.out.println("3. View Appointments by Date");
            System.out.println("4. Cancel Appointment");
            System.out.println("5. Back to Main Menu");
            System.out.print("Enter your choice: ");
            
            int choice = getChoice();
            scanner.nextLine();
            
            switch (choice) {
                case 1:
                    scheduleAppointment();
                    break;
                case 2:
                    appointmentManager.displayAllAppointments();
                    break;
                case 3:
                    viewAppointmentsByDate();
                    break;
                case 4:
                    cancelAppointment();
                    break;
                case 5:
                    return;
                default:
                    System.out.println("Invalid choice.");
            }
        }
    }

    private void scheduleAppointment() {
        System.out.println("\n--- Schedule New Appointment ---");
        System.out.print("Patient ID: ");
        String patientId = scanner.nextLine();
        System.out.print("Doctor ID: ");
        String doctorId = scanner.nextLine();
        System.out.print("Date (YYYY-MM-DD): ");
        String dateStr = scanner.nextLine();
        System.out.print("Time (HH:MM): ");
        String timeStr = scanner.nextLine();
        System.out.print("Purpose: ");
        String purpose = scanner.nextLine();
        
        try {
            LocalDateTime dateTime = LocalDateTime.parse(dateStr + "T" + timeStr);
            Appointment appointment = new Appointment(patientId, doctorId, dateTime, purpose);
            appointmentManager.scheduleAppointment(appointment);
            System.out.println("Appointment scheduled successfully! ID: " + appointment.getAppointmentId());
        } catch (Exception e) {
            System.out.println("Invalid date/time format.");
        }
    }

    private void viewAppointmentsByDate() {
        System.out.print("Enter date (YYYY-MM-DD): ");
        String dateStr = scanner.nextLine();
        try {
            LocalDate date = LocalDate.parse(dateStr);
            appointmentManager.displayAppointmentsByDate(date);
        } catch (Exception e) {
            System.out.println("Invalid date format.");
        }
    }

    private void cancelAppointment() {
        System.out.print("Enter Appointment ID: ");
        String appointmentId = scanner.nextLine();
        appointmentManager.cancelAppointment(appointmentId);
    }

    // EHR Menu
    private void ehrMenu() {
        while (true) {
            System.out.println("\n========== ELECTRONIC HEALTH RECORDS ==========");
            System.out.println("1. Create Medical Record");
            System.out.println("2. View Patient Records");
            System.out.println("3. Update Medical Record");
            System.out.println("4. Back to Main Menu");
            System.out.print("Enter your choice: ");
            
            int choice = getChoice();
            scanner.nextLine();
            
            switch (choice) {
                case 1:
                    createMedicalRecord();
                    break;
                case 2:
                    viewPatientRecords();
                    break;
                case 3:
                    updateMedicalRecord();
                    break;
                case 4:
                    return;
                default:
                    System.out.println("Invalid choice.");
            }
        }
    }

    private void createMedicalRecord() {
        System.out.println("\n--- Create Medical Record ---");
        System.out.print("Patient ID: ");
        String patientId = scanner.nextLine();
        System.out.print("Doctor ID: ");
        String doctorId = scanner.nextLine();
        System.out.print("Diagnosis: ");
        String diagnosis = scanner.nextLine();
        System.out.print("Treatment: ");
        String treatment = scanner.nextLine();
        System.out.print("Medications: ");
        String medications = scanner.nextLine();
        System.out.print("Notes: ");
        String notes = scanner.nextLine();
        
        MedicalRecord record = new MedicalRecord(patientId, doctorId, diagnosis, treatment, medications, notes);
        ehrManager.addRecord(record);
        System.out.println("Medical record created successfully! Record ID: " + record.getRecordId());
    }

    private void viewPatientRecords() {
        System.out.print("Enter Patient ID: ");
        String patientId = scanner.nextLine();
        ehrManager.displayPatientRecords(patientId);
    }

    private void updateMedicalRecord() {
        System.out.print("Enter Record ID: ");
        String recordId = scanner.nextLine();
        ehrManager.updateRecord(recordId, scanner);
    }

    // Billing Menu
    private void billingMenu() {
        while (true) {
            System.out.println("\n========== BILLING & INVOICING ==========");
            System.out.println("1. Create New Bill");
            System.out.println("2. View All Bills");
            System.out.println("3. View Patient Bills");
            System.out.println("4. Process Payment");
            System.out.println("5. Back to Main Menu");
            System.out.print("Enter your choice: ");
            
            int choice = getChoice();
            scanner.nextLine();
            
            switch (choice) {
                case 1:
                    createNewBill();
                    break;
                case 2:
                    billingManager.displayAllBills();
                    break;
                case 3:
                    viewPatientBills();
                    break;
                case 4:
                    processPayment();
                    break;
                case 5:
                    return;
                default:
                    System.out.println("Invalid choice.");
            }
        }
    }

    private void createNewBill() {
        System.out.println("\n--- Create New Bill ---");
        System.out.print("Patient ID: ");
        String patientId = scanner.nextLine();
        
        List<BillItem> items = new ArrayList<>();
        while (true) {
            System.out.print("Service/Item name (or 'done' to finish): ");
            String itemName = scanner.nextLine();
            if (itemName.equalsIgnoreCase("done")) break;
            
            System.out.print("Amount: $");
            double amount = scanner.nextDouble();
            scanner.nextLine();
            
            items.add(new BillItem(itemName, amount));
        }
        
        Bill bill = new Bill(patientId, items);
        billingManager.createBill(bill);
        System.out.println("Bill created successfully! Bill ID: " + bill.getBillId());
    }

    private void viewPatientBills() {
        System.out.print("Enter Patient ID: ");
        String patientId = scanner.nextLine();
        billingManager.displayPatientBills(patientId);
    }

    private void processPayment() {
        System.out.print("Enter Bill ID: ");
        String billId = scanner.nextLine();
        System.out.print("Payment amount: $");
        double amount = scanner.nextDouble();
        scanner.nextLine();
        System.out.print("Payment method: ");
        String method = scanner.nextLine();
        
        billingManager.processPayment(billId, amount, method);
    }

    // Inventory Menu
    private void inventoryMenu() {
        while (true) {
            System.out.println("\n========== INVENTORY MANAGEMENT ==========");
            System.out.println("1. Add New Item");
            System.out.println("2. View All Items");
            System.out.println("3. Update Stock");
            System.out.println("4. Search Item");
            System.out.println("5. Low Stock Alert");
            System.out.println("6. Back to Main Menu");
            System.out.print("Enter your choice: ");
            
            int choice = getChoice();
            scanner.nextLine();
            
            switch (choice) {
                case 1:
                    addInventoryItem();
                    break;
                case 2:
                    inventoryManager.displayAllItems();
                    break;
                case 3:
                    updateStock();
                    break;
                case 4:
                    searchInventoryItem();
                    break;
                case 5:
                    inventoryManager.showLowStockAlert();
                    break;
                case 6:
                    return;
                default:
                    System.out.println("Invalid choice.");
            }
        }
    }

    private void addInventoryItem() {
        System.out.println("\n--- Add New Inventory Item ---");
        System.out.print("Item ID: ");
        String itemId = scanner.nextLine();
        System.out.print("Item Name: ");
        String name = scanner.nextLine();
        System.out.print("Quantity: ");
        int quantity = scanner.nextInt();
        System.out.print("Unit Price: $");
        double price = scanner.nextDouble();
        scanner.nextLine();
        
        InventoryItem item = new InventoryItem(itemId, name, quantity, price);
        inventoryManager.addItem(item);
        System.out.println("Item added successfully!");
    }

    private void updateStock() {
        System.out.print("Enter Item ID: ");
        String itemId = scanner.nextLine();
        System.out.print("New Quantity: ");
        int quantity = scanner.nextInt();
        scanner.nextLine();
        
        inventoryManager.updateStock(itemId, quantity);
    }

    private void searchInventoryItem() {
        System.out.print("Enter Item ID or Name: ");
        String searchTerm = scanner.nextLine();
        inventoryManager.searchItem(searchTerm);
    }

    // Staff Menu
    private void staffMenu() {
        while (true) {
            System.out.println("\n========== STAFF MANAGEMENT ==========");
            System.out.println("1. Add New Staff");
            System.out.println("2. View All Staff");
            System.out.println("3. Search Staff");
            System.out.println("4. Update Staff Information");
            System.out.println("5. Back to Main Menu");
            System.out.print("Enter your choice: ");
            
            int choice = getChoice();
            scanner.nextLine();
            
            switch (choice) {
                case 1:
                    addNewStaff();
                    break;
                case 2:
                    staffManager.displayAllStaff();
                    break;
                case 3:
                    searchStaff();
                    break;
                case 4:
                    updateStaff();
                    break;
                case 5:
                    return;
                default:
                    System.out.println("Invalid choice.");
            }
        }
    }

    private void addNewStaff() {
        System.out.println("\n--- Add New Staff ---");
        System.out.print("Staff ID: ");
        String staffId = scanner.nextLine();
        System.out.print("Name: ");
        String name = scanner.nextLine();
        System.out.print("Position: ");
        String position = scanner.nextLine();
        System.out.print("Department: ");
        String department = scanner.nextLine();
        System.out.print("Salary: $");
        double salary = scanner.nextDouble();
        scanner.nextLine();
        
        Staff staff = new Staff(staffId, name, position, department, salary);
        staffManager.addStaff(staff);
        System.out.println("Staff added successfully!");
    }

    private void searchStaff() {
        System.out.print("Enter Staff ID or Name: ");
        String searchTerm = scanner.nextLine();
        staffManager.searchStaff(searchTerm);
    }

    private void updateStaff() {
        System.out.print("Enter Staff ID: ");
        String staffId = scanner.nextLine();
        staffManager.updateStaff(staffId, scanner);
    }

    private void generateReports() {
        System.out.println("\n========== REPORTS ==========");
        System.out.println("1. Patient Statistics");
        System.out.println("2. Appointment Summary");
        System.out.println("3. Financial Report");
        System.out.println("4. Inventory Report");
        System.out.println("5. Staff Report");
        System.out.print("Enter your choice: ");
        
        int choice = getChoice();
        
        switch (choice) {
            case 1:
                patientManager.generateStatistics();
                break;
            case 2:
                appointmentManager.generateSummary();
                break;
            case 3:
                billingManager.generateFinancialReport();
                break;
            case 4:
                inventoryManager.generateInventoryReport();
                break;
            case 5:
                staffManager.generateStaffReport();
                break;
            default:
                System.out.println("Invalid choice.");
        }
    }
}

// Patient Class
class Patient {
    private static int patientCounter = 1000;
    private String patientId;
    private String name;
    private int age;
    private String gender;
    private String phone;
    private String address;
    private String emergencyContact;
    private LocalDate registrationDate;

    public Patient(String name, int age, String gender, String phone, String address, String emergencyContact) {
        this.patientId = "P" + (++patientCounter);
        this.name = name;
        this.age = age;
        this.gender = gender;
        this.phone = phone;
        this.address = address;
        this.emergencyContact = emergencyContact;
        this.registrationDate = LocalDate.now();
    }

    // Getters and setters
    public String getPatientId() { return patientId; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
    public String getGender() { return gender; }
    public void setGender(String gender) { this.gender = gender; }
    public String getPhone() { return phone; }
    public void setPhone(String phone) { this.phone = phone; }
    public String getAddress() { return address; }
    public void setAddress(String address) { this.address = address; }
    public String getEmergencyContact() { return emergencyContact; }
    public void setEmergencyContact(String emergencyContact) { this.emergencyContact = emergencyContact; }

    public void displayInfo() {
        System.out.println("\n--- Patient Information ---");
        System.out.println("Patient ID: " + patientId);
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Gender: " + gender);
        System.out.println("Phone: " + phone);
        System.out.println("Address: " + address);
        System.out.println("Emergency Contact: " + emergencyContact);
        System.out.println("Registration Date: " + registrationDate);
    }
}

// Patient Manager Class
class PatientManager {
    private Map<String, Patient> patients;

    public PatientManager() {
        this.patients = new HashMap<>();
    }

    public void addPatient(Patient patient) {
        patients.put(patient.getPatientId(), patient);
    }

    public Patient searchPatient(String searchTerm) {
        // Search by ID first
        if (patients.containsKey(searchTerm)) {
            return patients.get(searchTerm);
        }
        
        // Search by name
        for (Patient patient : patients.values()) {
            if (patient.getName().toLowerCase().contains(searchTerm.toLowerCase())) {
                return patient;
            }
        }
        return null;
    }

    public void displayAllPatients() {
        if (patients.isEmpty()) {
            System.out.println("No patients registered.");
            return;
        }
        
        System.out.println("\n========== ALL PATIENTS ==========");
        for (Patient patient : patients.values()) {
            patient.displayInfo();
            System.out.println("================================");
        }
    }

    public void updatePatient(String patientId, Scanner scanner) {
        Patient patient = patients.get(patientId);
        if (patient == null) {
            System.out.println("Patient not found.");
            return;
        }

        System.out.println("Current information:");
        patient.displayInfo();
        
        System.out.println("\nUpdate information (press Enter to skip):");
        System.out.print("Name: ");
        String name = scanner.nextLine();
        if (!name.isEmpty()) patient.setName(name);
        
        System.out.print("Age: ");
        String ageStr = scanner.nextLine();
        if (!ageStr.isEmpty()) {
            try {
                patient.setAge(Integer.parseInt(ageStr));
            } catch (NumberFormatException e) {
                System.out.println("Invalid age format.");
            }
        }
        
        System.out.print("Phone: ");
        String phone = scanner.nextLine();
        if (!phone.isEmpty()) patient.setPhone(phone);
        
        System.out.print("Address: ");
        String address = scanner.nextLine();
        if (!address.isEmpty()) patient.setAddress(address);
        
        System.out.println("Patient information updated successfully!");
    }

    public void generateStatistics() {
        System.out.println("\n========== PATIENT STATISTICS ==========");
        System.out.println("Total Patients: " + patients.size());
        
        Map<String, Integer> genderCount = new HashMap<>();
        int totalAge = 0;
        
        for (Patient patient : patients.values()) {
            genderCount.put(patient.getGender(), 
                genderCount.getOrDefault(patient.getGender(), 0) + 1);
            totalAge += patient.getAge();
        }
        
        if (!patients.isEmpty()) {
            System.out.println("Average Age: " + (totalAge / patients.size()));
        }
        
        System.out.println("Gender Distribution:");
        for (Map.Entry<String, Integer> entry : genderCount.entrySet()) {
            System.out.println("  " + entry.getKey() + ": " + entry.getValue());
        }
    }
}

// Appointment Class
class Appointment {
    private static int appointmentCounter = 1000;
    private String appointmentId;
    private String patientId;
    private String doctorId;
    private LocalDateTime dateTime;
    private String purpose;
    private String status;

    public Appointment(String patientId, String doctorId, LocalDateTime dateTime, String purpose) {
        this.appointmentId = "A" + (++appointmentCounter);
        this.patientId = patientId;
        this.doctorId = doctorId;
        this.dateTime = dateTime;
        this.purpose = purpose;
        this.status = "Scheduled";
    }

    // Getters
    public String getAppointmentId() { return appointmentId; }
    public String getPatientId() { return patientId; }
    public String getDoctorId() { return doctorId; }
    public LocalDateTime getDateTime() { return dateTime; }
    public String getPurpose() { return purpose; }
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }

    public void displayInfo() {
        System.out.println("\n--- Appointment Information ---");
        System.out.println("Appointment ID: " + appointmentId);
        System.out.println("Patient ID: " + patientId);
        System.out.println("Doctor ID: " + doctorId);
        System.out.println("Date & Time: " + dateTime.format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm")));
        System.out.println("Purpose: " + purpose);
        System.out.println("Status: " + status);
    }
}

// Appointment Manager Class
class AppointmentManager {
    private Map<String, Appointment> appointments;

    public AppointmentManager() {
        this.appointments = new HashMap<>();
    }

    public void scheduleAppointment(Appointment appointment) {
        appointments.put(appointment.getAppointmentId(), appointment);
    }

    public void displayAllAppointments() {
        if (appointments.isEmpty()) {
            System.out.println("No appointments scheduled.");
            return;
        }
        
        System.out.println("\n========== ALL APPOINTMENTS ==========");
        for (Appointment appointment : appointments.values()) {
            appointment.displayInfo();
            System.out.println("=====================================");
        }
    }

    public void displayAppointmentsByDate(LocalDate date) {
        boolean found = false;
        System.out.println("\n========== APPOINTMENTS FOR " + date + " ==========");
        
        for (Appointment appointment : appointments.values()) {
            if (appointment.getDateTime().toLocalDate().equals(date)) {
                appointment.displayInfo();
                System.out.println("=====================================");
                found = true;
            }
        }
        
        if (!found) {
            System.out.println("No appointments found for this date.");
        }
    }

    public void cancelAppointment(String appointmentId) {
        Appointment appointment = appointments.get(appointmentId);
        if (appointment != null) {
            appointment.setStatus("Cancelled");
            System.out.println("Appointment cancelled successfully.");
        } else {
            System.out.println("Appointment not found.");
        }
    }

    public void generateSummary() {
        System.out.println("\n========== APPOINTMENT SUMMARY ==========");
        System.out.println("Total Appointments: " + appointments.size());
        
        Map<String, Integer> statusCount = new HashMap<>();
        for (Appointment appointment : appointments.values()) {
            statusCount.put(appointment.getStatus(), 
                statusCount.getOrDefault(appointment.getStatus(), 0) + 1);
        }
        
        System.out.println("Status Distribution:");
        for (Map.Entry<String, Integer> entry : statusCount.entrySet()) {
            System.out.println("  " + entry.getKey() + ": " + entry.getValue());
        }
    }
}

// Medical Record Class
class MedicalRecord {
    private static int recordCounter = 1000;
    private String recordId;
    private String patientId;
    private String doctorId;
    private LocalDate date;
    private String diagnosis;
    private String treatment;
    private String medications;
    private String notes;

    public MedicalRecord(String patientId, String doctorId, String diagnosis, 
                        String treatment, String medications, String notes) {
        this.recordId = "MR" + (++recordCounter);
        this.patientId = patientId;
        this.doctorId = doctorId;
        this.date = LocalDate.now();
        this.diagnosis = diagnosis;
        this.treatment = treatment;
        this.medications = medications;
        this.notes = notes;
    }

    // Getters and setters
    public String getRecordId() { return recordId; }
    public String getPatientId() { return patientId; }
    public String getDoctorId() { return doctorId; }
    public LocalDate getDate() { return date; }
    public String getDiagnosis() { return diagnosis; }
    public void setDiagnosis(String diagnosis) { this.diagnosis = diagnosis; }
    public String getTreatment() { return treatment; }
    public void setTreatment(String treatment) { this.treatment = treatment; }
    public String getMedications() { return medications; }
    public void setMedications(String medications) { this.medications = medications; }
    public String getNotes() { return notes; }
    public void setNotes(String notes) { this.notes = notes; }

    public void displayInfo() {
        System.out.println("\n--- Medical Record ---");
        System.out.println("Record ID: " + recordId);
        System.out.println("Patient ID: " + patientId);
        System.out.println("Doctor ID: " + doctorId);
        System.out.println("Date: " + date);
        System.out.println("Diagnosis: " + diagnosis);
        System.out.println("Treatment: " + treatment);
        System.out.println("Medications: " + medications);
        System.out.println("Notes: " + notes);
    }
}

// EHR Manager Class
class EHRManager {
    private Map<String, MedicalRecord> records;

    public EHRManager() {
        this.records = new HashMap<>();
    }

    public void addRecord(MedicalRecord record) {
        records.put(record.getRecordId(), record);
    }

    public void displayPatientRecords(String patientId) {
        boolean found = false;
        System.out.println("\n========== MEDICAL RECORDS FOR PATIENT " + patientId + " ==========");
        
        for (MedicalRecord record : records.values()) {
            if (record.getPatientId().equals(patientId)) {
                record.displayInfo();
                System.out.println("=======================================");
                found = true;
            }
        }
        
        if (!found) {
            System.out.println("No medical records found for this patient.");
        }
    }

    public void updateRecord(String recordId, Scanner scanner) {
        MedicalRecord record = records.get(recordId);
        if (record == null) {
            System.out.println("Medical record not found.");
            return;
        }

        System.out.println("Current record:");
        record.displayInfo();
        
        System.out.println("\nUpdate information (press Enter to skip):");
        System.out.print("Diagnosis: ");
        String diagnosis = scanner.nextLine();
        if (!diagnosis.isEmpty()) record.setDiagnosis(diagnosis);
        
        System.out.print("Treatment: ");
        String treatment = scanner.nextLine();
        if (!treatment.isEmpty()) record.setTreatment(treatment);
        
        System.out.print("Medications: ");
        String medications = scanner.nextLine();
        if (!medications.isEmpty()) record.setMedications(medications);
        
        System.out.print("Notes: ");
        String notes = scanner.nextLine();
        if (!notes.isEmpty()) record.setNotes(notes);
        
        System.out.println("Medical record updated successfully!");
    }
}

// Bill Item Class
class BillItem {
    private String itemName;
    private double amount;

    public BillItem(String itemName, double amount) {
        this.itemName = itemName;
        this.amount = amount;
    }

    public String getItemName() { return itemName; }
    public double getAmount() { return amount; }

    public void displayInfo() {
        System.out.printf("  %-30s $%.2f%n", itemName, amount);
    }
}

// Bill Class
class Bill {
    private static int billCounter = 1000;
    private String billId;
    private String patientId;
    private List<BillItem> items;
    private double totalAmount;
    private LocalDate billDate;
    private String status;
    private double paidAmount;

    public Bill(String patientId, List<BillItem> items) {
        this.billId = "B" + (++billCounter);
        this.patientId = patientId;
        this.items = new ArrayList<>(items);
        this.billDate = LocalDate.now();
        this.status = "Pending";
        this.paidAmount = 0.0;
        calculateTotal();
    }

    private void calculateTotal() {
        this.totalAmount = items.stream().mapToDouble(BillItem::getAmount).sum();
    }

    // Getters and setters
    public String getBillId() { return billId; }
    public String getPatientId() { return patientId; }
    public List<BillItem> getItems() { return items; }
    public double getTotalAmount() { return totalAmount; }
    public LocalDate getBillDate() { return billDate; }
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }
    public double getPaidAmount() { return paidAmount; }
    public void setPaidAmount(double paidAmount) { this.paidAmount = paidAmount; }

    public double getBalanceAmount() {
        return totalAmount - paidAmount;
    }

    public void displayInfo() {
        System.out.println("\n--- Bill Information ---");
        System.out.println("Bill ID: " + billId);
        System.out.println("Patient ID: " + patientId);
        System.out.println("Bill Date: " + billDate);
        System.out.println("Items:");
        for (BillItem item : items) {
            item.displayInfo();
        }
        System.out.printf("Total Amount: $%.2f%n", totalAmount);
        System.out.printf("Paid Amount: $%.2f%n", paidAmount);
        System.out.printf("Balance: $%.2f%n", getBalanceAmount());
        System.out.println("Status: " + status);
    }
}

// Billing Manager Class
class BillingManager {
    private Map<String, Bill> bills;

    public BillingManager() {
        this.bills = new HashMap<>();
    }

    public void createBill(Bill bill) {
        bills.put(bill.getBillId(), bill);
    }

    public void displayAllBills() {
        if (bills.isEmpty()) {
            System.out.println("No bills found.");
            return;
        }
        
        System.out.println("\n========== ALL BILLS ==========");
        for (Bill bill : bills.values()) {
            bill.displayInfo();
            System.out.println("===============================");
        }
    }

    public void displayPatientBills(String patientId) {
        boolean found = false;
        System.out.println("\n========== BILLS FOR PATIENT " + patientId + " ==========");
        
        for (Bill bill : bills.values()) {
            if (bill.getPatientId().equals(patientId)) {
                bill.displayInfo();
                System.out.println("===============================");
                found = true;
            }
        }
        
        if (!found) {
            System.out.println("No bills found for this patient.");
        }
    }

    public void processPayment(String billId, double amount, String method) {
        Bill bill = bills.get(billId);
        if (bill == null) {
            System.out.println("Bill not found.");
            return;
        }

        double newPaidAmount = bill.getPaidAmount() + amount;
        bill.setPaidAmount(newPaidAmount);
        
        if (newPaidAmount >= bill.getTotalAmount()) {
            bill.setStatus("Paid");
        } else {
            bill.setStatus("Partially Paid");
        }
        
        System.out.printf("Payment of $%.2f processed successfully using %s.%n", amount, method);
        System.out.printf("Remaining balance: $%.2f%n", bill.getBalanceAmount());
    }

    public void generateFinancialReport() {
        System.out.println("\n========== FINANCIAL REPORT ==========");
        System.out.println("Total Bills: " + bills.size());
        
        double totalRevenue = 0;
        double totalPaid = 0;
        double totalPending = 0;
        
        Map<String, Integer> statusCount = new HashMap<>();
        
        for (Bill bill : bills.values()) {
            totalRevenue += bill.getTotalAmount();
            totalPaid += bill.getPaidAmount();
            totalPending += bill.getBalanceAmount();
            
            statusCount.put(bill.getStatus(), 
                statusCount.getOrDefault(bill.getStatus(), 0) + 1);
        }
        
        System.out.printf("Total Revenue: $%.2f%n", totalRevenue);
        System.out.printf("Total Collected: $%.2f%n", totalPaid);
        System.out.printf("Total Pending: $%.2f%n", totalPending);
        
        System.out.println("Bill Status Distribution:");
        for (Map.Entry<String, Integer> entry : statusCount.entrySet()) {
            System.out.println("  " + entry.getKey() + ": " + entry.getValue());
        }
    }
}

// Inventory Item Class
class InventoryItem {
    private String itemId;
    private String itemName;
    private int quantity;
    private double unitPrice;
    private int minStockLevel;

    public InventoryItem(String itemId, String itemName, int quantity, double unitPrice) {
        this.itemId = itemId;
        this.itemName = itemName;
        this.quantity = quantity;
        this.unitPrice = unitPrice;
        this.minStockLevel = 10; // Default minimum stock level
    }

    // Getters and setters
    public String getItemId() { return itemId; }
    public String getItemName() { return itemName; }
    public void setItemName(String itemName) { this.itemName = itemName; }
    public int getQuantity() { return quantity; }
    public void setQuantity(int quantity) { this.quantity = quantity; }
    public double getUnitPrice() { return unitPrice; }
    public void setUnitPrice(double unitPrice) { this.unitPrice = unitPrice; }
    public int getMinStockLevel() { return minStockLevel; }
    public void setMinStockLevel(int minStockLevel) { this.minStockLevel = minStockLevel; }

    public boolean isLowStock() {
        return quantity <= minStockLevel;
    }

    public double getTotalValue() {
        return quantity * unitPrice;
    }

    public void displayInfo() {
        System.out.println("\n--- Inventory Item ---");
        System.out.println("Item ID: " + itemId);
        System.out.println("Item Name: " + itemName);
        System.out.println("Quantity: " + quantity);
        System.out.printf("Unit Price: $%.2f%n", unitPrice);
        System.out.printf("Total Value: $%.2f%n", getTotalValue());
        System.out.println("Min Stock Level: " + minStockLevel);
        System.out.println("Stock Status: " + (isLowStock() ? "LOW STOCK" : "OK"));
    }
}

// Inventory Manager Class
class InventoryManager {
    private Map<String, InventoryItem> inventory;

    public InventoryManager() {
        this.inventory = new HashMap<>();
    }

    public void addItem(InventoryItem item) {
        inventory.put(item.getItemId(), item);
    }

    public void displayAllItems() {
        if (inventory.isEmpty()) {
            System.out.println("No items in inventory.");
            return;
        }
        
        System.out.println("\n========== INVENTORY ITEMS ==========");
        for (InventoryItem item : inventory.values()) {
            item.displayInfo();
            System.out.println("====================================");
        }
    }

    public void updateStock(String itemId, int newQuantity) {
        InventoryItem item = inventory.get(itemId);
        if (item != null) {
            item.setQuantity(newQuantity);
            System.out.println("Stock updated successfully for " + item.getItemName());
            if (item.isLowStock()) {
                System.out.println("WARNING: Stock is now below minimum level!");
            }
        } else {
            System.out.println("Item not found.");
        }
    }

    public void searchItem(String searchTerm) {
        InventoryItem item = inventory.get(searchTerm);
        if (item != null) {
            item.displayInfo();
            return;
        }
        
        // Search by name
        boolean found = false;
        for (InventoryItem invItem : inventory.values()) {
            if (invItem.getItemName().toLowerCase().contains(searchTerm.toLowerCase())) {
                invItem.displayInfo();
                found = true;
            }
        }
        
        if (!found) {
            System.out.println("Item not found.");
        }
    }

    public void showLowStockAlert() {
        System.out.println("\n========== LOW STOCK ALERT ==========");
        boolean hasLowStock = false;
        
        for (InventoryItem item : inventory.values()) {
            if (item.isLowStock()) {
                System.out.printf("ALERT: %s (ID: %s) - Current Stock: %d, Min Level: %d%n",
                    item.getItemName(), item.getItemId(), item.getQuantity(), item.getMinStockLevel());
                hasLowStock = true;
            }
        }
        
        if (!hasLowStock) {
            System.out.println("All items are adequately stocked.");
        }
    }

    public void generateInventoryReport() {
        System.out.println("\n========== INVENTORY REPORT ==========");
        System.out.println("Total Items: " + inventory.size());
        
        double totalValue = 0;
        int lowStockItems = 0;
        
        for (InventoryItem item : inventory.values()) {
            totalValue += item.getTotalValue();
            if (item.isLowStock()) {
                lowStockItems++;
            }
        }
        
        System.out.printf("Total Inventory Value: $%.2f%n", totalValue);
        System.out.println("Low Stock Items: " + lowStockItems);
        System.out.printf("Average Item Value: $%.2f%n", 
            inventory.isEmpty() ? 0 : totalValue / inventory.size());
    }
}

// Staff Class
class Staff {
    private String staffId;
    private String name;
    private String position;
    private String department;
    private double salary;
    private LocalDate joinDate;
    private String status;

    public Staff(String staffId, String name, String position, String department, double salary) {
        this.staffId = staffId;
        this.name = name;
        this.position = position;
        this.department = department;
        this.salary = salary;
        this.joinDate = LocalDate.now();
        this.status = "Active";
    }

    // Getters and setters
    public String getStaffId() { return staffId; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getPosition() { return position; }
    public void setPosition(String position) { this.position = position; }
    public String getDepartment() { return department; }
    public void setDepartment(String department) { this.department = department; }
    public double getSalary() { return salary; }
    public void setSalary(double salary) { this.salary = salary; }
    public LocalDate getJoinDate() { return joinDate; }
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }

    public void displayInfo() {
        System.out.println("\n--- Staff Information ---");
        System.out.println("Staff ID: " + staffId);
        System.out.println("Name: " + name);
        System.out.println("Position: " + position);
        System.out.println("Department: " + department);
        System.out.printf("Salary: $%.2f%n", salary);
        System.out.println("Join Date: " + joinDate);
        System.out.println("Status: " + status);
    }
}

// Staff Manager Class

class StaffManager {
    private Map<String, Staff> staff;

    public StaffManager() {
        this.staff = new HashMap<>();
    }

    public void addStaff(Staff staffMember) {
        staff.put(staffMember.getStaffId(), staffMember);
    }

    public void displayAllStaff() {
        if (staff.isEmpty()) {
            System.out.println("No staff members found.");
            return;
        }
        
        System.out.println("\n========== ALL STAFF MEMBERS ==========");
        for (Staff staffMember : staff.values()) {
            staffMember.displayInfo();
            System.out.println("======================================");
        }
    }

    public void searchStaff(String searchTerm) {
        Staff staffMember = staff.get(searchTerm);
        if (staffMember != null) {
            staffMember.displayInfo();
            return;
        }
        
        // Search by name
        boolean found = false;
        for (Staff member : staff.values()) {
            if (member.getName().toLowerCase().contains(searchTerm.toLowerCase())) {
                member.displayInfo();
                found = true;
            }
        }
        
        if (!found) {
            System.out.println("Staff member not found.");
        }
    }

    public void updateStaff(String staffId, Scanner scanner) {
        Staff staffMember = staff.get(staffId);
        if (staffMember == null) {
            System.out.println("Staff member not found.");
            return;
        }

        System.out.println("Current information:");
        staffMember.displayInfo();
        
        System.out.println("\nUpdate information (press Enter to skip):");
        System.out.print("Name: ");
        String name = scanner.nextLine();
        if (!name.isEmpty()) staffMember.setName(name);
        
        System.out.print("Position: ");
        String position = scanner.nextLine();
        if (!position.isEmpty()) staffMember.setPosition(position);
        
        System.out.print("Department: ");
        String department = scanner.nextLine();
        if (!department.isEmpty()) staffMember.setDepartment(department);
        
        System.out.print("Salary: ");
        String salaryStr = scanner.nextLine();
        if (!salaryStr.isEmpty()) {
            try {
                staffMember.setSalary(Double.parseDouble(salaryStr));
            } catch (NumberFormatException e) {
                System.out.println("Invalid salary format.");
            }
        }
        
        System.out.println("Staff information updated successfully!");
    }

    public void generateStaffReport() {
        System.out.println("\n========== STAFF REPORT ==========");
        System.out.println("Total Staff Members: " + staff.size());
        
        Map<String, Integer> departmentCount = new HashMap<>();
        Map<String, Integer> positionCount = new HashMap<>();
        double totalSalary = 0;
        
        for (Staff member : staff.values()) {
            departmentCount.put(member.getDepartment(), 
                departmentCount.getOrDefault(member.getDepartment(), 0) + 1);
            positionCount.put(member.getPosition(), 
                positionCount.getOrDefault(member.getPosition(), 0) + 1);
            totalSalary += member.getSalary();
        }
        
        System.out.printf("Total Salary Expense: $%.2f%n", totalSalary);
        if (!staff.isEmpty()) {
            System.out.printf("Average Salary: $%.2f%n", totalSalary / staff.size());
        }
        
        System.out.println("Department Distribution:");
        for (Map.Entry<String, Integer> entry : departmentCount.entrySet()) {
            System.out.println("  " + entry.getKey() + ": " + entry.getValue());
        }
        
        System.out.println("Position Distribution:");
        for (Map.Entry<String, Integer> entry : positionCount.entrySet()) {
            System.out.println("  " + entry.getKey() + ": " + entry.getValue());
        }
    }
}
