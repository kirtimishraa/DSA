Memory visualization of C Memory Layout:

+----------------------+
|        Heap          | ← Used for dynamic memory (malloc, calloc, etc.)
|                      |
|       (grows up)     |
+----------------------+
|        Stack         | ← Local variables, function calls
|                      |    auto, local variables
|      (grows down)    |
+----------------------+
| Initialized Data     | ← Global/static variables with values
| Segment              |
|  (e.g., static int i = 5;) |
+----------------------+
| Uninitialized Data   | ← Global/static variables without values
| Segment (BSS)        |
|  (e.g., static int i;)     |
+----------------------+
|     Code Segment     | ← Actual compiled code/instructions
|   (Text Segment)     |
+----------------------+


- Static Memory:
    memory that Once provided/allocated throughout the program it can not grow/shrink

- Dynamic Memory allocation: Flexible to change or shrink

    function to create/free memory allocation:
    1. malloc()
    2. calloc()
    3. realloc()
    4. free() 

- sizeof(Data_Type) operator/keyword            //it gives the size of data type

- malloc():
    Syntax:    (void*)malloc(size_t size)

    return type (void*) by default: When memory allocation happen malloc retun the address of that particular memory which need to be stored in pointer 
    void pointer; As void is generic pointer type, it can point to any data type

    argument to malloc: size_t means unsigned int 
    We provides size in bytes; How many bytes memory we want, When we're provinding the memory to programmer then we share the address of that memory to programmer, that's where we use pointer; We may/maynot use typecasting with void pointer

    - we can use: malloc(N*sizeof(int));
    as 
    void main()
    {
        int *p;
        int N;
        pf("enter the number of elements");
        sf("%d", &N);
        p = (int*)malloc(N*sizeof(int));            //did typecasting with void pointer
        p = malloc(N*sizeof(int));      //didn't used typecasting
    }

    - if memory is not available malloc() will return NULL/Invalid address

    - Taking input/ Printing output

    P=malloc(N*sizeof(int))     //N*4(int_value)=4N bytes of contegious data whose return address has been hold by pointer P, if p is corrently pointing to starting address of memory i.e 100

    scanf("%d", p);         //1st input will be stored at 100
    scanf("%d", p+1);       //2nd i/p will be stored at 100+1*4=104 
    scanf("%d", p+2);       //3rd i/p will be stored at 108 so on

    so we can say 
    for(i=0; i<N; i++)
        scanf("%d", p+i);

    for(i=0; i<N; i++)
    pf("%d", *(p+i)); or ("%d", p[i])           // Used * to get the value

------------------------

- Calloc: Contiguous Allocation
    Syntax: (void*)calloc(No_elements, element_size)

    - Malloc is not reliable; Because it malloc allocates; it provide garbage bit/value, so it takes less time and its cheaper 

    - calloc is reliable; Because when it finds the memory first it initialize bits 0, then provide the address

-------------------------

- realloc: 
    Relocate the same memory that was used before 
    When we allocated memory of N bytes but now want memory to grow from 2N but now we don't have contigeous memory, in this case we would have to find another consecutive memory of 2N bytes; 
    in new block we will copy the used N bytes of memory

    int *P;     //Declared pointer
    P = malloc(sizeof(int)*5);  //allocated memory of 5*4=20 bytes

    for(i=0; i<5; i++)
        scanf("%d", P+i);       //Get the 5 input in consecutive blocks


    P = realloc(P, 10*sizeof(int));     //rellocate the memory to the same pointer but with different size
---------------------------

- Free():
    We need to free the memory to avoid memroy leakage problem/Exhousted problem
    Syntax: free(pointer_name)  //pointer name to that we had allocated the memory
    free(p);
    Memory leakage: Memory is there but we can't access
    Dangling pointer: Memory is not there but trying to access

- use realloc as malloc and free
    1. realloc as malloc
        we will make ptr=NULL;
        realloc(ptr, New_size)  //realloc
        ptr = realloc(ptr, N*sizeof(int));

    2. realloc to free:
       ptr = realloc(ptr, 0);

-------------------------------------------------------------------------------------------------


Linked List: 
A linear Data Structure, each element called node And each node contains Data and Pointer that contains the address of Next node and so on...(Imagine there is is a series of boxes(Nodes) which all are locked and each box has two different parts first part has some goods(Data) and second part has key of Second Box(Pointer which has address of next Node) and and then Second Box's second part has key of Third and so on. But we need key(Address) of first box(Node) i.e START/HEAD pointer)

1. if START = NULL simply LL is empty

struct node
{
    int data;   //Data of node
    struct node *Link;      
    //Link is pointer which will have the address of Next node 
};

struct node *START;
//START is a pointer to struct node i.e it has address of first node 

Representation:

Start --> [1000]
            |
            v
+--------+--------+       +--------+--------+       +--------+--------+       +--------+--------+
| Data: 8| Link:1000| --> | Data: 6| Link:1100| --> | Data: 4| Link:1200| --> | Data: 2| Link:NULL|
+--------+--------+       +--------+--------+       +--------+--------+       +--------+--------+
   ^                       ^                          ^                         ^
   |                       |                          |                         |
 Address:              Address:                    Address:                    Address:
  1000                  1050                         1100                       1200

- To access the members of strcuture/node

(*START).data                 //  To access the data
(*START).Link                //To access the address, we're getting address of first node i.e 1000

To get directly vaue/Data from pointer
START -> data
START -> Link           //Address of 2nd node means pointer is pointing to second node (as START had contaned address if First node)
START -> Link -> Link       //Pointer is pointing to the 3rd node 

struct node *ptr;             // Created a pointer ptr
ptr = START;           //ptr get the addrss of 1st node that START HAD pointing, Created new ptr to avoid chaos with direct pointer 
ptr = ptr -> Link;       //Assigned the Next address of node which is pointed by ptr i.e 1050; so now ptr is poiting to the 2nd node
ptr = ptr -> Link;           //ptr is poiting Next to Next address of Start i.e Address 1100
pf("%d", ptr->Data);         //as ptr is poiting to the address 1100 so data of that address will be printed
o/p: Data: 4 will be pinted

ptr -> Link ->Link = NULL   //Assigning NULL to the address 1100; that address has contained by 2nd Node so 2nd node would contain  NULL and link would be broaken from Next 2 nodes and those nodes will be lost

----------------------------

- Memory address Allocation:

*Dynamic data structure
struct node{
    int data;
    struct node *Link;
};

malloc(sizeof(struct node));    //As Syntax of malloc is: malloc(sizeof(Data Type)); As we want to create memory allocation of size structure(Has two element value and pointer)

    - As malloc returns Void pointer by default so it need to be stored somewhere 

struct node *temp;                       //created pointer temp
temp = malloc(sizeof(struct node));     //temp will hold the return address of allocated memory by Type struct node(2 element) so If memory has address 5000, temp will is containg 5000 Address

temp = (struct node*)malloc(sizeof(struct node));           //By default malloc return void pointer but its a good practice to typecast it

temp -> data = 10;      //Temp will access the data variable and value 10 will be assigned into that
temp -> Link = NULL;     //Temp will access Link pointer and NULL will be assigned into that

Operations on LL:
1. Insertion: 1. Insert a node(Element) in the beg; 2. insertion in the End. 3. insertion After a specific node.
    Printing Last node: Atleast one node should be there, Printing second last atleast 2 node should be there...
2. Finding the length of the LL.
3. Deletion: 1. Deletion of nods beg; 2. Deletion Last node; 3. Deletion from anywhere
4. Priting a LL; Printing a LL in reverse order
5. Searching an element in LL
6. Sorting in LL

1. Insertion at End
We have to create a New node with 2 element data and pointer and then have to insert this node into already created LL

    struct node{
        int Data;
        struct node *Link;
    };

    - Creation of New Node:

int key;   
struct node *ptr = START;    //created a pointer ptr and assigned START Address into it
struct node *temp   //Created a pointer;
temp = (*struct node)malloc(sizeof(struct node));              //Assign the return address into temp; temp is pointer to structure
pf("Enter an Element");
sf("%d", &key);         //Taking the input into key variable
temp -> data = key;     //Assigned the value of Key into data of temp 
temp->Link = NULL       //Assigning NULL into Link of temp

We have to insert the address of New node into the last node of LL which has contained NULL, but before start Finding NULL into LL, We have to check whether the START/HEAD should not be NULL as if that would be NULL then LL wound be empty

if(START == NULL)
    START = temp;   //Temp had the address of created node so we assigned that to Start of Start was already NULL
else                //But If Start was already pointing to some address
{
    ptr = START;        
    while(ptr->Link!=NULL)      //Loop will keep checking untill Link of ptr has NULL or not
    {
        ptr=ptr->LInk;          //ptr will keep containing next address of Nodes with iterations
    }
    ptr->Link = temp;
}

- Insertion At the Beg:
    - Created a New node
        .
        .
        temp = (*struct node)malloc(sizeof(struct node)); 
        .
        temp -> Data = key;
        temp -> Link = NULL;

    - Insertion of already Created node to the begining of the LL/first node
      temp -> Link = START;   //Link of temp/New node will have the address of START(Address of first node of exsisted LL); so 1st it'll be 2nd
      START = temp;   //Start will be poiting to the New Node which has become the First now

- Length of LL:

    int count =0;
    struct node ptr = START;
    while(ptr!=NULL)    // When ptr started pointing last node it contained address NULL But it will not ptr->Link!=NULL because if LL will be empty and we're trying to check/access the Next element it'll be segmentation fault
    {
        ptr = ptr -> Link;
        count++;
    }



