# creating-binary-search-tree
#include <stdio.h>
#include <stdlib.h>

// Structure for a binary search tree node
struct Node {
  int data;
  struct Node* left;
  struct Node* right;
};

// Function to create a new node
struct Node* newNode(int data) {
  struct Node* temp = (struct Node*)malloc(sizeof(struct Node));
  temp->data = data;
  temp->left = temp->right = NULL;
  return temp;
}

// Function to construct BST from in-order and post-order traversals
// This is a helper function for recursive construction
struct Node* bst_construct_util(int in[], int post[], int inStart, int inEnd, int* postIndex) {
  if (inStart > inEnd) {
    return NULL;
  }

  // Find the root node in in-order traversal
  int rootValue = post[*postIndex];
  (*postIndex)--;
  int i;
  for (i = inStart; i <= inEnd; ++i) {
    if (in[i] == rootValue) {
      break;
    }
  }

  // Construct the left subtree
  struct Node* leftChild = bst_construct_util(in, post, inStart, i - 1, postIndex);

  // Construct the right subtree
  struct Node* rightChild = bst_construct_util(in, post, i + 1, inEnd, postIndex);

  // Create the node for the root and link children
  struct Node* root = newNode(rootValue);
  root->left = leftChild;
  root->right = rightChild;

  return root;
}

struct Node* bst_construct(int in[], int post[], int n) {
  int postIndex = n - 1;
  return bst_construct_util(in, post, 0, n - 1, &postIndex);
}

// Function to perform Breadth-First Search (BFS) traversal
void bfs(struct Node* root) {
  if (root == NULL) {
    return;
  }

  // Create a queue for level order traversal
  struct Node* queue[100];
  int front = 0, rear = -1;

  // Enqueue root node
  queue[++rear] = root;

  while (front <= rear) {
    // Dequeue a node and print its data
    struct Node* node = queue[front++];
    printf("%d ", node->data);

    // Enqueue left and right children (if they exist)
    if (node->left != NULL) {
      queue[++rear] = node->left;
    }
    if (node->right != NULL) {
      queue[++rear] = node->right;
    }
  }
}

int main() {
  int in[] = {5, 10, 15, 20, 25, 30, 45};
  int post[] = {5, 15, 10, 25, 45, 30, 20};
  int n = sizeof(in) / sizeof(in[0]);

  struct Node* root = bst_construct(in, post, n);

  printf("Breadth-First Search Traversal: ");
  bfs(root);
  printf("\n");

  return 0;
}
