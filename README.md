//# DSA-Blood-Donor-Management-System
//A robust C++ system designed to streamline blood donation processes using efficient Data Structures like, Linked Lists, and Queues //for real-time donor-patient matching.
#include <iostream>
#include <string>
struct Donor {
    string name;
    int age;
    string contact;
    string bloodGroup;
    int donations;
    Donor* next;
};
struct BSTNode {
    string name;
    string bloodGroup;
    BSTNode* left;
    BSTNode* right;
};

struct BloodRequest {
    string patientName;
        string bloodGroup;
    int units;
};

class BloodDonationSystem {
private:
    Donor* head;
    queue<BloodRequest> requestQueue;
    stack<string> actionStack;
    BSTNode* root;
    string bloodGroups[8] = {"A+", "A-", "B+", "B-", "AB+", "AB-", "O+", "O-"};
    int stock[8];
};
public:
    BloodDonationSystem() {
        head = NULL;
        root = NULL;
        for (int i = 0; i < 8; i++) stock[i] = 0;
    }

    int getIndex(string bg) {
        for (int i = 0; i < 8; i++)
            if (bloodGroups[i] == bg) return i;
        return -1;
            void addDonor(string name, int age, string contact, string bg, int initialDonation = 0) {
        Donor* newDonor = new Donor;
        newDonor->name = name;
        newDonor->age = age;
        newDonor->contact = contact;
        newDonor->bloodGroup = bg;
        newDonor->donations = 0;
        newDonor->next = NULL;

    }
            if (head == NULL)
		 head = newDonor;
        else {
            Donor* temp = head;
            while (temp->next != NULL)
			 temp = temp->next;
            temp->next = newDonor;
        }
        root = insertBST(root, name, bg);
        actionStack.push("Added donor: " + name);

        // If initial donation is given, update donor and stock
        if (initialDonation > 0) {
            donateBlood(name, bg, initialDonation);
        }
    }
	    BSTNode* insertBST(BSTNode* node, string name, string bg) {
        if (node == NULL) {
            BSTNode* newNode = new BSTNode;
            newNode->name = name;
            newNode->bloodGroup = bg;
            newNode->left = NULL;
            newNode->right = NULL;
            return newNode;
        }
        if (name > node->name)
            node->left = insertBST(node->left, name, bg);
        else
            node->right = insertBST(node->right, name, bg);
        return node;
    }
    void searchDonorBST(string name) {
        BSTNode* temp = root;
        while (temp != NULL) {
            if (temp->name == name) {
                cout << "\nDonor Found: " << temp->name
                     << " | Blood Group: " << temp->bloodGroup << endl;
                return;
            }
            temp = (name < temp->name) ? temp->left : temp->right;
        }
        cout << "\nDonor not found.\n";
    }

    void makeRequest(string patient, string bg, int units) {
        BloodRequest req;
        req.patientName = patient;
        req.bloodGroup = bg;
        req.units = units;
        requestQueue.push(req);
        actionStack.push("Blood requested by " + patient);
        cout << "\nBlood request added to queue for patient: " << patient << endl;
    }
	  void processRequest(){
        if (requestQueue.empty()){
            cout <<"\nNo requests in queue.\n";
            return;
        }
        BloodRequest req=requestQueue.front();
        int index=getIndex(req.bloodGroup);
        cout<<"\nProcessing Request:\n";
        cout<<"Patient: "<< req.patientName
            <<"| Blood Group: " << req.bloodGroup
            <<"| Units: "<<req.units<< endl;
        if (index >= 0 && stock[index] >= req.units) {
            stock[index] -= req.units;
            requestQueue.pop();
            cout <<"Request fulfilled successfully!\n";
        } else {
            cout <<"Insufficient stock for this blood group.\n";
        }
    }
 void donateBlood(string name, string bg, int units) {
        Donor* temp = head;
        while (temp != NULL) {
            if (temp->name == name && temp->bloodGroup == bg) {
                temp->donations += 1;
                int index = getIndex(bg);
                if (index >= 0) stock[index] += units;
                actionStack.push("Donation by " + name);
                return;
            }
            temp = temp->next;
        }
        cout << "\nDonor not found in records.\n";
    }
    void showStock() {
        cout << "\n------ BLOOD STOCK ------\n";
        for (int i = 0; i < 8; i++) {
            cout << bloodGroups[i] << " : " << stock[i] << " units\n";
        }
    }

    void showDonors() {
        Donor* temp = head;
        if (temp == NULL) {
            cout << "\nNo donors found.\n";
            return;
        }
        cout << "\n------ DONOR LIST ------\n";
        while (temp != NULL) {
            cout << "Name: " << temp->name
                 << " | Age: " << temp->age
                 << " | Contact: " << temp->contact
                 << " | Blood Group: " << temp->bloodGroup
                 << " | Donations: " << temp->donations << endl;
            temp = temp->next;
        }
    }

    void undoAction() {
        if (actionStack.empty()) {
            cout << "\nNo actions to undo.\n";
            return;
        }
        cout << "\nUndoing: " << actionStack.top() << endl;
        actionStack.pop();
    }

    void inorderBST(BSTNode* node) {
        if (node == NULL) return;
        inorderBST(node->left);
        cout << "Name: " << node->name << " | Blood Group: " << node->bloodGroup << endl;
        inorderBST(node->right);
    }
    void showSortedDonors() {
        cout << "\n------ SORTED DONORS (BST) ------\n";
        inorderBST(root);
    }

    // ===================== SIMPLE SORTING METHODS =====================
    void sortDonorNames() {
        int count = 0;
        Donor* temp = head;
        while(temp)
		 { count++; temp = temp->next; }

        string names[count];
        temp = head;
        for(int i=0; i<count; i++)
		{ names[i] = temp->name; temp = temp->next; }

        for(int i=1; i<count; i++){
            string key = names[i];
            int j = i-1;
            while(j>=0 && names[j] > key){
                names[j+1] = names[j];
                j--;
            }
            names[j+1] = key;
        }

        cout << "\nDonors Sorted Alphabetically:\n";
        for(int i=0; i<count; i++) cout << names[i] << endl;
    }
    void sortByDonations() {
        int count = 0;
        Donor* temp = head;
        while(temp){ count++; temp = temp->next; }

        Donor* donors[count];
        temp = head;
        for(int i=0;i<count;i++){ donors[i] = temp; temp=temp->next; }

        for(int i=0;i<count-1;i++){
            for(int j=0;j<count-i-1;j++){
                if(donors[j]->donations < donors[j+1]->donations)
                    swap(donors[j], donors[j+1]);
            }
        }

        cout << "\nDonors Sorted by Donations:\n";
        for(int i=0;i<count;i++)
            cout << donors[i]->name << " | Donations: " << donors[i]->donations << endl;
    }
    void sortStock() {
        int n = 8;
        int s[n];
        for(int i=0;i<n;i++) s[i]=stock[i];

        for(int i=0;i<n-1;i++){
            int minIdx=i;
            for(int j=i+1;j<n;j++) if(s[j]<s[minIdx]) minIdx=j;
            swap(s[i],s[minIdx]);
        }

        cout << "\nBlood Stock Sorted (Lowest to Highest):\n";
        for(int i=0;i<n;i++) cout << bloodGroups[i] << " : " << s[i] << " units\n";
    }

    // ========================= MENU ==========================
    void menu() {
        int choice;
        do {
            cout << "\n===== BLOOD DONATION MANAGEMENT SYSTEM =====\n";
            cout << "1. Add Donor\n2. Show Donors\n3. Search Donor (BST)\n";
            cout << "4. Donate Blood\n5. Show Blood Stock\n6. Make Blood Request\n";
            cout << "7. Process Request\n8. Undo Last Action\n9. Show Sorted Donors (BST)\n";
            cout << "10. Sort Donor Names Alphabetically\n";
            cout << "11. Sort Donors by Donations\n12. Sort Blood Stock\n0. Exit\n";
            cout << "Enter your choice: ";
            cin >> choice; cin.ignore();
			            switch(choice){
                case 1:{
                    string name, contact, bg;
					 int age;
                    cout << "Enter Name: ";
					 getline(cin,name);
                    cout << "Enter Age: ";
					 cin >> age;
					  cin.ignore();
                    cout << "Enter Contact: "; 
					getline(cin,contact);
                    cout << "Enter Blood Group: ";
					 getline(cin,bg);
                    addDonor(name,age,contact,bg); 
					break;
                }
                case 2: showDonors(); break;
                case 3:{ string name;
				 cout << "Enter Donor Name: "; 
				 getline(cin,name); 
				 searchDonorBST(name); 
				 break;}
                case 4:{
                    string name,bg; int units;
                    cout << "Enter Donor Name: "; 
					getline(cin,name);
                    cout << "Enter Blood Group: ";
					 getline(cin,bg);
                    cout << "Enter Units Donated: "; 
					cin >> units;
					 cin.ignore();
                    donateBlood(name,bg,units); 
					break;
                }
                case 5: showStock(); break;
                case 6:{
                    string patient,bg; int units;
                    cout << "Enter Patient Name: "; 
					getline(cin,patient);
                    cout << "Enter Blood Group: "; 
					getline(cin,bg);
                    cout << "Enter Units Required: "; 
					cin >> units;
					 cin.ignore();
                    makeRequest(patient,bg,units);
					 break;
                }
                case 7: processRequest(); break;
                case 8: undoAction(); break;
                case 9: showSortedDonors(); break;
                case 10: sortDonorNames(); break;
                case 11: sortByDonations(); break;
                case 12: sortStock(); break;
                case 0: cout << "\nExiting Program...\n"; break;
                default: cout << "\nInvalid choice. Try again.\n";
            }
        }while(choice!=0);
    }
};

int main() {
    BloodDonationSystem system;

    // Predefined donors with initial stock
    system.addDonor("Zohran", 29, "555-1234", "A+", 5);
    system.addDonor("Farrukh", 35, "555-5678", "O+", 3);
    system.addDonor("Ali", 42, "555-8765", "B-", 2);
    system.addDonor("ahmed", 30, "555-4321", "AB+", 4);
    system.addDonor("Haseeb", 38, "555-9876", "O-", 6);

    // Interactive menu
    system.menu();
    return 0;
}











