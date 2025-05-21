#include <iostream>
#include <string>  
using namespace std;

// Node structure for child linked lists
struct ChildNode {
    int rollNo;
    string name;
    float result;
    ChildNode* next;

    ChildNode(int rollNo, string name, float result) {
        this->rollNo = rollNo;
        this->name = name;
        this->result = result;
        this->next = NULL;
    }
};

// Child linked list class
class ChildLinkedList {
public:
    ChildNode* head;

    ChildLinkedList() : head(NULL) {}

    // Function to add a node to the child linked list
    void addNode(int rollNo, string name, float result) {
        ChildNode* newNode = new ChildNode(rollNo, name, result);

        if (head == NULL) {
            head = newNode;
        } else {
            ChildNode* temp = head;
            while (temp->next != NULL) {
                temp = temp->next;
            }
            temp->next = newNode;
        }
    }

    // Function to display the child linked list
    void displayList() {
        ChildNode* temp = head;
        while (temp != NULL) {
            cout << "expenses: " << temp->rollNo << ", Name: " << temp->name << ", persentage: " << temp->result <<endl;
            temp = temp->next;
        }
    }

    // Function to delete a node from the child linked list
    void deleteNode(int rollNo) {
        if (head == NULL) return;

        // If the node to be deleted is the head
        if (head->rollNo == rollNo) {
            ChildNode* temp = head;
            head = head->next;
            delete temp;
            return;
        }

        ChildNode* temp = head;
        while (temp->next != NULL && temp->next->rollNo != rollNo) {
            temp = temp->next;
        }

        // If the node wasn't found
        if (temp->next == NULL) {
            cout << "Node with expense " << rollNo << " not found." <<endl;
            return;
        }

        // Node to be deleted is found
        ChildNode* toDelete = temp->next;
        temp->next = temp->next->next;
        delete toDelete;
    }

    // Function to update a node's details
    void updateNode(int rollNo, string newName, float newResult) {
        ChildNode* temp = head;
        while (temp != NULL) {
            if (temp->rollNo == rollNo) {
                temp->name = newName;
                temp->result = newResult;
                return;
            }
            temp = temp->next;
        }
        cout << "Node with roll number " << rollNo << " not found." << endl;
    }

    // Function to compare two nodes by result
    void compareNodes(int rollNo1, int rollNo2) {
        ChildNode* node1 = NULL, * node2 = NULL;
        ChildNode* temp = head;

        while (temp != NULL) {
            if (temp->rollNo == rollNo1) node1 = temp;
            if (temp->rollNo == rollNo2) node2 = temp;
            temp = temp->next;
        }

        if (node1 == NULL) {
            cout << "Node with expense " << rollNo1 << " not found." << endl;
            return;
        }

        if (node2 == NULL) {
            cout << "Node with expense " << rollNo2 << " not found." << endl;
            return;
        }

        if (node1->result > node2->result) {
            cout<<node1->name <<" has a higher expense (" << node1->result << ") than " << node2->name << " (" << node2->result << ")." <<endl;
        } else if(node1->result < node2->result) {
            cout<<node2->name <<" has a higher expense (" << node2->result << ") than " << node1->name << " (" << node1->result << ")." <<endl;
        } else {
            cout<<node1->name <<" and " << node2->name << " have the same expense (" << node1->result << ")." << endl;
        }
    }
};

// Node structure for master linked list
struct MasterNode {
    ChildLinkedList* childList;
    MasterNode* next;

    MasterNode(ChildLinkedList* childList) {
        this->childList = childList;
        this->next = NULL;
    }
};

// Master linked list class
class MasterLinkedList {
public:
    MasterNode* head;

    MasterLinkedList() : head(NULL) {}

    // Function to add a child linked list to the master linked list
    void addChildList(ChildLinkedList* childList) {
        MasterNode* newNode = new MasterNode(childList);

        if (head == NULL) {
            head = newNode;
        } else {
            MasterNode* temp = head;
            while (temp->next != NULL) {
                temp = temp->next;
            }
            temp->next = newNode;
        }
    }

    // Function to display the master linked list
    void displayMasterList() {
        MasterNode* temp = head;
        int i = 1;
        while (temp != NULL) {
            std::cout << "Costomer Linked List " << i << ":" << std::endl;
            temp->childList->displayList();
            std::cout << std::endl;
            temp = temp->next;
            i++;
        }
    }
};

int main() {
    MasterLinkedList masterList;

    // Create Child Linked Lists
    ChildLinkedList* childList1 = new ChildLinkedList();
    childList1->addNode(10000, "ali", 85.5);
    childList1->addNode(20000, "humza", 90.2);

    ChildLinkedList* childList2 = new ChildLinkedList();
    childList2->addNode(30000, "bilal", 78.9);
    childList2->addNode(40000, "hamna", 92.1);

    ChildLinkedList* childList3 = new ChildLinkedList();
    childList3->addNode(50000, "aliya", 88.7);
    childList3->addNode(60000, "jannat", 95.6);

    // Add child linked lists to master list
    masterList.addChildList(childList1);
    masterList.addChildList(childList2);
    masterList.addChildList(childList3);

    // Display the original lists
    masterList.displayMasterList();

    // Update a node
    childList1->updateNode(10000, "Ali Updated", 91.0);

    // Delete a node
    childList2->deleteNode(30000);

    // Compare nodes
    childList3->compareNodes(50000, 60000);

    // Display updated lists
    cout << "\nUpdated Master List:\n";
    masterList.displayMasterList();

    return 0;
}
