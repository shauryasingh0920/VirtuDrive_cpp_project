# VirtuDrive_cpp_project

#include <bits/stdc++.h>
#include <iostream>
#include <fstream>
#include <chrono>
#include <stdlib.h>
#include <ctime>
#include <string>
#include <vector>
using namespace std;

#define V 9

class Road
{
public:
  string name;
  int length;

  Road(const string &n, int l) : name(n), length(l) {}
};

class Intersection
{
public:
  string name;
  vector<Road> connected_roads;

  Intersection(const string &n, const vector<Road> &roads) : name(n), connected_roads(roads) {}
};

class Vehicle
{
public:
  string vehicle_type;
  double arrival_time;

  Vehicle(const string &type, double time) : vehicle_type(type), arrival_time(time) {}
};

class TrafficSignal
{
public:
  string name;
  int state; // 0 for Red, 1 for Green
  int duration;

  TrafficSignal(const string &n, int s, int d) : name(n), state(s), duration(d) {}
};

class TrafficManagementSystem
{
public:
  vector<Road> roads;
  vector<Intersection> intersections;
  vector<Vehicle> vehicles;
  vector<TrafficSignal> traffic_signals;

  void addRoad(const string &name, int length)
  {
    Road road(name, length);
    roads.push_back(road);
  }

  void addIntersection(const string &name, const vector<Road> &connected_roads)
  {
    Intersection intersection(name, connected_roads);
    intersections.push_back(intersection);
  }

  void generateVehicle(const string &vehicle_type, double arrival_time)
  {
    Vehicle vehicle(vehicle_type, arrival_time);
    vehicles.push_back(vehicle);
  }

  void addTrafficSignal(const string &name, int state, int duration)
  {
    TrafficSignal signal(name, state, duration);
    traffic_signals.push_back(signal);
  }

  int welcome()
  {

    system("clear");
    time_t tt;
    struct tm *ti;
    time(&tt);
    ti = localtime(&tt);
    cout << endl
         << endl
         << endl
         << endl
         << endl
         << endl
         << endl
         << endl
         << endl
         << " " << asctime(ti);
    delay_1();
    system("clear");

    cout << "' '" << endl;
    cout << "' HEARTY WELCOME TO '" << endl;
    cout << "' '" << endl;
    cout << "' TRAFFIC MANAGEMENT SYSTEM '" << endl;
    cout << "' '" << endl;
    cout << "' '" << endl;
    cout << "' '" << endl;
    cout << "' '" << endl;
    cout << "' ENTER YOUR DESIRED OPTION: '" << endl;
    cout << "' '" << endl;
    cout << "' Press 1 to record new vehicles '" << endl;
    cout << "' Press 2 to get record of challan '" << endl;
    cout << "' Press 3 to search record of vehicles '" << endl;
    cout << "' Press 4 to search traffic control booths '" << endl;
    cout << "' Press 5 to control the traffic '" << endl;
    cout << "' Press 6 If you require HELP '" << endl;
    cout << "' '" << endl;
    cout << "' Please enter your desired choice: _ '" << endl;

    int ch{0};
    cin >> ch;
    if (ch > 0 && ch < 7)
    {
      switch (ch)
      {
      case 1:
        system("clear");
        recordOfVehicle();
        break;
      case 2:
        system("clear");
        recordOfChallan();
        break;
      case 3:
        system("clear");
        vehSearch();
        break;
      case 4:
        system("clear");
        trafContBooth();
        break;
      case 5:
        system("clear");
        trafContBooth();
        break;
      case 6:
        system("clear");
        helpInfo();
        break;
      }
    }
    else
    {
      cout << "Please enter a valid option !!" << endl;
      delay_0();
      system("clear");
      welcome();
    }
    return 0;
  }
  struct Node
  {
    int info;
    int priority;
    struct Node *prev, *next;
  };

  void push(Node **fr, Node **rr, int n, int p)
  {
    Node *news = (Node *)malloc(sizeof(Node));
    news->info = n;
    news->priority = p;

    if (*fr == NULL)
    {
      *fr = news;
      *rr = news;
      news->next = NULL;
    }
    else
    {
      if (p <= (*fr)->priority)
      {
        news->next = *fr;
        (*fr)->prev = news->next;
        *fr = news;
      }

      else if (p > (*rr)->priority)
      {
        news->next = NULL;
        (*rr)->next = news;
        news->prev = (*rr)->next;
        *rr = news;
      }

      else
      {
        Node *start = (*fr)->next;
        while (start->priority > p)
          start = start->next;
        (start->prev)->next = news;
        news->next = start->prev;
        news->prev = (start->prev)->next;
        start->prev = news->next;
      }
    }
  }

  int peek(Node *fr) { return fr->info; }

  bool isEmpty(Node *fr) { return (fr == NULL); }
  int pop(Node **fr, Node **rr)
  {
    Node *temp = *fr;
    int res = temp->info;
    (*fr) = (*fr)->next;
    free(temp);
    if (*fr == NULL)
      *rr = NULL;
    return res;
  }

  int delay_0()
  {
    using namespace std::this_thread; // sleep_for, sleep_until
    using namespace std::chrono;      // nanoseconds, system_clock, seconds
    sleep_for(nanoseconds(1000000));
    sleep_until(system_clock::now() + seconds(1));
  }
  int delay_1()
  {
    using namespace std::this_thread; // sleep_for, sleep_until
    using namespace std::chrono;      // nanoseconds, system_clock, seconds
    sleep_for(nanoseconds(1000000000));
    sleep_until(system_clock::now() + seconds(1));
  }
  int delay_2()
  {
    using namespace std::this_thread; // sleep_for, sleep_until
    using namespace std::chrono;      // nanoseconds, system_clock, seconds
    sleep_for(nanoseconds(100000000));
    sleep_until(system_clock::now() + seconds(1));
  }

  int minDistance(int dist[], bool sptSet[])
  {

    int min = INT_MAX, min_index;

    for (int v = 0; v < V; v++)
      if (sptSet[v] == false && dist[v] <= min)
        min = dist[v], min_index = v;

    return min_index;
  }

  void printSolution(int dist[])
  {
    cout << "Vertex \t Distance from Source" << endl;
    for (int i = 0; i < V; i++)
      cout << i << " \t\t\t\t" << dist[i] << endl;
  }
  void dijkstra(int graph[V][V], int src)
  {
    int dist[V]; // The output array. dist[i] will hold the

    bool sptSet[V]; // sptSet[i] will be true if vertex i is
    for (int i = 0; i < V; i++)
      dist[i] = INT_MAX, sptSet[i] = false;

    dist[src] = 0;
    for (int count = 0; count < V - 1; count++)
    {
      int u = minDistance(dist, sptSet);
      sptSet[u] = true;
      for (int v = 0; v < V; v++)
        if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX && dist[u] + graph[u][v] < dist[v])
          dist[v] = dist[u] + graph[u][v];
    }
    printSolution(dist);
  }

  int recordOfVehicle()
  {

    cout << "* *" << endl;
    cout << "* HEARTY WELCOME TO *" << endl;
    cout << "* *" << endl;
    cout << "* TRAFFIC MANAGEMENT SYSTEM *" << endl;
    cout << "* *" << endl;
    cout << "* *" << endl;
    cout << "* *" << endl;
    cout << "* *" << endl;
    cout << "* ----Record of Vehicles---- *" << endl;
    cout << "* *" << endl;
    cout << "* Select your desired option :- *" << endl;
    cout << "* *" << endl;
    cout << "* Press 1 to Add a New Vehicle *" << endl;
    cout << "* Press 2 to Search for a Vehicle Using its registration number *" << endl;
    cout << "* Press 3 to search a vehicle through the owner's name *" << endl;
    cout << "* *" << endl;
    cout << "* Press 0 for home *" << endl;
    cout << "* Please enter your desired choice__ *" << endl;
    cout << "* *" << endl;

    int RegofChoice{0};
    cin >> RegofChoice;
    switch (RegofChoice)
    {
    case 0:
      system("clear");
      welcome();
      break;
    case 1:
      recordOfVehicle_1();
      break;
    case 2:
      recordOfVehicle_2();
      break;
    case 3:
      recordOfVehicle_3();
      break;
    }
    return 0;
  }
  int recordOfVehicle_1()
  {
    system("clear");
    fstream fio;
    string line;
    cout << " Hey Welcome to the vehicle registration portal\n"
         << endl;
    cout << "Please enter your vehicle's registration number in the first line" << endl;
    cout << "Please enter the owner's name in the second line " << endl;
    cout << endl
         << "If you want to exit please enter './' ";
    fio.open("/home/lamecodes/CLionProjects/untitled/cmake-build-debug/RecordOfTheVehicles.txt", ios::app | ios::out | ios::in);

    while (fio)
    {

      getline(cin, line);

      if (line == "./")
        break;
      fio << line << endl;
    }
    cout << "Your data has been entered successfully !!" << endl;
    delay_0();
    system("clear");
    recordOfVehicle();
  }
  int recordOfVehicle_2()
  {
    system("clear");
    string path = "/home/lamecodes/CLionProjects/untitled/cmake-build-debug/RecordOfTheVehicles.txt";
    ifstream file(path.c_str());
    if (file.is_open())
    {
      cout << " WELCOME to the registration portal\n";
      cout << endl
           << "Please enter the registration number of the vehicle that you are searching for\n";
      string word;
      cin >> word;
      int count = 0;
      string person;
      while (file >> person)
      {
        if (word == person)
          ++count;
      }
      if (count > 0)
      {
        cout << "The entered registration number " << word << "'is found in our records" << endl;
        int choice;
        cout << endl
             << "Press 1 to go back to the home screen and Press 2 if you want to enter the registration number again ";
        cin >> choice;
        (choice == 1) ? welcome() : recordOfVehicle_2();
      }
      else
      {
        cout << "Sorry!!!! Entered Registration number is not found";
        int choice;
        cout << endl
             << "Press 1 to go back to the home screen and Press 2 if you want to enter the registration number again ";
        cin >> choice;
        (choice == 1) ? welcome() : recordOfVehicle_2();
      }
    }
    else
    {
      cerr << "Error!!!!401!!!\n";
      delay_0();
      welcome();
    }
  }
  int recordOfVehicle_3()
  {
    system("clear");
    string path = "/home/lamecodes/CLionProjects/untitled/cmake-build-debug/RecordOfTheVehicles.txt";
    ifstream file(path.c_str());
    if (file.is_open())
    {
      cout << " WELCOME to the registration portal\n";
      cout << endl
           << "Please enter the name of the owner of the vehicle that you are searching for\n";
      string word;
      cin >> word;
      int countwords = 0;
      string candidate;
      while (file >> candidate) // for each candidate word read from the file
      {
        if (word == candidate)
          ++countwords;
      }
      if (countwords > 0)
      {
        cout << "The Owner's Name " << word << "' has been found successfully in our records." << endl;
        int choice;
        cout << endl
             << "Press 1 to go back to the home screen and Press 2 if you want to enter the owner's name again";
        cin >> choice;
        if (choice == 1)
          welcome();
        else
          recordOfVehicle_2();
      }
      else
      {
        cout << "Sorry!!!!Owner's name is unavailable ";
        int choice;
        cout << endl
             << "Press 1 to go back to the home screen and Press 2 if you want to enter the owner's name again ";
        cin >> choice;
        if (choice == 1)
          welcome();
        else
          recordOfVehicle_2();
      }
    }
    else
    {
      cerr << "Error! 401!\n";
      delay_0();
      welcome();
    }
  }
  int recordOfChallan()
  {

    cout << "* *" << endl;
    cout << "* WELCOME TO *" << endl;
    cout << "* *" << endl;
    cout << "* TRAFFIC MANAGEMENT SYSTEM *" << endl;
    cout << "* *" << endl;
    cout << "* ----Record of Challan---- *" << endl;
    cout << "* *" << endl;
    cout << "* Enter your desired Option :- *" << endl;
    cout << "* *" << endl;
    cout << "* Press 1 to Add a New Challan *" << endl;
    cout << "* Press 2 to search for Challan Using registration number *" << endl;
    cout << "* Press 3 to search for Challan Using Owner's name *" << endl;
    cout << "* *" << endl;
    cout << "* Press 0 if you want to go back to home *" << endl;
    cout << "* Enter your desired choice _ *" << endl;
    cout << "* *" << endl;
    cout << "* *" << endl;
    cout << "* *" << endl;
    cout << "* *" << endl;
    int ROCChoice{0};
    cin >> ROCChoice;
    switch (ROCChoice)
    {
    case 0:
      system("clear");
      welcome();
      break;
    case 1:
      recordOfChallan_1();
      break;
    case 2:
      recordOfChallan_2();
      break;
    case 3:
      recordOfChallan_3();
      break;
    default:
      cout << "please enter a valid case" << endl;
    }
    return 0;
  }
  int recordOfChallan_1()
  {
    system("clear");
    fstream fio;
    string text;
    cout << " Welcome to the CHALLAN MANAGEMENT portal\n"
         << endl;
    cout << "Please enter your vehicle's registration number in the first line " << endl;
    cout << "Please enter owner's name in the second line" << endl;
    cout << endl
         << "If you want to exit please enter './' ";
    // Execute a loop If file successfully Opened
    fio.open("/home/lamecodes/CLionProjects/untitled/cmake-build-debug/RecordOfTheChallan.txt", ios::app | ios::out | ios::in);
    // Execute a loop If file successfully Opened
    while (fio)
    {
      // Read a Line from standard input
      getline(cin, text);
      // Press -1 to exit
      if (text == "./")
        break;
      // Write line in file
      fio << text << endl;
    }
    cout << "Data has been Entered successfully !!" << endl;
    delay_0();
    system("clear");
    recordOfChallan();
  }
  int recordOfChallan_2()
  {
    system("clear");
    string path = "/home/lamecodes/CLionProjects/untitled/cmake-build-debug/RecordOfChallan.txt";
    ifstream file(path.c_str());
    if (file.is_open())
    {
      cout << " Welcome to Challan Management System\n"
           << endl;
      cout << endl
           << "Enter the registration number of the vehicle that you are searching for\n";
      string text;
      cin >> text;
      int countwords = 0;
      string candidate;
      while (file >> candidate)
      {
        if (text == candidate)
          ++countwords;
      }
      if (countwords > 0)
      {
        cout << "The entered Registration number '" << text << "' has been found successfully in our records." << endl;
        int choice;
        cout << endl
             << "Enter 1 to go to home screen and press 2 if you want to enter thr registration number again";
        cin >> choice;
        if (choice == 1)
          welcome();
        else
          recordOfChallan_2();
      }
      else
      {
        cout << "Sorry!!!!Registration number is not found !!";
        int choice;
        cout << endl
             << "Enter 1 to go back to the home screen and Press 2 if you want to enter the registration number again";
        cin >> choice;
        if (choice == 1)
          welcome();
        else
          recordOfChallan_2();
      }
    }
    else
    {
      cerr << "Error!!!! 402!\n";
      delay_0();
      welcome();
    }
  }
  int recordOfChallan_3()
  {
    system("clear");
    string path = "/home/lamecodes/CLionProjects/untitled/cmake-build-debug/RecordOfTheChallanOfVehicles.txt";
    ifstream file(path.c_str());
    if (file.is_open())
    {
      cout << " Welcome to the Challan Management Portal\n"
           << endl;
      cout << endl
           << "Enter the vehicle owner's name that you are searching for\n";
      string text;
      cin >> text;
      int countwords = 0;
      string owner;
      while (file >> owner) // for each candidate word read from the file
      {
        if (text == owner)
          ++countwords;
      }
      if (countwords > 0)
      {
        cout << "The Owner's name " << text << "' has been successfully found in the records." << endl;
        int choice;
        cout << endl
             << "Enter 1 to go back to the home screen and Press 2 if you want to enter the owner's name again";
        cin >> choice;
        if (choice == 1)
          welcome();
        else
          recordOfChallan_3();
      }
      else
      {
        cout << "Owner's name not found !!";
        int choice;
        cout << endl
             << "Enter 1 to go back to the home screen and Press 2 if you want to enter the owner's name again ";
        cin >> choice;
        if (choice == 1)
          welcome();
        else
          recordOfChallan_3();
      }
    }
    else
    {
      cerr << "Error!!!! 402!\n";
      delay_0();
      welcome();
    }
  }
  int vehSearch()
  {
    cout << "* *" << endl;
    cout << "* WELCOME TO *" << endl;
    cout << "* *" << endl;
    cout << "* TRAFFIC MANAGEMENT SYSTEM *" << endl;
    cout << "* *" << endl;
    cout << "* ----Search for the Record of Vehicles---- *" << endl;
    cout << "* *" << endl;
    cout << "* *" << endl;
    cout << "* *" << endl;
    cout << "* Enter The Vehicle's Registration Number *" << endl;
    cout << "* *" << endl;
    cout << "* *" << endl;
    cout << "* Press 0 if you want to go back to home *" << endl;
    cout << "* Enter your desired choice _ *" << endl;
    cout << "* *" << endl;
    string path = "/home/lamecodes/CLionProjects/untitled/cmake-build-debug/RecordOfTheVehicles.txt";
    ifstream file(path.c_str());
    system("clear");
    if (file.is_open())
    {
      string word;
      cin >> word;
      int countwords = 0;
      string candidate;
      while (file >> candidate) // for each candidate word read from the file
      {
        if (word == candidate)
          ++countwords;
      }
      if (countwords > 0)
      {
        cout << "The entered registration number " << word << "' has been found successfully in our records." << endl;
        int choice;
        cout << endl
             << "Enter 1 to go back to the home screen and Press 2 if you want to enter the owner's name again ";
        cin >> choice;
        if (choice == 1)
          welcome();
        else
          vehSearch();
      }
      else
      {
        cout << "Sorry!!!Registration number is not found !!";
        int choice;
        cout << endl
             << "Enter 1 to go back to the home screen and Press 2 if you want to enter the owner's name again ";
        cin >> choice;
        if (choice == 1)
          welcome();
        else
          vehSearch();
      }
    }
    else
    {
      cerr << "Error!!!! 401!\n";
      delay_0();
      welcome();
    }
  }
  int trafContBooth()
  {
    // Traffic Control Booths
    cout << "* WELCOME TO *" << endl;
    cout << "* TRAFFIC MANAGEMENT SYSTEM *" << endl;
    cout << "* ----Traffic Control Booths---- *" << endl;
    cout << "* *" << endl;
    cout << "* Enter your desired Option :- *" << endl;
    cout << "* *" << endl;
    cout << "* 1.Bhubaneswar Traffic Control Booth *" << endl;
    cout << "* 2. Cuttack Traffic Control Booth *" << endl;
    cout << "* 3. Puri Traffic Control Booth *" << endl;
    cout << "* *" << endl;
    cout << "* Press 0 if you want to go back to the home screen *" << endl;
    cout << "* Enter your desired choice _ *" << endl;
    cout << "* *" << endl;
    int TrafficCBChoice{0};
    cin >> TrafficCBChoice;
    switch (TrafficCBChoice)
    {
    case 0:
      system("clear");
      welcome();
      break;
    case 1:
      trafficContBooth_1();
      break;
    case 2:
      trafficContBooth_2();
      break;
    case 3:
      trafficContBooth_3();
      break;
    }
    return 0;
  }
  int trafficContBooth_1()
  {
    system("clear");
    for (int i = 0; i < 100; ++i)
    {
      cout << " Bhubaneswar Traffic Control Booth " << endl;
      cout << "Vehicles that are going out of the city Vehicles that are coming into the city" << endl;
      cout << endl
           << " " << i + 1 << " " << i + 5 << endl;
      delay_0();
      system("clear");
    }
  }
  int trafficContBooth_2()
  {
    system("clear");
    for (int i = 0; i < 100; ++i)
    {
      cout << " Cuttack Traffic Control Booth " << endl;
      cout << "Vehicles that are going out of the city Vehicles that are coming into the city" << endl;
      cout << endl
           << " " << i + 5 << " " << i * 7 << endl;
      delay_0();
      system("clear");
    }
  }
  int trafficContBooth_3()
  {
    system("clear");
    for (int i = 0; i < 100; ++i)
    {
      cout << " Puri Traffic Control Booth " << endl;
      cout << "Vehicles that are going out of the city Vehicles that are coming into the city" << endl;
      cout << endl
           << " " << i * 16 << " " << i * 22 << endl;
      delay_0();
      system("clear");
    }
  }
  int helpInfo()
  {
    // Helpline Information and nearby hospitals
    cout << "* WELCOME TO *" << endl;
    cout << "* TRAFFIC MANAGEMENT SYSTEM *" << endl;
    cout << "* ----Helpline Info And Nearby Healthcare Centres---- *" << endl;
    cout << "* *" << endl;
    cout << "* Enter Your desired Option :- *" << endl;
    cout << "* *" << endl;
    cout << "* Press 1 to get the helpline number *" << endl;
    cout << "* Press 2 to get info of the hospitals in Cuttack *" << endl;
    cout << "* Press 2 to get info of the hospitals in Puri *" << endl;
    cout << "* *" << endl;
    cout << "* Press 0 if you want to go back to home *" << endl;
    cout << "* Enter your desired choice___ *" << endl;
    cout << "* *" << endl;
    int CChoice{0};
    cin >> CChoice;
    switch (CChoice)
    {
    case 0:
      system("clear");
      welcome();
      break;
    case 1:
    {
      system("clear");
      string para;
      ifstream myfile("/home/lamecodes/CLionProjects/untitled/cmake-build-debug/HelplineNumbers.txt");
      if (myfile.is_open())
      {
        while (getline(myfile, para))
        {
          cout << para << '\n';
        }
        myfile.close();
      }
      else
        cout << "Error!!!! 403!";
      int ch;
      cout << endl
           << "Press 1 if you want to go back to the Home Page" << endl;
      cin >> ch;
      if (ch == 1)
      {
        system("clear");
        welcome();
      }
      else
      {
        cout << endl
             << "Please Enter Valid option !!";
        cout << endl
             << endl
             << "Press 1 if you want to go back to the Home Page" << endl;
        cin >> ch;
        if (ch == 1)
        {
          system("clear");
          welcome();
        }
      }
      break;
    }
    case 2:
    {
      system("clear");
      string line;
      ifstream myfile("/home/lamecodes/CLionProjects/untitled/cmake-build-debug/HCuttack.txt");
      if (myfile.is_open())
      {
        while (getline(myfile, line))
        {
          cout << line << '\n';
        }
        myfile.close();
      }
      else
        cout << "Error!!!! 403!";
      int ch;
      cout << endl
           << "Press 1 if you want to go back to the Home Page" << endl;
      cin >> ch;
      if (ch == 1)
      {
        system("clear");
        welcome();
      }
      else
      {
        cout << endl
             << "Please Enter Valid option !!";
        cout << endl
             << endl
             << "Press 1 if you want to go back to the Home Page" << endl;
        cin >> ch;
        if (ch == 1)
        {
          system("clear");
          welcome();
        }
      }
    }
    break;
    case 3:
    {
      system("clear");
      string line;
      ifstream myfile("/home/lamecodes/CLionProjects/untitled/cmake-build-debug/HPuri.txt");
      if (myfile.is_open())
      {
        while (getline(myfile, line))
        {
          cout << line << '\n';
        }
        myfile.close();
      }
      else
        cout << "Error!!!! 403!";
      int ch;
      cout << endl
           << "Press 1 if you want to go back to the Home Page" << endl;
      cin >> ch;
      if (ch == 1)
      {
        system("clear");
        welcome();
      }
      else
      {
        cout << endl
             << "Please Enter Valid option !!";
        cout << endl
             << endl
             << "Press 1 if you want to go back to the Home Page" << endl;
        cin >> ch;
        if (ch == 1)
        {
          system("clear");
          welcome();
        }
      }
      break;
    }
    }
    return 0;
  }
};
int main()
{
  TrafficManagementSystem ob1;
  ob1.welcome();
}
