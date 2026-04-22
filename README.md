# Avl-tree-
How to create an AVL tree
#include <stdio.h>
#include <stdlib.h>

/* AVL Tree Node */
struct Node {
    int key;
    int height;
    struct Node *left;
    struct Node *right;
};

/* Create New Node */
struct Node* createNode(int key)
{
    struct Node* node =
      (struct Node*)malloc(sizeof(struct Node));

    node->key = key;
    node->height = 1;
    node->left = NULL;
    node->right = NULL;

    return node;
}

/* Maximum of two numbers */
int max(int a,int b)
{
    if(a>b)
        return a;
    else
        return b;
}

/* Height of node */
int height(struct Node* n)
{
    if(n==NULL)
        return 0;
    return n->height;
}

/* Balance Factor */
int getBalance(struct Node* n)
{
    if(n==NULL)
        return 0;

    return height(n->left)-height(n->right);
}

/* Update Height */
void updateHeight(struct Node* n)
{
    if(n!=NULL)
    {
        n->height=
        1+max(height(n->left),
              height(n->right));
    }
}

/* Right Rotation (LL) */
struct Node* rotateRight(struct Node* y)
{
    struct Node* x=y->left;
    struct Node* T2=x->right;

    x->right=y;
    y->left=T2;

    updateHeight(y);
    updateHeight(x);

    return x;
}

/* Left Rotation (RR) */
struct Node* rotateLeft(struct Node* x)
{
    struct Node* y=x->right;
    struct Node* T2=y->left;

    y->left=x;
    x->right=T2;

    updateHeight(x);
    updateHeight(y);

    return y;
}

/* Insert in AVL Tree */
struct Node* insert(struct Node* node,int key)
{

    /* Normal BST Insert */
    if(node==NULL)
        return createNode(key);

    if(key < node->key)
        node->left=
        insert(node->left,key);

    else if(key > node->key)
        node->right=
        insert(node->right,key);

    else
        return node;

    /* Update height */
    updateHeight(node);

    int balance=getBalance(node);

    /* LL Case */
    if(balance>1 &&
       key<node->left->key)
        return rotateRight(node);

    /* RR Case */
    if(balance<-1 &&
       key>node->right->key)
        return rotateLeft(node);

    /* LR Case */
    if(balance>1 &&
       key>node->left->key)
    {
        node->left=
        rotateLeft(node->left);

        return rotateRight(node);
    }

    /* RL Case */
    if(balance<-1 &&
       key<node->right->key)
    {
        node->right=
        rotateRight(node->right);

        return rotateLeft(node);
    }

    return node;
}

/* Inorder Traversal */
void inOrder(struct Node* root)
{
    if(root!=NULL)
    {
        inOrder(root->left);
        printf("%d ",root->key);
        inOrder(root->right);
    }
}

/* Preorder Traversal */
void preOrder(struct Node* root)
{
    if(root!=NULL)
    {
        printf("%d ",root->key);
        preOrder(root->left);
        preOrder(root->right);
    }
}

/* Search */
int search(struct Node* root,int key)
{
    if(root==NULL)
        return 0;

    if(root->key==key)
        return 1;

    if(key<root->key)
        return search(root->left,key);

    return search(root->right,key);
}

/* MAIN */
int main()
{
    struct Node* root=NULL;

    int values[]=
    {30,20,40,10,25,50,5};

    int i;

    for(i=0;i<7;i++)
        root=insert(root,values[i]);

    printf("Values inserted: ");
    for(i=0;i<7;i++)
        printf("%d ",values[i]);

    printf("\n");

    printf("In-Order Traversal : ");
    inOrder(root);

    printf("\n");

    printf("Pre-Order Traversal : ");
    preOrder(root);

    printf("\n");

    printf("Root of AVL Tree : %d\n",
           root->key);

    printf("Height of Tree : %d\n",
           root->height);

    printf("Balance Factor : %d\n",
           getBalance(root));

    if(search(root,25))
        printf("Search 25 : Found\n");
    else
        printf("Search 25 : Not Found\n");

    if(search(root,99))
        printf("Search 99 : Found\n");
    else
        printf("Search 99 : Not Found\n");

    return 0;
}
