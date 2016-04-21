//Homework 5
// Homework5
//Homework Five - Sorting 


//Ruqaiyah Safiyuddin

#include <iostream>
#include <stack>
#include <fstream>
#include <vector>
#include <cstring>
#include <stdio.h>
#include <cstdlib>
#include <stdlib.h>
#include <string>
#include <sstream>
using namespace std;

//global variables to be used in recursive functions and then their values can be printed out in main.
int quickCompare=0;
int quickSwap=0;
int mergeCompare=0;
int mergeSwap=0;

	void bubbleSort(int A[], int n);

	void insertionSort(int A[], int n);

	void shellSort(int A[], int n);

	void mergeSort(int A[], int low, int high);
	void merge (int A[], int low, int high, int mid);

	void quicksort(int a[], int left, int right);
	int partition(int a[], int left, int right, int pivotIndex);

	int *readIntsFile (char * file_name);


	int * readIntsFile(char * file_name)
	{
		//this function will read in the integers from the specified file, and put them into an array
	    ifstream in;
	    in.open (file_name);

	    if(!in.is_open())
	    {
	        cout << "The file could not be opened.";
	        return 0;
	    }

	    //reading in the file
	    string temp;
	    string contents="";

	    while(in.peek()!=EOF)
	    {
	        in>>temp;
	        contents += temp + " ";

	    }

	    in.clear(); in.close();

	    //putting the file into a vector

	    vector<string> lines;
	    temp= "";
	    for(int i = 0; i< contents.length();i++)
	    {
	        if (contents[i] == ' ')
	        {
	            lines.push_back(temp);
	            temp= "";

	        }
	        else
	            temp +=contents[i];
	    }

	    //creating an array of all the integers which we will then return
	    //useful to do it as a vector and then array because not sure about how many ints are in file.
	    int * myInts = new int[lines.size()];

	    for(int i=0; i<lines.size(); i++)
	    {
	        myInts[i]=atoi(lines[i].c_str());
	    }
	    return myInts;
	}


void bubbleSort(int A[], int n)
{
	//bubble sort function
	//parameters are the array, and the number of elements in the array.
	cout<<"Bubble Sort"<<endl;
	int comparison =0;//initialize comparisons variable
	int swaps =0;//initialize swaps variable
	int i, j, temp;

	for (i=1; i<n; i++)// this for loop goes through the number of passes
	{
		for (j=0;j<n-1;j++)
		{
			comparison++;//increment the comparison variable because a comparison is occurring here
			if (A[j]>A[j+1])//here we are comparing 2 adjacent numbers in the array
			{

				//here we are swapping the value at index A[j] with the value at A[j+1]
				temp = A[j];
				A[j]=A[j+1];
				A[j+1]= temp;
				swaps++;//increment the swap variable because a swap is occurring here.
			}

		}
	}
	for(int k = 0; k< n; k++){
//this for loop goes through the sorted array and prints it
		//this is used for testing purposes in order to make sure the array has in fact been sorted.
	     cout<<A[k]<<endl;
	}

	cout <<"Number of comparisons = "<<comparison<<endl;//print nujmber of comparisons
	cout<<"Number of swaps = "<<swaps<<endl;//print number of swaps

}

void insertionSort(int A[], int n)
{
//insertion sort function
	cout<<"Insertion Sort"<<endl;
	int comparison=0;//initialize comparison variable to 0
	int swap=0;//iniitialize swap variable to 0
	int i, j, element;
	for (i=1;i<n;i++)
		//for loop to go through all the elements in the array
	{
		element = A[i];
		//initialize element as the the value at index i of the array (according to the for loop)
		j=i;

		comparison++;//to account for the time when you do the comparison and it doesn't hold
		//so you don't go into the while loop but you have still made a comparison


		while ( (j>0)&& (A[j-1]>element ))
		{
		//we are comparing if the value at index A[j-1]is more than the element
		//if so then the while loop occurs
		comparison ++;//increment the comparison variable because there is a comparison occurring
		A[j]=A[j-1];
		j=j-1;
		//here we are shifting the elements

		//swap++;
		}

		A[j]=element;//making the value of A[j] equal to element after the while loop is done comparing
		//in other words we are putting the element at the jth position
		swap++; //that's a swap and so we increment the swap variable.
	}
	cout <<"Number of comparisons = "<<comparison<<endl;//print number of comparisons
	cout<<"Number of swaps = "<<swap<<endl;//print number of swaps

	for(int k = 0; k< n; k++){
//for loop to print out the sorted array for testing purposes
		     cout<<A[k]<<endl;
		}

}

void shellSort(int A[], int n)
{
//shell sort function
	//shell sort makes repetitive use of insertion sort
cout<<"Shell Sort"<<endl;

int comparison = 0;//initialize comparison variable
int	swap = 0;//initialize swap variable
	int temp, gap, i;
	int swapped;
	gap = n/2;//create a fixed distance as to where to compare elements
	do
	{
		do
		{
			swapped =0;
			for (i=0;i<n-gap;i++)
			{
				//for loop to go through all the elements that occur before n-1
				comparison++;//increment comparison variable because there is a comparison occurring below


				if (A[i]>A[i+gap])//check to see if the value at index A[i]>A[i+gap]
				{

					//if this is the case then a swap will occur.
					temp = A[i];
					swap ++;//incrementing the swap variable
					A[i]=A[i+gap];
					A[i+gap]=temp;
					swapped=1;

				}
			}
			}while (swapped==1);

	}while((gap=gap/2)>=1);//gap variable decremented

	//do while loop because elements at fixed gaps are compared
	//the gap is decremented by some value at each pass
	//gap goes from n/2 to n/4 to n/8 until it =1 - condition of the while loop

	cout <<"Number of comparisons = "<<comparison<<endl;//print out the number of comparisons
		cout<<"Number of swaps = "<<swap<<endl;//print out the number of swaps

		for(int k = 0; k< n; k++){
//for loop used to print every element in the sorted array to make sure that the array has been sorted correctly
		     cout<<A[k]<<endl;
		}
}

void merge (int A[], int low, int high, int mid)
{
	//this function will be called in the mergesort function
	//merging combines sorted files into a third sorted file.


	int i, j, k;
	int * C;
	C = new int [high+1];
	i=low;//initializing index for first part
	j=mid+1;//initializing index for second part
	k=0;//index for array C
	while ((i<=mid) && (j<=high))
	{
		//here we will merge the 2 arrays into array C
		mergeCompare++;//incrementing the global variable because comparison occurring
		if (A[i]<A[j])//looking at whether the value contained at index A[i] is greater than that at A[j]
		{

			C[k]=A[i++];
		}
		else
		{

		C[k] = A[j++];
		}
		k++;
	}
	while (i<=mid)

		C[k++]= A[i++];


	while (j<=high)



		C[k++]=A[j++];

	for (i=low, j=0; i<=high; i++, j++)
	{
		//for loop used in order to array C back into array A
		A[i]=C[i];

	}
}

void mergeSort(int A[], int low, int high)
{
	//this function calls the merge function
	//recursive function

	int mid;
	if (low<high)
	{
		mid = (low+high)/2;//setting the value of mid
		//recursive functions to do sorting
		mergeSort(A, low, mid);
		mergeSort(A,mid+1,high);
		merge(A,low, high, mid);

	}
	//note: I do not think that there are any actual swaps going on
	//rather there is just merging
	//therefore I have not incremented the swap variable anywhere
	//because whatever file is sorted, swap=0 always

}


int partition(int a[], int left, int right, int pivotIndex)
{

	//partition function which will be used in the quick sort function

	int pivot = a[pivotIndex];//initialize pivot to the value stored at a[pivotIndex]
    do
    {
        quickCompare++;//to account for the comparison when you don't go inside of the while loop
        //because you  have still made a comparison
    	while (a[left] < pivot) {//while loop that runs while the value at index a[left] is less than pivot
        	left++;//incrementing the value of left
        quickCompare++;//since there is a comparison going on, increment global comparison variables
        }


    	quickCompare++;//to account for the comparison when you don't go inside the while loop
        while (a[right] > pivot)//while loop that runs while the value at index a{right} is more than pivot
        {
        	right--;//incrementing the value of right

        quickCompare++;//since there is a comparison going on, increment the global comparison variable
        }



        quickCompare++;//incrementing the global comparison variable because
        if (left < right && a[left] != a[right])//you are comparing whether 2 values don't equal each other
        {
            swap(a[left], a[right]);//inbuilt swap function that swaps the values at these 2 indices
            quickSwap++;//incrementing swap variable because a swap has occurred
        }
        else//if the 2 values at the indexes specified to equal each other then return right.
        {
            return right;
        }
    }
    while (left < right);

    return right;
}


void quicksort(int a[], int left, int right)
{
	//quick sort function
	//this is a recursive function
	//this function will call the partition function

	//cout<<"Quick Sort"<<endl;


	if (left < right)
    {

        int pivot = (left + right) / 2; // middle
        int pivotNew = partition(a, left, right, pivot);
        quicksort(a, left, pivotNew - 1);
        quicksort(a, pivotNew + 1, right);
    }

}


int main() {

//creating my own small arrays for testing purposes.
	int numbers1[] = {76, 67, 36,55,24,14,6,1};
	int nums[] = {1,13,4,6,4,33,5,25,7,1};
	int test[]={1,2,3,4,5};

	//shellSort(test,5);//testing shellSort function
	//insertionSort(nums,10);//testing insertionSort function
	//insertionSort(test,5);
//The code below is used to test all the functions, each with the 4 files.



	shellSort(readIntsFile("Random.txt"), 10000);
	shellSort(readIntsFile("FewUnique.txt"), 10000);
	shellSort(readIntsFile("Reversed.txt"), 10000);
	shellSort(readIntsFile("NearlySorted.txt"), 10000);


	bubbleSort(readIntsFile("Random.txt"),10000);
	bubbleSort(readIntsFile("FewUnique.txt"),10000);
	bubbleSort(readIntsFile("Reversed.txt"),10000);
	bubbleSort(readIntsFile("NearlySorted.txt"),10000);



	insertionSort(readIntsFile("Random.txt"), 10000);
	insertionSort(readIntsFile("FewUnique.txt"), 10000);
	insertionSort(readIntsFile("Reversed.txt"), 10000);
	insertionSort(readIntsFile("NearlySorted.txt"), 10000);
	cout<<"Quick Sort"<<endl;
	//note, can only call one quick sort function at a time because of global variable increasing
	quicksort(readIntsFile("Random.txt"),0,9999);//going from 0 to 9999 as these are the array indices
	quicksort(readIntsFile("FewUnique.txt"),0,9999);
	quicksort(readIntsFile("Reversed.txt"),0,9999);
	quicksort(readIntsFile("NearlySorted.txt"),0,9999);
	cout <<"Number of comparisons = "<<quickCompare<<endl;
	cout<<"Number of swaps = "<<quickSwap<<endl;


	cout<<"Merge Sort"<<endl;
	//note can only call one merge sort function at a time becasue of global variable increasing

	mergeSort(readIntsFile("Random.txt"),0,9999);//going from 0 to 9999 as these are the array indeces
	mergeSort(readIntsFile("FewUnique.txt"),0,9999);
	mergeSort(readIntsFile("Reversed.txt"),0,9999);
	mergeSort(readIntsFile("NearlySorted.txt"),0,9999);
	cout<<"Number of comparisons for Merge function = "<<mergeCompare<<endl;
	cout<<"Number of swaps for Merge function = "<<mergeSwap<<endl;//will always be 0 because no swapping occurs


	return 0;

}

