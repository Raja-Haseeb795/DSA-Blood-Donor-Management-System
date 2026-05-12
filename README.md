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








