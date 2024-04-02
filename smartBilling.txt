#include <iostream>
#include <fstream>
#include <sstream>
#include <cstdlib>
#include <limits>

using namespace std;

class Shopping
{
private:
	int pcode;
	float price;
	float dis;
	string pname;

public:
	void menu();
	void administrator();
	void buyer();
	void add();
	void edit();
	void rem();
	void list();
	void receipt();
	bool validateCredentials(const string &email, const string &password);
};

void clearInputBuffer()
{
	cin.clear();
	cin.ignore(numeric_limits<streamsize>::max(), '\n');
}

bool Shopping::validateCredentials(const string &email, const string &password)
{
	// Perform actual authentication logic here
	return (email == "ani@gmail.com" && password == "polam123");
}

void Shopping::menu()
{
	int choice;
	string email, password;

	while (true)
	{
		cout << "\n\t\t\t\t______________________________________\n";
		cout << "\t\t\t\t                                      \n";
		cout << "\t\t\t\t          Supermarket Main Menu       \n";
		cout << "\t\t\t\t                                      \n";
		cout << "\t\t\t\t______________________________________\n";
		cout << "\t\t\t\t                                      \n";
		cout << "\t\t\t\t|  1) Administrator   |\n";
		cout << "\t\t\t\t|                     |\n";
		cout << "\t\t\t\t|  2) Buyer           |\n";
		cout << "\t\t\t\t|                     |\n";
		cout << "\t\t\t\t|  3) Exit            |\n";
		cout << "\t\t\t\t|                     |\n";
		cout << "\n\t\t\t Please select: ";
		cin >> choice;

		switch (choice)
		{
		case 1:
			cout << "\t\t\t Please Login \n";
			cout << "\t\t\t Enter Email: ";
			cin >> email;
			cout << "\t\t\t Password: ";
			cin >> password;
			if (validateCredentials(email, password))
			{
				administrator();
			}
			else
			{
				cout << "Invalid email/password\n";
				clearInputBuffer();
			}
			break;

		case 2:
			buyer();
			break;

		case 3:
			exit(0);

		default:
			cout << "Please select from the given options\n";
			clearInputBuffer();
		}
	}
}

void Shopping::administrator()
{
	int choice;
	while (true)
	{
		cout << "\n\n\n\t\t\t Administrator menu";
		cout << "\n\t\t\t|_____1) Add the product_____|";
		cout << "\n\t\t\t|                            |";
		cout << "\n\t\t\t|_____2) Modify the product__|";
		cout << "\n\t\t\t|                            |";
		cout << "\n\t\t\t|_____3) Delete the product__|";
		cout << "\n\t\t\t|                            |";
		cout << "\n\t\t\t|_____4) Back to main menu___|";

		cout << "\n\n\t Please enter your choice ";
		cin >> choice;

		switch (choice)
		{
		case 1:
			add();
			break;

		case 2:
			edit();
			break;

		case 3:
			rem();
			break;

		case 4:
			return;

		default:
			cout << "Invalid choice!";
			clearInputBuffer();
		}
	}
}

void Shopping::buyer()
{
	int choice;
	while (true)
	{
		cout << "\t\t\t  Buyer \n";
		cout << "\t\t\t_____________  \n";
		cout << "                     \n";
		cout << "\t\t\t1) Buy product \n";
		cout << "                     \n";
		cout << "\t\t\t2) Go back     \n";
		cout << "\t\t\t Enter your choice : ";

		cin >> choice;

		switch (choice)
		{
		case 1:
			receipt();
			break;

		case 2:
			return;

		default:
			cout << "Invalid choice\n";
			clearInputBuffer();
		}
	}
}

void Shopping::add()
{
	fstream data;
	int token = 0;
	float p;
	float d;
	string n;

	cout << "\n\n\t\t\t Add new product";
	cout << "\n\n\t Product code of the product: ";
	cin >> pcode;
	cout << "\n\n\t Name of the product: ";
	cin >> pname;
	cout << "\n\n\t Price of the product:";
	cin >> price;
	cout << "\n\n\t Discount on product:";
	cin >> dis;
	data.open("database.txt", ios::in);

	if (!data)
	{
		data.open("database.txt", ios::app | ios::out);
		data << pcode << " " << pname << " " << price << " " << dis << "\n";
		data.close();
	}
	else
	{
		int c;
		string line;
		while (getline(data, line))
		{
			stringstream ss(line);
			ss >> c >> n >> p >> d;
			if (c == pcode)
			{
				token++;
				break;
			}
		}
		data.close();

		if (token == 1)
			cout << "\n\n\t\t Product code already exists!";
		else
		{
			data.open("database.txt", ios::app | ios::out);
			data << pcode << " " << pname << " " << price << " " << dis << "\n";
			data.close();
			cout << "\n\n\t\t Record inserted !";
		}
	}
}

void Shopping::edit()
{
	fstream data, data1;
	int pkey;
	int token = 0;
	int c;
	float p;
	float d;
	string n;

	cout << "\n\t\t\t Modify the record";
	cout << "\n\t\t\t Product code :";
	cin >> pkey;

	data.open("database.txt", ios::in);
	if (!data)
	{
		cout << "\n\nFile doesn't exist! ";
	}
	else
	{
		data1.open("database1.txt", ios::app | ios::out);

		while (data >> c >> n >> p >> d)
		{
			if (pkey == c)
			{
				cout << "\n\t\t Product new code :";
				cin >> c;
				cout << "\n\t\t Name of the product :";
				cin >> n;
				cout << "\n\t\t Price :";
				cin >> p;
				cout << "\n\t\t Discount :";
				cin >> d;
				data1 << c << " " << n << " " << p << " " << d << "\n";
				cout << "\n\n\t\t Record edited ";
				token++;
			}
			else
			{
				data1 << c << " " << n << " " << p << " " << d << "\n";
			}
		}
		data.close();
		data1.close();

		remove("database.txt");

		rename("database1.txt", "database.txt");

		if (token == 0)
		{
			cout << "\n\n Record not found sorry!";
		}
	}
}

void Shopping::rem()
{
	fstream data, data1;
	int pkey;
	int token = 0;
	cout << "\n\n\t Delete product";
	cout << "\n\n\t Product code :";
	cin >> pkey;
	data.open("database.txt", ios::in);
	if (!data)
	{
		cout << "File doesn't exist";
	}
	else
	{
		data1.open("database1.txt", ios::app | ios::out);
		int c;
		string n;
		float p, d;
		while (data >> c >> n >> p >> d)
		{
			if (c == pkey)
			{
				cout << "\n\n\t Product deleted successfully";
				token++;
			}
			else
			{
				data1 << c << " " << n << " " << p << " " << d << "\n";
			}
		}
		data.close();
		data1.close();
		remove("database.txt");
		rename("database1.txt", "database.txt");
		if (token == 0)
		{
			cout << "\n\n Record not found";
		}
	}
}

void Shopping::list()
{
	fstream data;
	data.open("database.txt", ios::in);
	cout << "\n\n|___________________________________________________________\n";
	cout << "ProNo\t\tName\t\tPrice\n";
	cout << "\n\n|___________________________________________________________\n";
	int c;
	string n;
	float p, d;
	while (data >> c >> n >> p >> d)
	{
		cout << c << "\t\t" << n << "\t\t" << p << "\n";
	}
	data.close();
}

void Shopping::receipt()
{
	system("cls");
	fstream data;

	int arrc[100], arrq[100];
	char choice;
	int c = 0;
	float amount = 0;
	float total = 0;

	cout << "\n\n\t\t\t Receipt ";
	data.open("database.txt", ios::in);
	if (!data)
	{
		cout << "\n\n Empty database";
	}
	else
	{
		data.close();
		list();
		cout << "\n____________________________\n";
		cout << "\n|                            |";
		cout << "\n|    Please place the order  |";
		cout << "\n|____________________________|\n";
		do
		{
			cout << "\n\n Product Code : ";
			cin >> arrc[c];
			cout << "\n Product Quantity : ";
			cin >> arrq[c];
			for (int i = 0; i < c; i++)
			{
				if (arrc[c] == arrc[i])
				{
					cout << "\n\n Duplicate Product Code. Please try again!";
					clearInputBuffer(); // Clear input buffer to handle invalid inputs
					continue;
				}
			}
			c++;
			cout << "\n\n Want to buy another product? Press y for yes and n for no : ";
			cin >> choice;
		} while (choice == 'y');
		system("cls");

		cout << "\n\n\t\t\t__________RECEIPT______________\n";
		cout << "\nProduct Num.\tProduct Name\tQuantity \tPrice \tAmount \tAmount with discount\n";

		for (int i = 0; i < c; i++)
		{
			data.open("database.txt", ios::in);
			int pc;
			string pn;
			float pr, di;
			while (data >> pc >> pn >> pr >> di)
			{
				if (pc == arrc[i])
				{
					amount = pr * arrq[i];
					total += amount;
					cout << "\n"
						 << pc << "\t\t" << pn << "\t\t" << arrq[i] << "\t\t" << pr << "\t" << amount << "\t\t" << amount - (amount * di / 100);
				}
			}
			data.close();
		}
		cout << "\n\n----------------------------------------";
		cout << "\n Total Amount : " << total;
	}
}
int main()
{
	Shopping s;
	s.menu();
	return 0;
}
