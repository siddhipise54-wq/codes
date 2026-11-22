DS1:
#include <iostream>
using namespace std;

struct Student
{
    int PRN;
    int date;
    int month;
};

int main()
{
    Student A[50], B[50], SE_AIML_DOB[100];
    int n, m, i, j, k;

    cout << "Enter number of students in Division A: ";
    cin >> n;

    cout << "Enter PRN, date and month for Division A:\n";
    for (i = 0; i < n; i++)
    {
        cin >> A[i].PRN >> A[i].date >> A[i].month;
    }

    cout << "Enter number of students in Division B: ";
    cin >> m;

    cout << "Enter PRN, date and month for Division B:\n";
    for (i = 0; i < m; i++)
    {
        cin >> B[i].PRN >> B[i].date >> B[i].month;
    }

    i = 0;
    j = 0;
    k = 0;

    while (i < n && j < m)
    {
        if (A[i].month < B[j].month ||
           (A[i].month == B[j].month &&
            A[i].date <= B[j].date))
        {
            SE_AIML_DOB[k] = A[i];
            i++;
        }
        else
        {
            SE_AIML_DOB[k] = B[j];
            j++;
        }
        k++;
    }

    while (i < n)
    {
        SE_AIML_DOB[k] = A[i];
        i++;
        k++;
    }

    while (j < m)
    {
        SE_AIML_DOB[k] = B[j];
        j++;
        k++;
    }

    cout << "\nSorted SE AIML Students by Date of Birth:\n";
    cout << "PRN\tDate\tMonth\n";

    for (i = 0; i < k; i++)
    {
        cout << SE_AIML_DOB[i].PRN << "\t"
             << SE_AIML_DOB[i].date << "\t"
             << SE_AIML_DOB[i].month << endl;
    }

    return 0;
}

DS2:
#include <iostream>
using namespace std;

struct Node
{
    int roll;
    Node *next;
};

Node* insert(Node *head, int x)
{
    Node *temp = new Node;
    temp->roll = x;
    temp->next = NULL;

    if (head == NULL)
        return temp;

    Node *p = head;
    while (p->next != NULL)
        p = p->next;

    p->next = temp;
    return head;
}

int search(Node *head, int x)
{
    while (head != NULL)
    {
        if (head->roll == x)
            return 1;
        head = head->next;
    }
    return 0;
}

void display(Node *head)
{
    while (head != NULL)
    {
        cout << head->roll << " ";
        head = head->next;
    }
    cout << endl;
}

int main()
{
    Node *A = NULL, *B = NULL;
    Node *p;
    int n, m, x, total;

    cout << "Enter total number of students: ";
    cin >> total;

    cout << "Enter number of students who like Vanilla: ";
    cin >> n;

    cout << "Enter roll numbers:\n";
    for (int i = 0; i < n; i++)
    {
        cin >> x;
        A = insert(A, x);
    }

    cout << "Enter number of students who like Butterscotch: ";
    cin >> m;

    cout << "Enter roll numbers:\n";
    for (int i = 0; i < m; i++)
    {
        cin >> x;
        B = insert(B, x);
    }

    cout << "\n1. Students who like either or both:\n";
    display(A);

    p = B;
    while (p != NULL)
    {
        if (!search(A, p->roll))
            cout << p->roll << " ";
        p = p->next;
    }
    cout << endl;

    cout << "\n2. Students who like both:\n";
    p = A;
    while (p != NULL)
    {
        if (search(B, p->roll))
            cout << p->roll << " ";
        p = p->next;
    }
    cout << endl;

    cout << "\n3. Students who like only Vanilla:\n";
    p = A;
    while (p != NULL)
    {
        if (!search(B, p->roll))
            cout << p->roll << " ";
        p = p->next;
    }
    cout << endl;

    cout << "\n4. Students who like only Butterscotch:\n";
    p = B;
    while (p != NULL)
    {
        if (!search(A, p->roll))
            cout << p->roll << " ";
        p = p->next;
    }
    cout << endl;

    int count = 0;
    for (int i = 1; i <= total; i++)
    {
        if (!search(A, i) && !search(B, i))
            count++;
    }

    cout << "\n5. Number of students who like neither: "
         << count << endl;

    return 0;
}

DS3:
#include <iostream>
#include <cctype>
using namespace std;

char stack[100];
int top = -1;

void push(char x)
{
    stack[++top] = x;
}

char pop()
{
    return stack[top--];
}

int priority(char x)
{
    if (x == '+' || x == '-')
        return 1;
    if (x == '*' || x == '/')
        return 2;
    return 0;
}

int main()
{
    char infix[100], postfix[100];
    int i, j = 0;

    cout << "Enter infix expression: ";
    cin >> infix;

    for (i = 0; infix[i] != '\0'; i++)
    {
        char x = infix[i];

        if (isalnum(x))
        {
            postfix[j++] = x;
        }
        else if (x == '(')
        {
            push(x);
        }
        else if (x == ')')
        {
            while (top != -1 && stack[top] != '(')
                postfix[j++] = pop();

            if (top != -1)
                pop();
        }
        else
        {
            while (top != -1 && priority(stack[top]) >= priority(x))
                postfix[j++] = pop();

            push(x);
        }
    }

    while (top != -1)
        postfix[j++] = pop();

    postfix[j] = '\0';

    cout << "Postfix expression: " << postfix << endl;

    int value[100], t = -1;

    cout << "\nEnter digit values for operands (if required):\n";

    for (i = 0; postfix[i] != '\0'; i++)
    {
        char x = postfix[i];

        if (isdigit(x))
        {
            value[++t] = x - '0';
        }
        else if (isalpha(x))
        {
            int v;
            cout << x << " = ";
            cin >> v;
            value[++t] = v;
        }
        else
        {
            int b = value[t--];
            int a = value[t--];

            switch (x)
            {
                case '+': value[++t] = a + b; break;
                case '-': value[++t] = a - b; break;
                case '*': value[++t] = a * b; break;
                case '/':
                    if (b == 0)
                    {
                        cout << "Division by zero error!";
                        return 0;
                    }
                    value[++t] = a / b;
                    break;
            }
        }
    }

    cout << "Result = " << value[t] << endl;

    return 0;
}


DS4:
#include <iostream>
using namespace std;

#define M 5

class PizzaParlor
{
    int queue[M];
    int front, rear;

public:
    PizzaParlor()
    {
        front = -1;
        rear = -1;
    }

    void placeOrder()
    {
        int order;

        if ((rear + 1) % M == front)
        {
            cout << "Queue is full! Cannot accept order.\n";
            return;
        }

        cout << "Enter order number: ";
        cin >> order;

        if (front == -1)
            front = 0;

        rear = (rear + 1) % M;
        queue[rear] = order;

        cout << "Order placed successfully.\n";
    }

    void serveOrder()
    {
        if (front == -1)
        {
            cout << "No orders to serve.\n";
            return;
        }

        cout << "Order served: " << queue[front] << endl;

        if (front == rear)
        {
            front = -1;
            rear = -1;
        }
        else
        {
            front = (front + 1) % M;
        }
    }

    void display()
    {
        if (front == -1)
        {
            cout << "No pending orders.\n";
            return;
        }

        cout << "Pending orders: ";

        int i = front;

        while (true)
        {
            cout << queue[i] << " ";

            if (i == rear)
                break;

            i = (i + 1) % M;
        }

        cout << endl;
    }
};

int main()
{
    PizzaParlor p;
    int choice;

    do
    {
        cout << "\n--- Pizza Parlor ---\n";
        cout << "1. Place Order\n";
        cout << "2. Serve Order\n";
        cout << "3. Display Orders\n";
        cout << "4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice)
        {
            case 1:
                p.placeOrder();
                break;

            case 2:
                p.serveOrder();
                break;

            case 3:
                p.display();
                break;

            case 4:
                cout << "Exiting program.\n";
                break;

            default:
                cout << "Invalid choice.\n";
        }

    } while (choice != 4);

    return 0;
}


DS5:

#include <iostream>
#include <string>
using namespace std;

struct Node
{
    string keyword, meaning;
    Node *left, *right;
};

Node* createNode(string k, string m)
{
    Node *temp = new Node;
    temp->keyword = k;
    temp->meaning = m;
    temp->left = NULL;
    temp->right = NULL;
    return temp;
}

Node* insert(Node *root, string k, string m)
{
    if (root == NULL)
        return createNode(k, m);

    if (k < root->keyword)
        root->left = insert(root->left, k, m);
    else if (k > root->keyword)
        root->right = insert(root->right, k, m);
    else
        cout << "Keyword already exists!\n";

    return root;
}

Node* search(Node *root, string k, int &count)
{
    while (root != NULL)
    {
        count++;

        if (k == root->keyword)
            return root;
        else if (k < root->keyword)
            root = root->left;
        else
            root = root->right;
    }

    return NULL;
}

Node* minimum(Node *root)
{
    while (root->left != NULL)
        root = root->left;

    return root;
}

Node* deleteNode(Node *root, string k)
{
    if (root == NULL)
        return NULL;

    if (k < root->keyword)
        root->left = deleteNode(root->left, k);
    else if (k > root->keyword)
        root->right = deleteNode(root->right, k);
    else
    {
        if (root->left == NULL)
        {
            Node *temp = root->right;
            delete root;
            return temp;
        }
        else if (root->right == NULL)
        {
            Node *temp = root->left;
            delete root;
            return temp;
        }

        Node *temp = minimum(root->right);
        root->keyword = temp->keyword;
        root->meaning = temp->meaning;
        root->right = deleteNode(root->right, temp->keyword);
    }

    return root;
}

void ascending(Node *root)
{
    if (root != NULL)
    {
        ascending(root->left);
        cout << root->keyword << " : "
             << root->meaning << endl;
        ascending(root->right);
    }
}

void descending(Node *root)
{
    if (root != NULL)
    {
        descending(root->right);
        cout << root->keyword << " : "
             << root->meaning << endl;
        descending(root->left);
    }
}

int main()
{
    Node *root = NULL;
    Node *temp;
    string k, m;
    int choice, count;

    do
    {
        cout << "\n--- DICTIONARY ---\n";
        cout << "1. Add Keyword\n";
        cout << "2. Delete Keyword\n";
        cout << "3. Update Meaning\n";
        cout << "4. Search Keyword\n";
        cout << "5. Display Ascending\n";
        cout << "6. Display Descending\n";
        cout << "7. Maximum Comparisons\n";
        cout << "8. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice)
        {
            case 1:
                cout << "Enter keyword: ";
                cin >> k;
                cout << "Enter meaning: ";
                cin >> m;
                root = insert(root, k, m);
                break;

            case 2:
                cout << "Enter keyword to delete: ";
                cin >> k;
                count = 0;
                temp = search(root, k, count);

                if (temp != NULL)
                {
                    root = deleteNode(root, k);
                    cout << "Keyword deleted.\n";
                }
                else
                    cout << "Keyword not found.\n";
                break;

            case 3:
                cout << "Enter keyword to update: ";
                cin >> k;
                count = 0;
                temp = search(root, k, count);

                if (temp != NULL)
                {
                    cout << "Enter new meaning: ";
                    cin >> m;
                    temp->meaning = m;
                    cout << "Meaning updated.\n";
                }
                else
                    cout << "Keyword not found.\n";
                break;

            case 4:
                cout << "Enter keyword to search: ";
                cin >> k;
                count = 0;
                temp = search(root, k, count);

                if (temp != NULL)
                    cout << "Meaning: " << temp->meaning << endl;
                else
                    cout << "Keyword not found.\n";

                cout << "Comparisons: " << count << endl;
                break;

            case 5:
                cout << "\nDictionary in Ascending Order:\n";
                ascending(root);
                break;

            case 6:
                cout << "\nDictionary in Descending Order:\n";
                descending(root);
                break;

            case 7:
                count = 0;
                temp = root;

                if (root == NULL)
                    cout << "Dictionary is empty.\n";
                else
                {
                    // Find the maximum number of comparisons
                    // by finding the deepest node.
                    int maxCount = 0;

                    // Calculate the height of the BST.
                    Node *stack[100];
                    int depth[100];
                    int top = 0;

                    stack[top] = root;
                    depth[top] = 1;
                    top++;

                    while (top > 0)
                    {
                        top--;
                        Node *p = stack[top];
                        int d = depth[top];

                        if (d > maxCount)
                            maxCount = d;

                        if (p->left != NULL)
                        {
                            stack[top] = p->left;
                            depth[top] = d + 1;
                            top++;
                        }

                        if (p->right != NULL)
                        {
                            stack[top] = p->right;
                            depth[top] = d + 1;
                            top++;
                        }
                    }

                    cout << "Maximum comparisons required: "
                         << maxCount << endl;
                }
                break;

            case 8:
                cout << "Exiting...\n";
                break;

            default:
                cout << "Invalid choice.\n";
        }

    } while (choice != 8);

    return 0;
}

DS6:

#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node *left, *right;
};

Node* createNode(int x)
{
    Node *temp = new Node;
    temp->data = x;
    temp->left = NULL;
    temp->right = NULL;
    return temp;
}

Node* insert(Node *root, int x)
{
    if (root == NULL)
        return createNode(x);

    if (x < root->data)
        root->left = insert(root->left, x);
    else if (x > root->data)
        root->right = insert(root->right, x);

    return root;
}

int height(Node *root)
{
    if (root == NULL)
        return 0;

    int l = height(root->left);
    int r = height(root->right);

    if (l > r)
        return l + 1;
    else
        return r + 1;
}

int minimum(Node *root)
{
    if (root == NULL)
        return -1;

    while (root->left != NULL)
        root = root->left;

    return root->data;
}

void swapTree(Node *root)
{
    if (root == NULL)
        return;

    Node *temp = root->left;
    root->left = root->right;
    root->right = temp;

    swapTree(root->left);
    swapTree(root->right);
}

int search(Node *root, int x)
{
    if (root == NULL)
        return 0;

    if (root->data == x)
        return 1;

    if (x < root->data)
        return search(root->left, x);
    else
        return search(root->right, x);
}

void inorder(Node *root)
{
    if (root != NULL)
    {
        inorder(root->left);
        cout << root->data << " ";
        inorder(root->right);
    }
}

int main()
{
    Node *root = NULL;
    int n, x, choice;

    cout << "Enter number of nodes: ";
    cin >> n;

    cout << "Enter values:\n";
    for (int i = 0; i < n; i++)
    {
        cin >> x;
        root = insert(root, x);
    }

    do
    {
        cout << "\n\n--- BINARY SEARCH TREE ---\n";
        cout << "1. Insert new node\n";
        cout << "2. Longest path from root\n";
        cout << "3. Minimum value\n";
        cout << "4. Swap left and right pointers\n";
        cout << "5. Search a value\n";
        cout << "6. Display tree (Inorder)\n";
        cout << "7. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice)
        {
            case 1:
                cout << "Enter value to insert: ";
                cin >> x;
                root = insert(root, x);
                cout << "Node inserted.\n";
                break;

            case 2:
                cout << "Number of nodes in longest path: "
                     << height(root) << endl;
                break;

            case 3:
                if (root == NULL)
                    cout << "Tree is empty.\n";
                else
                    cout << "Minimum value: "
                         << minimum(root) << endl;
                break;

            case 4:
                swapTree(root);
                cout << "Left and right pointers swapped.\n";
                break;

            case 5:
                cout << "Enter value to search: ";
                cin >> x;

                if (search(root, x))
                    cout << "Value found.\n";
                else
                    cout << "Value not found.\n";
                break;

            case 6:
                cout << "Tree values: ";
                inorder(root);
                cout << endl;
                break;

            case 7:
                cout << "Exiting program.\n";
                break;

            default:
                cout << "Invalid choice.\n";
        }

    } while (choice != 7);

    return 0;
}

DS7:

#include <iostream>
using namespace std;

int a[10][10], visited[10], n;

void DFS(int v)
{
    cout << v << " ";
    visited[v] = 1;

    for (int i = 0; i < n; i++)
    {
        if (a[v][i] == 1 && visited[i] == 0)
            DFS(i);
    }
}

void BFS(int start)
{
    int q[10], front = 0, rear = 0;
    int vis[10] = {0};

    q[rear++] = start;
    vis[start] = 1;

    while (front < rear)
    {
        int v = q[front++];
        cout << v << " ";

        for (int i = 0; i < n; i++)
        {
            if (a[v][i] == 1 && vis[i] == 0)
            {
                q[rear++] = i;
                vis[i] = 1;
            }
        }
    }
}

int main()
{
    int i, j, edges, u, v, start;

    cout << "Enter number of landmarks: ";
    cin >> n;

    cout << "Enter number of roads: ";
    cin >> edges;

    cout << "Enter connected landmarks (u v):\n";

    for (i = 0; i < edges; i++)
    {
        cin >> u >> v;
        a[u][v] = 1;
        a[v][u] = 1;
    }

    cout << "Adjacency Matrix:\n";

    for (i = 0; i < n; i++)
    {
        for (j = 0; j < n; j++)
            cout << a[i][j] << " ";

        cout << endl;
    }

    cout << "Enter starting landmark: ";
    cin >> start;

    cout << "DFS: ";
    DFS(start);

    cout << "\nBFS: ";
    BFS(start);

    return 0;
}

DS8:

#include <iostream>
using namespace std;

int main()
{
    int cost[10][10];
    string city[10];
    int n, i, j;

    cout << "Enter number of cities: ";
    cin >> n;

    cout << "Enter city names:\n";
    for (i = 0; i < n; i++)
        cin >> city[i];

    for (i = 0; i < n; i++)
    {
        for (j = 0; j < n; j++)
            cost[i][j] = 0;
    }

    cout << "Enter flight cost between cities (0 if no flight):\n";

    for (i = 0; i < n; i++)
    {
        for (j = 0; j < n; j++)
        {
            if (i != j)
            {
                cout << city[i] << " to " << city[j] << ": ";
                cin >> cost[i][j];
            }
        }
    }

    cout << "\nFlight Graph (Adjacency Matrix):\n\t";

    for (i = 0; i < n; i++)
        cout << city[i] << "\t";

    cout << endl;

    for (i = 0; i < n; i++)
    {
        cout << city[i] << "\t";

        for (j = 0; j < n; j++)
            cout << cost[i][j] << "\t";

        cout << endl;
    }

    return 0;
}