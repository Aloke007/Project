# Project
/*
 * Assignment No: 1
 * Title: Linear Search and Binary Search on Student Roll Numbers
 */

#include <stdio.h>

int linearSearch(int arr[], int n, int key) {
    for (int i = 0; i < n; i++) {
        if (arr[i] == key) return i;
    }
    return -1;
}

int binarySearch(int arr[], int n, int key) {
    int low = 0, high = n - 1;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (arr[mid] == key) return mid;
        else if (key < arr[mid]) high = mid - 1;
        else low = mid + 1;
    }
    return -1;
}

int main() {
    int n, choice, key, res;
    int arr[100];

    printf("Enter number of students: ");
    scanf("%d", &n);

    do {
        printf("\n--- MENU ---\n1. Linear Search (Unsorted)\n2. Binary Search (Sorted)\n3. Exit\nEnter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter %d roll numbers: ", n);
                for (int i = 0; i < n; i++) scanf("%d", &arr[i]);
                printf("Enter roll number to search: ");
                scanf("%d", &key);
                res = linearSearch(arr, n, key);
                if (res != -1) printf("Found at index %d.\n", res);
                else printf("Roll number not found.\n");
                break;

            case 2:
                printf("Enter %d roll numbers in ASCENDING order: ", n);
                for (int i = 0; i < n; i++) scanf("%d", &arr[i]);
                printf("Enter roll number to search: ");
                scanf("%d", &key);
                res = binarySearch(arr, n, key);
                if (res != -1) printf("Found at index %d.\n", res);
                else printf("Roll number not found.\n");
                break;

            case 3:
                break;
            default:
                printf("Invalid choice!\n");
        }
    } while (choice != 3);

    return 0;
}
/*
 * Assignment No: 2
 * Title: Selection Sort and Bubble Sort on Student Percentages
 */

#include <stdio.h>

void bubbleSort(float arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                float temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

void selectionSort(float arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) min_idx = j;
        }
        if (min_idx != i) {
            float temp = arr[i];
            arr[i] = arr[min_idx];
            arr[min_idx] = temp;
        }
    }
}

void displayTopFive(float arr[], int n) {
    int count = 0;
    printf("\nTop 5 Scores:\n");
    for (int i = n - 1; i >= 0 && count < 5; i--) {
        printf("Rank %d: %.2f%%\n", ++count, arr[i]);
    }
}

int main() {
    int n, choice;
    float arr[100], copy[100];

    printf("Enter number of students: ");
    scanf("%d", &n);
    printf("Enter percentages: ");
    for (int i = 0; i < n; i++) scanf("%f", &arr[i]);

    do {
        for (int i = 0; i < n; i++) copy[i] = arr[i];
        printf("\n--- MENU ---\n1. Bubble Sort\n2. Selection Sort\n3. Exit\nEnter choice: ");
        scanf("%d", &choice);

        if (choice == 1) {
            bubbleSort(copy, n);
            printf("Sorted array: ");
            for (int i = 0; i < n; i++) printf("%.2f ", copy[i]);
            displayTopFive(copy, n);
        } else if (choice == 2) {
            selectionSort(copy, n);
            printf("Sorted array: ");
            for (int i = 0; i < n; i++) printf("%.2f ", copy[i]);
            displayTopFive(copy, n);
        }
    } while (choice != 3);

    return 0;
}
/*
 * Assignment No: 3
 * Title: Telephone Book using Hash Table Collision Resolution
 */

#include <stdio.h>
#include <string.h>

#define SIZE 10

typedef struct {
    long long phone;
    char name[50];
    int occupied;
} Entry;

Entry table[SIZE];

void init() {
    for (int i = 0; i < SIZE; i++) {
        table[i].phone = -1;
        table[i].occupied = 0;
    }
}

int h1(long long key) { return key % SIZE; }
int h2(long long key) { return 7 - (key % 7); }

void insertLinear(long long phone, char name[]) {
    int base = h1(phone);
    for (int i = 0; i < SIZE; i++) {
        int idx = (base + i) % SIZE;
        if (!table[idx].occupied) {
            table[idx].phone = phone;
            strcpy(table[idx].name, name);
            table[idx].occupied = 1;
            printf("Inserted at index %d\n", idx);
            return;
        }
    }
    printf("Hash table full!\n");
}

void insertQuadratic(long long phone, char name[]) {
    int base = h1(phone);
    for (int i = 0; i < SIZE; i++) {
        int idx = (base + (i * i)) % SIZE;
        if (!table[idx].occupied) {
            table[idx].phone = phone;
            strcpy(table[idx].name, name);
            table[idx].occupied = 1;
            printf("Inserted at index %d\n", idx);
            return;
        }
    }
    printf("Hash table full!\n");
}

void insertDouble(long long phone, char name[]) {
    int base = h1(phone);
    int step = h2(phone);
    for (int i = 0; i < SIZE; i++) {
        int idx = (base + (i * step)) % SIZE;
        if (!table[idx].occupied) {
            table[idx].phone = phone;
            strcpy(table[idx].name, name);
            table[idx].occupied = 1;
            printf("Inserted at index %d\n", idx);
            return;
        }
    }
    printf("Hash table full!\n");
}

void search(long long phone) {
    int base = h1(phone);
    for (int i = 0; i < SIZE; i++) {
        int idx = (base + i) % SIZE;
        if (!table[idx].occupied) break;
        if (table[idx].phone == phone) {
            printf("Found at index %d: %s | %lld\n", idx, table[idx].name, table[idx].phone);
            return;
        }
    }
    printf("Not found!\n");
}

void display() {
    printf("\nIndex\tPhone\t\tName\n");
    for (int i = 0; i < SIZE; i++) {
        if (table[i].occupied) printf("[%d]\t%-15lld\t%s\n", i, table[i].phone, table[i].name);
        else printf("[%d]\t---\t\t---\n", i);
    }
}

int main() {
    int choice;
    long long phone;
    char name[50];
    init();

    do {
        printf("\n--- MENU ---\n1. Linear Probing\n2. Quadratic Probing\n3. Double Hashing\n4. Search\n5. Display\n6. Exit\nEnter choice: ");
        scanf("%d", &choice);

        if (choice >= 1 && choice <= 3) {
            printf("Enter Name: "); scanf("%s", name);
            printf("Enter Phone: "); scanf("%lld", &phone);
            if (choice == 1) insertLinear(phone, name);
            else if (choice == 2) insertQuadratic(phone, name);
            else insertDouble(phone, name);
        } else if (choice == 4) {
            printf("Enter Phone: "); scanf("%lld", &phone);
            search(phone);
        } else if (choice == 5) {
            display();
        }
    } while (choice != 6);

    return 0;
}
/*
 * Assignment No: 4
 * Title: Pinnacle Club Member Management using Singly Linked List
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Node {
    int prn;
    char name[50];
    struct Node* next;
} Node;

Node* createNode(int prn, const char* name) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->prn = prn;
    strcpy(newNode->name, name);
    newNode->next = NULL;
    return newNode;
}

Node* addPresident(Node* head, int prn, const char* name) {
    Node* newNode = createNode(prn, name);
    newNode->next = head;
    return newNode;
}

Node* addSecretary(Node* head, int prn, const char* name) {
    Node* newNode = createNode(prn, name);
    if (!head) return newNode;
    Node* temp = head;
    while (temp->next != NULL) temp = temp->next;
    temp->next = newNode;
    return head;
}

Node* addMember(Node* head, int prn, const char* name) {
    if (!head || !head->next) {
        printf("Must have President and Secretary before adding members!\n");
        return head;
    }
    Node* newNode = createNode(prn, name);
    Node* temp = head;
    while (temp->next->next != NULL) temp = temp->next;
    newNode->next = temp->next;
    temp->next = newNode;
    return head;
}

Node* deletePresident(Node* head) {
    if (!head) return NULL;
    Node* temp = head;
    head = head->next;
    free(temp);
    return head;
}

Node* deleteSecretary(Node* head) {
    if (!head) return NULL;
    if (!head->next) { free(head); return NULL; }
    Node* temp = head;
    while (temp->next->next != NULL) temp = temp->next;
    free(temp->next);
    temp->next = NULL;
    return head;
}

Node* deleteMember(Node* head, int prn) {
    if (!head) return NULL;
    if (head->prn == prn) return deletePresident(head);

    Node* temp = head;
    while (temp->next != NULL && temp->next->prn != prn) temp = temp->next;
    if (temp->next == NULL) printf("PRN not found!\n");
    else {
        Node* del = temp->next;
        temp->next = temp->next->next;
        free(del);
    }
    return head;
}

int countMembers(Node* head) {
    int c = 0;
    while (head) { c++; head = head->next; }
    return c;
}

void displayMembers(Node* head) {
    if (!head) { printf("Club is empty!\n"); return; }
    Node* temp = head;
    while (temp) {
        if (temp == head) printf("[Pres] ");
        else if (temp->next == NULL) printf("[Sec]  ");
        else printf("[Mem]  ");
        printf("PRN: %d | Name: %s\n", temp->prn, temp->name);
        temp = temp->next;
    }
}

Node* concatenate(Node* h1, Node* h2) {
    if (!h1) return h2;
    if (!h2) return h1;
    Node* temp = h1;
    while (temp->next) temp = temp->next;
    temp->next = h2;
    return h1;
}

int main() {
    Node *divA = NULL, *divB = NULL;
    int choice, prn;
    char name[50];

    do {
        printf("\n--- PINNACLE CLUB ---\n1. Add Pres\n2. Add Sec\n3. Add Mem\n4. Del Pres\n5. Del Sec\n6. Del Mem\n7. Count\n8. Display\n9. Merge Div B\n10. Exit\nEnter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: printf("PRN & Name: "); scanf("%d %s", &prn, name); divA = addPresident(divA, prn, name); break;
            case 2: printf("PRN & Name: "); scanf("%d %s", &prn, name); divA = addSecretary(divA, prn, name); break;
            case 3: printf("PRN & Name: "); scanf("%d %s", &prn, name); divA = addMember(divA, prn, name); break;
            case 4: divA = deletePresident(divA); break;
            case 5: divA = deleteSecretary(divA); break;
            case 6: printf("PRN: "); scanf("%d", &prn); divA = deleteMember(divA, prn); break;
            case 7: printf("Total: %d\n", countMembers(divA)); break;
            case 8: displayMembers(divA); break;
            case 9:
                divB = addPresident(divB, 901, "Pres_B");
                divB = addSecretary(divB, 902, "Sec_B");
                divA = concatenate(divA, divB);
                divB = NULL;
                printf("Merged successfully!\n");
                break;
        }
    } while (choice != 10);

    return 0;
}
/*
 * Assignment No: 5
 * Title: Cinemax Theater Ticket Booking using Circular Doubly Linked List
 */

#include <stdio.h>
#include <stdlib.h>

#define ROWS 10
#define SEATS 7

typedef struct Seat {
    int seatNo;
    int isBooked;
    struct Seat *prev, *next;
} Seat;

Seat* headRow[ROWS];

void initTheater() {
    for (int r = 0; r < ROWS; r++) {
        Seat *head = NULL, *tail = NULL;
        for (int i = 1; i <= SEATS; i++) {
            Seat* s = (Seat*)malloc(sizeof(Seat));
            s->seatNo = i;
            s->isBooked = 0;
            if (!head) {
                head = tail = s;
                head->next = head->prev = head;
            } else {
                tail->next = s;
                s->prev = tail;
                s->next = head;
                head->prev = s;
                tail = s;
            }
        }
        headRow[r] = head;
    }
}

void displaySeats() {
    printf("\n--- CINEMAX SEAT STATUS ---\n");
    for (int r = 0; r < ROWS; r++) {
        printf("Row %2d: ", r + 1);
        Seat* temp = headRow[r];
        do {
            if (temp->isBooked) printf("[X] ");
            else printf("[%d] ", temp->seatNo);
            temp = temp->next;
        } while (temp != headRow[r]);
        printf("\n");
    }
}

void bookSeat(int row, int seat) {
    if (row < 1 || row > ROWS || seat < 1 || seat > SEATS) return;
    Seat* temp = headRow[row - 1];
    do {
        if (temp->seatNo == seat) {
            if (temp->isBooked) printf("Seat already booked!\n");
            else { temp->isBooked = 1; printf("Seat booked!\n"); }
            return;
        }
        temp = temp->next;
    } while (temp != headRow[row - 1]);
}

void cancelSeat(int row, int seat) {
    if (row < 1 || row > ROWS || seat < 1 || seat > SEATS) return;
    Seat* temp = headRow[row - 1];
    do {
        if (temp->seatNo == seat) {
            if (!temp->isBooked) printf("Seat is not booked!\n");
            else { temp->isBooked = 0; printf("Booking cancelled!\n"); }
            return;
        }
        temp = temp->next;
    } while (temp != headRow[row - 1]);
}

int main() {
    int choice, r, s;
    initTheater();

    do {
        printf("\n--- CINEMAX MENU ---\n1. Display Seats\n2. Book Seat\n3. Cancel Seat\n4. Exit\nEnter choice: ");
        scanf("%d", &choice);

        if (choice == 1) displaySeats();
        else if (choice == 2) {
            printf("Enter Row (1-10) and Seat (1-7): ");
            scanf("%d %d", &r, &s);
            bookSeat(r, s);
        } else if (choice == 3) {
            printf("Enter Row (1-10) and Seat (1-7): ");
            scanf("%d %d", &r, &s);
            cancelSeat(r, s);
        }
    } while (choice != 4);

    return 0;
}
/*
 * Assignment No: 6
 * Title: Infix to Postfix Conversion and Evaluation using Stack
 */

#include <stdio.h>
#include <ctype.h>
#include <math.h>

#define MAX 100

char cStack[MAX];
int cTop = -1;
int nStack[MAX];
int nTop = -1;

void pushC(char c) { cStack[++cTop] = c; }
char popC() { return cStack[cTop--]; }
char peekC() { return cStack[cTop]; }

void pushN(int val) { nStack[++nTop] = val; }
int popN() { return nStack[nTop--]; }

int precedence(char op) {
    if (op == '^') return 3;
    if (op == '*' || op == '/') return 2;
    if (op == '+' || op == '-') return 1;
    return 0;
}

void infixToPostfix(char infix[], char postfix[]) {
    int i = 0, k = 0;
    cTop = -1;

    while (infix[i] != '\0') {
        char ch = infix[i++];
        if (isalnum(ch)) {
            postfix[k++] = ch;
        } else if (ch == '(') {
            pushC(ch);
        } else if (ch == ')') {
            while (cTop != -1 && peekC() != '(') postfix[k++] = popC();
            if (cTop != -1) popC();
        } else {
            while (cTop != -1 && peekC() != '(' && precedence(ch) <= precedence(peekC())) {
                postfix[k++] = popC();
            }
            pushC(ch);
        }
    }
    while (cTop != -1) postfix[k++] = popC();
    postfix[k] = '\0';
}

int evaluatePostfix(char postfix[]) {
    int i = 0;
    nTop = -1;

    while (postfix[i] != '\0') {
        char ch = postfix[i++];
        if (isdigit(ch)) {
            pushN(ch - '0');
        } else {
            int op2 = popN();
            int op1 = popN();
            switch (ch) {
                case '+': pushN(op1 + op2); break;
                case '-': pushN(op1 - op2); break;
                case '*': pushN(op1 * op2); break;
                case '/': pushN(op1 / op2); break;
                case '^': pushN((int)pow(op1, op2)); break;
            }
        }
    }
    return popN();
}

int main() {
    char infix[MAX], postfix[MAX];
    int choice;

    do {
        printf("\n--- EXPRESSION MENU ---\n1. Infix to Postfix\n2. Evaluate Postfix\n3. Exit\nEnter choice: ");
        scanf("%d", &choice);

        if (choice == 1) {
            printf("Enter Infix: ");
            scanf("%s", infix);
            infixToPostfix(infix, postfix);
            printf("Postfix: %s\n", postfix);
        } else if (choice == 2) {
            printf("Enter Postfix: ");
            scanf("%s", postfix);
            printf("Result: %d\n", evaluatePostfix(postfix));
        }
    } while (choice != 3);

    return 0;
}
/*
 * Assignment No: 7
 * Title: Pizza Parlor Job Queue Simulation using Circular Queue
 */

#include <stdio.h>

#define MAX 5

typedef struct {
    int orders[MAX];
    int front;
    int rear;
    int count;
} PizzaQueue;

void initQueue(PizzaQueue* q) {
    q->front = 0;
    q->rear = -1;
    q->count = 0;
}

void enqueue(PizzaQueue* q, int id) {
    if (q->count == MAX) {
        printf("Queue is full! Cannot take order %d.\n", id);
        return;
    }
    q->rear = (q->rear + 1) % MAX;
    q->orders[q->rear] = id;
    q->count++;
    printf("Order %d placed.\n", id);
}

void dequeue(PizzaQueue* q) {
    if (q->count == 0) {
        printf("No orders to serve!\n");
        return;
    }
    printf("Order %d served.\n", q->orders[q->front]);
    q->front = (q->front + 1) % MAX;
    q->count--;
}

void displayQueue(PizzaQueue* q) {
    if (q->count == 0) {
        printf("Queue is empty!\n");
        return;
    }
    printf("Orders: ");
    int idx = q->front;
    for (int i = 0; i < q->count; i++) {
        printf("%d ", q->orders[idx]);
        idx = (idx + 1) % MAX;
    }
    printf("\n");
}

int main() {
    PizzaQueue q;
    initQueue(&q);
    int choice, id;

    do {
        printf("\n--- PIZZA PARLOR ---\n1. Place Order\n2. Serve Order\n3. Display Orders\n4. Exit\nEnter choice: ");
        scanf("%d", &choice);

        if (choice == 1) {
            printf("Enter Order ID: ");
            scanf("%d", &id);
            enqueue(&q, id);
        } else if (choice == 2) {
            dequeue(&q);
        } else if (choice == 3) {
            displayQueue(&q);
        }
    } while (choice != 4);

    return 0;
}
/*
 * Assignment No: 8
 * Title: Binary Search Tree Operations
 */

#include <stdio.h>
#include <stdlib.h>

typedef struct BSTNode {
    int data;
    struct BSTNode *left, *right;
} BSTNode;

BSTNode* createNode(int val) {
    BSTNode* n = (BSTNode*)malloc(sizeof(BSTNode));
    n->data = val;
    n->left = n->right = NULL;
    return n;
}

BSTNode* insert(BSTNode* root, int val) {
    if (!root) return createNode(val);
    if (val < root->data) root->left = insert(root->left, val);
    else if (val > root->data) root->right = insert(root->right, val);
    return root;
}

int findHeight(BSTNode* root) {
    if (!root) return 0;
    int lh = findHeight(root->left);
    int rh = findHeight(root->right);
    return (lh > rh ? lh : rh) + 1;
}

int findMin(BSTNode* root) {
    if (!root) return -1;
    while (root->left) root = root->left;
    return root->data;
}

void mirrorTree(BSTNode* root) {
    if (!root) return;
    mirrorTree(root->left);
    mirrorTree(root->right);
    BSTNode* temp = root->left;
    root->left = root->right;
    root->right = temp;
}

int search(BSTNode* root, int key) {
    if (!root) return 0;
    if (root->data == key) return 1;
    if (key < root->data) return search(root->left, key);
    return search(root->right, key);
}

void inorder(BSTNode* root) {
    if (root) {
        inorder(root->left);
        printf("%d ", root->data);
        inorder(root->right);
    }
}

int main() {
    BSTNode* root = NULL;
    int choice, val;

    do {
        printf("\n--- BST MENU ---\n1. Insert\n2. Inorder\n3. Height\n4. Min Value\n5. Mirror\n6. Search\n7. Exit\nEnter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: printf("Enter value: "); scanf("%d", &val); root = insert(root, val); break;
            case 2: printf("Inorder: "); inorder(root); printf("\n"); break;
            case 3: printf("Height: %d\n", findHeight(root)); break;
            case 4: printf("Min: %d\n", findMin(root)); break;
            case 5: mirrorTree(root); printf("Mirrored.\n"); break;
            case 6: printf("Search key: "); scanf("%d", &val); printf(search(root, val) ? "Found!\n" : "Not Found!\n"); break;
        }
    } while (choice != 7);

    return 0;
}
/*
 * Assignment No: 9
 * Title: Flight Path Graph and Connectivity Check using DFS
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 20

typedef struct FlightNode {
    int dest;
    int cost;
    struct FlightNode* next;
} FlightNode;

int numCities;
char cityNames[MAX][50];
FlightNode* adjList[MAX];
int visited[MAX];

int getIndex(char name[]) {
    for (int i = 0; i < numCities; i++) {
        if (strcmp(cityNames[i], name) == 0) return i;
    }
    return -1;
}

void addFlight(int u, int v, int cost) {
    FlightNode* n1 = (FlightNode*)malloc(sizeof(FlightNode));
    n1->dest = v; n1->cost = cost;
    n1->next = adjList[u]; adjList[u] = n1;

    FlightNode* n2 = (FlightNode*)malloc(sizeof(FlightNode));
    n2->dest = u; n2->cost = cost;
    n2->next = adjList[v]; adjList[v] = n2;
}

void display() {
    printf("\n--- FLIGHT PATHS ---\n");
    for (int i = 0; i < numCities; i++) {
        printf("%s -> ", cityNames[i]);
        FlightNode* temp = adjList[i];
        while (temp) {
            printf("[%s | Cost: %d] ", cityNames[temp->dest], temp->cost);
            temp = temp->next;
        }
        printf("NULL\n");
    }
}

void dfs(int u) {
    visited[u] = 1;
    FlightNode* temp = adjList[u];
    while (temp) {
        if (!visited[temp->dest]) dfs(temp->dest);
        temp = temp->next;
    }
}

int isConnected() {
    for (int i = 0; i < numCities; i++) visited[i] = 0;
    dfs(0);
    for (int i = 0; i < numCities; i++) {
        if (!visited[i]) return 0;
    }
    return 1;
}

int main() {
    int choice, routes, cost;
    char src[50], dest[50];

    do {
        printf("\n--- GRAPH MENU ---\n1. Setup Network\n2. Display Paths\n3. Check Connectivity\n4. Exit\nEnter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter number of cities: ");
                scanf("%d", &numCities);
                for (int i = 0; i < numCities; i++) {
                    printf("City %d: ", i + 1);
                    scanf("%s", cityNames[i]);
                    adjList[i] = NULL;
                }
                printf("Enter number of flight paths: ");
                scanf("%d", &routes);
                for (int i = 0; i < routes; i++) {
                    printf("Src Dest Cost: ");
                    scanf("%s %s %d", src, dest, &cost);
                    int u = getIndex(src), v = getIndex(dest);
                    if (u != -1 && v != -1) addFlight(u, v, cost);
                }
                break;

            case 2: display(); break;
            case 3: printf(isConnected() ? "Graph is CONNECTED.\n" : "Graph is NOT CONNECTED.\n"); break;
        }
    } while (choice != 4);

    return 0;
}
