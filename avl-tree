#include "avl-tree.h"
#include <iostream>
#include <stack>

using namespace std;

bool AVLTree::insert(DataType val) {

    //use normal BST insert
    bool insertResult;
    insertResult = BinarySearchTree::insert(val);

    if (insertResult == false){ //if you cannot insert into tree, return false
        return false;
    }

    //determine if inserted node or any of its ancestors are unbalanced
    //if no -> job done, return true
    //if yes -> find pointer to unbalanced ancestor that is closest

    int balance; // keep track of difference in balance between left and right subtree
    bool unbalanceFound = false; //flag for if the tree is balanced or unbalanced

    Node* rootPointer = BinarySearchTree::getRootNode(); //rootPointer pointer to root node
    stack <BinarySearchTree::Node *> treeStack; //create a stack

    //push memory addresses of nodes from tree into stack (first in, last out)
    //top of stack is memory address of inserted node (root -> ancestors -> inserted node)

    while (rootPointer != nullptr){ //push inserted node and any of its ancestors into a stack
        if (rootPointer->val == val){ //if the root node is the node we inserted, push to stack and break loop
            treeStack.push(rootPointer);
            break;
        }
        else if (rootPointer->val > val){ //if val is smaller than root push to stack, move left
            treeStack.push(rootPointer);
            rootPointer = rootPointer->left;
        }
        else{ //if val is greater than root push to stack, move right
            treeStack.push(rootPointer);
            rootPointer = rootPointer->right;
        }
    }

    //determine if tree is balanced or not
    while (!treeStack.empty() && !unbalanceFound){
        //memory address of inserted node
        BinarySearchTree::updateNodeBalance(treeStack.top()); //updates balance of inserted node
        balance = treeStack.top()->avlBalance;//avl balance returns an int of the difference between the left and right subtree height for a given node (it is a data member of each node)
        if (balance < -1 || balance > 1){ //if balance is not equal to -1, 0 or 1 than tree is unbalanced
            unbalanceFound = true;
            break; //break while loop
        }
        treeStack.pop();//if tree is balanced at inserted node, remove top element (memory address of top node)
        //continue checking for unbalance at each of the ancestor nodes
    }

    if (!unbalanceFound){//if entire tree is balanced, return true
        return true;
    }
    else{
        Node *alpha = treeStack.top(); //memory address of the node where imbalance is found
        treeStack.pop();//remove top of from stack
        Node *prev = NULL;

        //flag for what type of rotation is needed
        bool leftrightRotate = false;

        if (treeStack.empty() == false) {//if stack is not empty, the unbalance node has an ancestor (otherwise its root)
            prev = treeStack.top();//save memory address the parent node of node that caused unbalance
        }

        //right right visually -> single left rotation
        if (balance < -1 && val > alpha->right->val) {//right is longer side and inserted node is to the right of A
            leftRotation(alpha, prev, leftrightRotate);
            return true;
        }
        //left left visually -> single right rotation
        else if (balance > 1 && val < alpha->left->val) {//if left side is longer and val is on the left subtree of A
            rightRotation(alpha, prev);
            return true;
        }
        //right left rotation
        else if (balance < -1 && val < alpha->right->val) {//if right subtree is longer and inserted node is in left subtree of A
            //rightleftRotate = true;
            rightRotation(alpha->right, alpha);//single right rotate
            leftRotation(alpha, prev, leftrightRotate);//single left rotate
            return true;
        }
        //left right rotation
        else if (balance > 1 && val > alpha->left->val) {//if left subtree is longer and inserted node is in right subtree of A
            leftrightRotate = true;
            leftRotation(alpha->left, alpha, leftrightRotate);//left rotate
            rightRotation(alpha, prev);//right rotate
            return true;
        }
    }
}

bool AVLTree::remove(DataType val) {


}

void AVLTree::leftRotation(Node *alpha, Node *prev, bool leftrightRotation){
    Node *A = alpha->right;
    Node *t2 = A->left;

    //single left rotation
    alpha->right = t2;
    A->left = alpha;

    //set previous
    if (prev != NULL) {
        if (leftrightRotation){
            prev->left = A;
        }
        else{
            prev->right = A;
        }
    }
    else { //if no previous, set A as the new newRoot
        Node **newRoot = getRootNodeAddress();
        *newRoot = A;
    }
}

void AVLTree::rightRotation(Node *alpha, Node *prev){
    Node *A = alpha->left;
    Node *t2 = A->right;

    //single right rotation
    alpha->left = t2;
    A->right = alpha;


    //set previous right pointer to the new parent of right subtree
    if (prev != NULL) {//not the root node
        prev->right = A;
    }
    else {//if there is no previous, A is the new root
        Node **root = getRootNodeAddress();
        *root = A;
    }
}
