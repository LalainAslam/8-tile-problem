# 8-tile-problem

#include<iostream>
#include<queue>
using namespace std;
int win[3][3]={{0,1,2},{3,4,5},{6,7,8}};     //winning state
int winning=0;                               //winning is initially 0 (false)

class game
{
	struct node
	{
		int arr[3][3];
		node *child1;
		node *child2;
		node *child3;
		node *child4;		
	}; 
		node *root;
		node *parent;
	
	public:
		game()                            //both root and parent are NULL initially
		{
			root=NULL;
			parent=NULL;
		}
		
		
		
	bool bfs(node *root, node *checknode)        //function to traverse the tree by BFS
	{		
		queue<node*> Q;
		
		Q.push(root);
		while(!Q.empty())
		{
			node *current=Q.front();
			if(current->arr==checknode->arr)      //condition to check if new node is already a visited state
			{                                     //if state is visited state then return to createnode function with false to imply that this state should not be added to tree
				return false;
				break;
			}
			else
			{			
				if(current->child1!=NULL)
					Q.push(current->child1);
				if(current->child2!=NULL)
					Q.push(current->child2); 
				if(current->child3!=NULL)
					Q.push(current->child3);
				if(current->child4!=NULL)
					Q.push(current->child4);		
				Q.pop();
			}
						return true;     //if new node is not in the tree already, returns true to imply that new node should be added to tree

			
		}
	}
		
		
		void get()      //function to get initial state from the user
		{
		
			node *temp= new node;
			temp->child1=NULL;
			temp->child2=NULL;
			temp->child3=NULL;
			temp->child4=NULL;

			cout<<"Enter the matrix you want to start with: (Enter 0 for empty space)"<<endl;
			for(int i=0; i<3; i++)
			{
				for(int j=0; j<3; j++)
				{
					cin>>temp->arr[i][j];
				}
			}
		
			cout<<endl<<endl<<"You entered: "<<endl;
			for(int i=0; i<3; i++)
			{
				for(int j=0; j<3; j++)
				{
					cout<<temp->arr[i][j]<<"  ";
				}
				cout<<endl<<endl;
			}
			
			if(root==NULL)
			{
				root=temp;           //initial state is saved as the root of the tree
			}
		}
		

		
	
		void createNode(node *parent, int x, int y, int r, int c)      
		{
			
			node *newchild= new node;
			
			for(int i=0; i<3; i++)
			{
				for(int j=0; j<3; j++)
				{
					newchild->arr[i][j]=parent->arr[i][j];
				}
			}
			
			
			newchild->arr[y][x]=newchild->arr[r][c];   //made changes in newchild (swapped tiles)
			newchild->arr[r][c]=0;
			
			if(newchild->arr==win)
			winning=1;
			
			if(bfs(root, newchild))
			{
			
			for(int i=0; i<3; i++)
			{
				for(int j=0; j<3; j++)
				{
					cout<<newchild->arr[i][j]<<" ";
				}
				cout<<endl;
			}
			cout<<endl<<endl;
			
			
			if(parent->child1==NULL)
			{
				parent->child1=newchild;
			}
			
			else if(parent->child2==NULL)
			{
				parent->child2=newchild;

			}
			
			else if(parent->child3==NULL)
			{
				parent->child3=newchild;
			}
			
			else if(parent->child4==NULL)
			{
				parent->child4=newchild;
			}
		}

		
		}
					
		
		void options()             //function to check which tiles can be swapped in node 
		{
			int x,y,r,c;
			node *temp=root;
			parent=temp;
			for(int i=0; i<3; i++)
			{
				for(int j=0; j<3; j++)
				{
					if(temp->arr[i][j]==0)
					{
						x=j;
						y=i;						
						//cout<<"0 is at Row "<<i+1<<" column "<<j+1<<endl;
						break;
					}
				}
			}
			
			do
			{
			
				if(x+1<3)     //checks if 0 can move to right
				{
					r=y;
					c=x+1;
					createNode(parent, x, y, r, c);	          //calls function to create a new child node with swapped tiles 
				}
				
				if(x-1>=0)     //checks if 0 can move to left
				{			
					r=y;
					c=x-1;
					createNode(parent, x, y, r, c);		   //calls function to create a new child node with swapped tiles 
				}
				
				if(y+1<3)     //checks if 0 can move upward
				{
					r=y+1;
					c=x;
					createNode(parent, x, y, r, c);		   //calls function to create a new child node with swapped tiles 
				}
				
				if(y-1>=0)     //checks if 0 can move downward
				{
					r=y-1;
					c=x;
					createNode(parent, x, y, r, c);		      //calls function to create a new child node with swapped tiles 
				}			
			}while(winning!=1);
	
		}
		
						
};


int main()
{
	game g;
	g.get();
	g.options();
}
