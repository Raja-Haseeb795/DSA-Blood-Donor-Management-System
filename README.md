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








