#include<iostream>
#include<conio.h>
#include<cstdlib>
#include<cstring>
#include<cstdio>
#include<windows.h>
#include<unistd.h>
using namespace std;

class employee{
    private:
        char name[30];
        char id[5];
        char designation[10];
        int age;
        int experience;
        
        void listEmployees(){
            system("cls");
            FILE *file;
            file = fopen("data.txt", "r");
            cout << "\n\t\t\t     List of Employees";
            cout << "\n----------------------------------------------------------------------------------";
            cout << "\nNAME\t|\tID\t|\tDESIGNATION\t|\tAge\t|\tExperience";
            cout << "\n----------------------------------------------------------------------------------";
            
			while(fscanf(file, "%s %s %s %d %d", &name[0], &id[0] , &designation[0], &age, &experience)!= EOF)
            	cout << "\n"<<name<<"\t\t"<<id<<"\t\t"<<designation<<"\t\t"<<age<<"\t\t"<<experience;
            
			fclose(file);
        }

        void showDetails(){
            system("cls");
            FILE *file;
            char checkId[5];
            cout << "\n\nEnter Employee ID: ";
            cin >> checkId;
            file = fopen("data.txt", "r");
            
			while(fscanf(file, "%s %s %s %d %d", &name[0], &id[0] , &designation[0], &age, &experience)!=EOF){
			    if(strcmp(checkId,id) == 0){
                	cout << "\n---------------------";
                    cout << "\nName: " << name;
                    cout << "\n---------------------";
                    cout << "\nId: "<<id;
                    cout << "\n---------------------";
                    cout << "\nDesignation: "<<designation;
                    cout << "\n---------------------";
                    cout << "\nAge: "<<age;
                    cout << "\n---------------------";
                    cout << "\nExperience: "<<experience;
                    cout << "\n---------------------";
                }
        	}
            fclose(file);
        }

        void editExisting(){
            system("cls");
            char checkId[5], newDesignation[10];
            int newExperience;
			cout << "\nEnter employee id: ";
            cin >> checkId;
            cout << "\n-----------------------------";
            cout << "\nEnter new designation: ";
            cin >> newDesignation;
            cout << "------------------------------";
            cout << "\nEnter new Experience: ";
            cin >> newExperience;
            
			FILE *file, *tempfile;
            file = fopen("data.txt", "r");
            tempfile = fopen("temp.txt", "w");
            
			while(fscanf(file, "%s %s %s %d %d", &name[0], &id[0] , &designation[0], &age, &experience)!=EOF){
                if(strcmp(checkId, id) == 0)
                    fprintf(tempfile, "%s %s %s %d %d \n", name, id, newDesignation, age, newExperience);
                else
                    fprintf(tempfile, "%s %s %s %d %d \n", name, id, designation, age, experience );
            }
            
			fclose(file);
            fclose(tempfile);
            int isRemoved= remove("data.txt");
            int isRenamed= rename("temp.txt", "data.txt");
        }

        void addNewEmployee(){
            system("cls");
            cout << "\n----------------------------------------";
            cout << "\n Enter First Name of Employee: ";
            cin >> name;
            cout << "\n----------------------------------------";
            cout << "\n Enter Employee ID [max 4 digits]: ";
            cin >> id;
            cout << "\n----------------------------------------";
            cout << "\n Enter Designation: ";
            cin >> designation;
            cout << "\n----------------------------------------";
            cout << "\n Enter Employee Age: ";
            cin >> age;
            cout << "\n----------------------------------------";
            cout << "\n Enter Employee Experience: ";
            cin >> experience;
            cout << "\n----------------------------------------";

            char ch;
            cout << "\nEnter 'y' to save above information and 'n' to cancel above information: ";
            cin >> ch;
            
			if(ch == 'y'){
                FILE *file;
                file = fopen("data.txt","a");
                fprintf(file, "%s %s %s %d %d \n", name, id, designation, age, experience);
                fclose(file);
                cout << "\nNew Employee has been added to database\n";
                
				char choice;
                cout << "Enter 'y' to add more employee details and 'n' to go to menu page: ";
                cin >> choice;
                
                switch(choice){
                	case 'y': 	system("cls");
                				addNewEmployee();		break;
                	case 'n':	system("cls");       	
            					options();				break;
            		default:	cout << "Wrong Input";	break;
				}
            }
            else if(ch == 'n'){
				system("cls");       	
            	options();
            }
            
			else
            	cout << "Wrong Input";
        }

        void deleteEmployeeDetails(){
            system("cls");
            char checkId[10];
            cout << "\n----------------------------------";
            cout << "\nEnter Employee Id To Remove: ";
            cin >> checkId;
            char ch;
            cout << "----------------------------------";
            cout<<"\n\n\n\n\nCONFIRMATION\nEnter 'y' To Confirm Deletion and 'n' to cancel above information\n";
            cin >> ch;
            
			if(ch == 'y'){
                FILE *file, *tempfile;
                file = fopen("data.txt", "r");
                tempfile = fopen("temp.txt", "w");
                
				while(fscanf(file, "%s %s %s %d %d", &name[0], &id[0] , &designation[0], &age, &experience)!=EOF){
				    if(strcmp(checkId, id)!=0)
                        fprintf(tempfile, "%s %s %s %d %d \n", name, id, designation, age, experience );
            	}
            	
				fclose(file);
                fclose(tempfile);
                int isRemoved = remove("data.txt");
                int isRenamed = rename("temp.txt", "data.txt");
                cout << "\nRemoved Successfully\n";
            }
            else if(ch == 'n'){
				system("cls");       	
            	options();
            }
            else
            	cout << "Wrong Input";
        }


    public:
        void options(){
            cout<<"\n\t\t\t>>>>>>>>>  EMPLOYEE MANAGEMENT SYSTEM  <<<<<<<<<";
            cout<<"\n\t\t\t------------------------------------------------";
            cout<<"\n\t\t\tENTER   1:   To View List of Employees";
            cout<<"\n\t\t\t------------------------------------------------";
            cout<<"\n\t\t\tENTER   2:   To View Employee Details";
            cout<<"\n\t\t\t------------------------------------------------";
            cout<<"\n\t\t\tENTER   3:   To Modify Existing Employee Details";
            cout<<"\n\t\t\t------------------------------------------------";
            cout<<"\n\t\t\tENTER   4:   To Add New Employee Details";
            cout<<"\n\t\t\t------------------------------------------------";
            cout<<"\n\t\t\tENTER   5:   To Remove an Employee Details";
            cout<<"\n\t\t\t------------------------------------------------";
            cout<<"\n\t\t\tPlease Enter Your Choice: ";

            int choice;
            cin >> choice;
            switch (choice){
                case 0:		system("CLS");
        	        		cout<<"\n\nThanks for using Employee Management System\n\n";
                    		Sleep(10);					return;
                case 1:		listEmployees();			break;
                case 2:		showDetails();				break;
                case 3:		editExisting();				break;
                case 4:		addNewEmployee();			break;
                case 5:		deleteEmployeeDetails();	break;
                default:	cout<<"\n Sorry! I don't understand that! \n";	break;
            }
        }
};

int main(){
    employee e;
    e.options();
    return 0;
}
