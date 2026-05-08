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


