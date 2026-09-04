#include <iostream>
#include <cstdlib>
#include <cstring>
#include <chrono>
using namespace std;
using namespace std::chrono;

struct Student
{
    int id;
    char name[50];
    float cgpa;
};

int main()
{
    int n;

    cout << "Enter Number of Students: ";
    cin >> n;

    Student *students = (Student*)malloc(n * sizeof(Student));

    for(int i = 0; i < n; i++)
    {
        cout << "\nStudent " << i + 1 << endl;

        cout << "Enter ID: ";
        cin >> students[i].id;

        cout << "Enter Name: ";
        cin >> students[i].name;

        cout << "Enter CGPA: ";
        cin >> students[i].cgpa;
    }

    cout << "\n========== Student Records ==========\n";
    cout << "ID\tName\tCGPA\n";

    for(int i = 0; i < n; i++)
    {
        cout << students[i].id << "\t"
             << students[i].name << "\t"
             << students[i].cgpa << endl;
    }

    int key;

    cout << "\nEnter Student ID to Search (Linear Search): ";
    cin >> key;

    auto start = high_resolution_clock::now();

    int pos = -1;

    for(int i = 0; i < n; i++)
    {
        if(students[i].id == key)
        {
            pos = i;
            break;
        }
    }

    auto stop = high_resolution_clock::now();
    auto linearTime = duration_cast<nanoseconds>(stop - start);

    if(pos != -1)
        cout << "\nRecord Found Using Linear Search";
    else
        cout << "\nRecord Not Found";

    // Bubble Sort by Name
    for(int i = 0; i < n - 1; i++)
    {
        for(int j = 0; j < n - i - 1; j++)
        {
            if(strcmp(students[j].name, students[j + 1].name) > 0)
            {
                Student temp = students[j];
                students[j] = students[j + 1];
                students[j + 1] = temp;
            }
        }
    }

    cout << "\n\n===== Sorted By Name =====\n";
    cout << "ID\tName\tCGPA\n";

    for(int i = 0; i < n; i++)
    {
        cout << students[i].id << "\t"
             << students[i].name << "\t"
             << students[i].cgpa << endl;
    }

    // Selection Sort by CGPA
    for(int i = 0; i < n - 1; i++)
    {
        int min = i;

        for(int j = i + 1; j < n; j++)
        {
            if(students[j].cgpa < students[min].cgpa)
                min = j;
        }

        Student temp = students[i];
        students[i] = students[min];
        students[min] = temp;
    }

    cout << "\n\n===== Sorted By CGPA =====\n";
    cout << "ID\tName\tCGPA\n";

    for(int i = 0; i < n; i++)
    {
        cout << students[i].id << "\t"
             << students[i].name << "\t"
             << students[i].cgpa << endl;
    }

    // Bubble Sort by ID
    for(int i = 0; i < n - 1; i++)
    {
        for(int j = 0; j < n - i - 1; j++)
        {
            if(students[j].id > students[j + 1].id)
            {
                Student temp = students[j];
                students[j] = students[j + 1];
                students[j + 1] = temp;
            }
        }
    }

    cout << "\nEnter Student ID to Search (Binary Search): ";
    cin >> key;

    start = high_resolution_clock::now();

    int low = 0;
    int high = n - 1;
    pos = -1;

    while(low <= high)
    {
        int mid = (low + high) / 2;

        if(students[mid].id == key)
        {
            pos = mid;
            break;
        }
        else if(key < students[mid].id)
            high = mid - 1;
        else
            low = mid + 1;
    }

    stop = high_resolution_clock::now();
    auto binaryTime = duration_cast<nanoseconds>(stop - start);

    if(pos != -1)
        cout << "\nRecord Found Using Binary Search";
    else
        cout << "\nRecord Not Found";

    cout << "\n\n========== Performance Comparison ==========\n";
    cout << "Linear Search Time : " << linearTime.count() << " ns\n";
    cout << "Binary Search Time : " << binaryTime.count() << " ns\n";

    free(students);

    return 0;
}
