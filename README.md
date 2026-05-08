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



