#include<stdio.h>
#include<stdlib.h>
#include<string.h>

struct book
{
	char code[10];
	char name[50];
	char date[30];
	int cost;
	

}b;

int seat = 100;

void insert_Details();//for inserting movie details
void view_All(); // for view all data 
void find(); // for finding data
void book_Ticket(); //for booking tickets
void old_Record(); //for view old recorrds of users,booked tickets

int main()
{

	 int choice;
 	do{
	printf("\n====================================================================");
	printf("\n");	
	printf("\t  Welcome to Moive Ticket Booking System ");
	printf("\n");
	printf("\n====================================================================");
	
	printf("\nPress <1> Insert Movie");
   	printf("\nPress <2> View All Movie");
	printf("\nPress <3> Find Movie ");
	printf("\nPress <4> Book Ticket");
	printf("\nPress <5> View All Previous Transactions");
   	printf("\nPress <0> Exit ");

   	printf("\nEnter your Choice ::");
   	scanf("%d",&choice); 	

   	switch (choice)
   	{
    		case 1 :
    		insert_Details();
   		break;
		case 2:
    		view_All();
   		break;
    		
		case 3:
    		find();
   		break;

		case 4:
		book_Ticket();
		break;
		
		case 5:
		old_Record();
		break;

    		case 0:
    		exit(0);
    		break;

    		default:
    		printf("Wrong choice! Sorry For Your Inconvenience");
    		break;
   	}
 }while(choice!=0);

	


}


void insert_Details()
{
	
	FILE *fp;
	struct book b;
	printf("Enter the Movie code :- ");	
	scanf("%s",b.code);
	printf("Enter the Movie name :- ");	
	scanf("%s",b.name);
	printf("Enter the Release Date:- ");	
	scanf("%s",b.date);
	printf("Enter the Ticket Price:- ");	
	scanf("%d",&b.cost);
	
	fp=fopen("data.txt","a");

	if(fp == NULL)
	{
		printf("FIle not Found");
	}
	else
	{
		fprintf(fp,"%s %s %s %d \n",b.code,b.name,b.date,b.cost);
		printf("Record insert Sucessfull");
	}
		printf("\n");
	fclose(fp);
}
void find() //find details
{
	struct book b;
	FILE *fp;
	
	char ch[20];
	printf("Enter movie code :");
	scanf("%s",ch);
	//system("clear");
	fp = fopen("data.txt","r");
	if(fp == NULL)
	{
		printf("file does not found !");
		exit(1);

	}
	else
	{	
		while(getc(fp) != EOF)
		{
			fscanf(fp,"%s %s %s %d",b.code,b.name,b.date,&b.cost);
			if(strcmp(b.code,ch) == 0)
			{	
				//printf("%s / %s / %s / %d\n",b.code,b.name,b.date,b.cost);
				printf("\n Record Found\n");
				printf("\n\t\tCode ::%s",b.code);
				printf("\n\t\tmovie name ::%s",b.name);
				printf("\n\t\tmovie date ::%s",b.date);
				printf("\n\t\tprice of ticket ::%d",b.cost);
				break;
			}
		}
		
	}

	fclose(fp);
}
void view_All()
{
	char ch;
	FILE *fp;

	fp = fopen("data.txt","r");
	if(fp == NULL)
	{
		printf("file does not found !");
		exit(1);

	}
	else
	{	
		system("clear");
		while( ( ch = fgetc(fp) ) != EOF )
      		printf("%c",ch);
		
	}
	fclose(fp);
}
//for ticket booking 
void book_Ticket()
{
 struct book b;
	FILE *fp;

	FILE *ufp;

	int total_seat,mobile,total_amount;
	char name[50];

	
	char choice; //used in display all movies
	char movie_code[20]; //for searching

	// disply all moives by default for movie code
	fp = fopen("data.txt","r");
	if(fp == NULL)
	{
		printf("file does not found !");
		exit(1);

	}
	else
	{	
		system("cls");
		while( ( choice = fgetc(fp) ) != EOF )
      		printf("%c",choice);
		
	}
	fclose(fp);
	
	//display ends
	printf("\n For Booking ticket Choose Movie(Enter Movie Code ,First Latter Of Movie)\n");
	printf("\n Enter movie code :");
	scanf("%s",movie_code);
	//system("clear");
	fp = fopen("data.txt","r");
	if(fp == NULL)
	{
		printf("File  not found !");
		exit(1);

	}
	else
	{	
		while(getc(fp) != EOF)
		{
			fscanf(fp,"%s %s %s %d",b.code,b.name,b.date,&b.cost);
			if(strcmp(b.code,movie_code) == 0)
			{	
				//printf("%s / %s / %s / %d\n",b.code,b.name,b.date,b.cost);
				printf("\n Record Found\n");
				printf("\n\t\tCode ::%s",b.code);
				printf("\n\t\tMovie name ::%s",b.name);
				printf("\n\t\tdate name ::%s",b.date);
				printf("\n\t\tPrice of ticket::%d",b.cost);
			}
		}
		
	}
	printf("\n* Fill Deatails  *");
	printf("\n Your name :");
	scanf("%s",name);
	printf("\n Mobile number :");
	scanf("%d",&mobile);
	printf("\n Total number of tickets :");
	scanf("%d",&total_seat);
	
	
	total_amount = b.cost * total_seat;
	
	printf("\n ***** ENJOY MOVIE ****\n");
	printf("\n\t\tname : %s",name);
	printf("\n\t\tmobile Number : %d",mobile);
	printf("\n\t\tmovie name : %s",b.name);
	printf("\n\t\tTotal seats : %d",total_seat);
	printf("\n\t\tcost per ticket : %d",b.cost);
	printf("\n\t\tTotal Amount : %d",total_amount);
	
	
	ufp=fopen("oldTransaction.txt","a");
	if(ufp == NULL)
	{
		printf("FIle not Found");
	}
	else
	{
		fprintf(ufp,"%s %d %d %d %s %d \n",name,mobile,total_seat,total_amount,b.name,b.cost);
		printf("\n Record insert Sucessfull to the old record file");
	}
		printf("\n");
	fclose(ufp);
	fclose(fp);

}
//for view all user transections
void old_Record()
{
	char choice;
	FILE *fp;

	//system("clear");
	
	fp = fopen("oldTransection.txt","r");
	if(fp == NULL)
	{
		printf("file does not found !");
		exit(1);

	}
	else
	{	
		system("clear");
		while( ( choice = fgetc(fp) ) != EOF )
      		printf("%c",choice);
		
	}
	fclose(fp);


}


  
