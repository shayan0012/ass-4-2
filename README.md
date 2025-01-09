#include <iostream>//hellooooooow
#include <fstream>
#include <vector>
#include <string>

using namespace std;

struct Patient
{
    int id;
    string name;
    int age;
    string disease;
    int roomNumber;
};

class HospitalManagement
{
private:
    vector<Patient> patients;

public:
    void addPatient();
    void searchPatient(int id);
    void displayPatients();
    void saveToFile();
    void loadFromFile();
};

void HospitalManagement::addPatient()
{
    Patient newPatient;
    cout << "Enter Patient ID: ";
    cin >> newPatient.id;
    cout << "Enter Name: ";
    cin.ignore();
    getline(cin, newPatient.name);
    cout << "Enter Age: ";
    cin >> newPatient.age;
    cout << "Enter Disease: ";
    cin.ignore();
    getline(cin, newPatient.disease);
    cout << "Enter Room Number: ";
    cin >> newPatient.roomNumber;

    patients.push_back(newPatient);
    cout << "Patient added successfully!\n";
}

void HospitalManagement::searchPatient(int id)
{
    for (const auto& patient : patients)
    {
        if (patient.id == id)
        {
            cout << "Patient Found: \n";
            cout << "ID: " << patient.id << ", Name: " << patient.name
                 << ", Age: " << patient.age << ", Disease: " << patient.disease
                 << ", Room Number: " << patient.roomNumber << endl;
            return;
        }
    }
    cout << "Patient with ID " << id << " not found.\n";
}

void HospitalManagement::displayPatients()
{
    if (patients.empty())
    {
        cout << "No patients admitted.\n";
        return;
    }
    cout << "List of Admitted Patients:\n";
    for (const auto& patient : patients)
    {
        cout << "ID: " << patient.id << ", Name: " << patient.name
             << ", Age: " << patient.age << ", Disease: " << patient.disease
             << ", Room Number: " << patient.roomNumber << endl;
    }
}

void HospitalManagement::saveToFile()
{
    ofstream outFile("patients.txt");
    for (const auto& patient : patients)
    {
        outFile << patient.id << "\n" << patient.name << "\n"
                << patient.age << "\n" << patient.disease << "\n"
                << patient.roomNumber << "\n";
    }
    outFile.close();
    cout << "Patient records saved to file.\n";
}

void HospitalManagement::loadFromFile()
{
    ifstream inFile("patients.txt");
    if (!inFile)
    {
        cout << "No records found.\n";
        return;
    }
    Patient patient;
    while (inFile >> patient.id)
    {
        inFile.ignore();
        getline(inFile, patient.name);
        inFile >> patient.age;
        inFile.ignore();
        getline(inFile, patient.disease);
        inFile >> patient.roomNumber;
        patients.push_back(patient);
    }
    inFile.close();
    cout << "Patient records loaded from file.\n";
}

int main()
{
    HospitalManagement hm;
    hm.loadFromFile();

    int choice;
    do {
        cout << "\nHospital Patient Management System\n";
        cout << "1. Add Patient\n";
        cout << "2. Search Patient by ID\n";
        cout << "3. Display All Patients\n";
        cout << "4. Save Records to File\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
            case 1:
                hm.addPatient();
                break;
            case 2: {
                int id;
                cout << "Enter Patient ID to search: ";
                cin >> id;
                hm.searchPatient(id);
                break;
            }
            case 3:
                hm.displayPatients();
                break;
            case 4:
                hm.saveToFile();
                break;
            case 5:
                cout << "Exiting the system. Goodbye!\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 5);

    return 0;
}
