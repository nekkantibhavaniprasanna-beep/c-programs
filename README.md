#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node *left, *right;
};

struct node* create(int x) {
    struct node *temp = (struct node*)malloc(sizeof(struct node));
    temp->data = x;
    temp->left = temp->right = NULL;
    return temp;
}

struct node* insert(struct node *root, int x) {
    if (root == NULL) return create(x);
    if (x < root->data) root->left = insert(root->left, x);
    else if (x > root->data) root->right = insert(root->right, x);
    return root;
}

struct node* search(struct node *root, int key) {
    if (root == NULL || root->data == key) return root;
    if (key < root->data) return search(root->left, key);
    return search(root->right, key);
}

struct node* minValueNode(struct node *node) {
    struct node *current = node;
    while (current && current->left != NULL) current = current->left;
    return current;
}

struct node* deleteNode(struct node *root, int key) {
    if (root == NULL) return root;
    if (key < root->data)
        root->left = deleteNode(root->left, key);
    else if (key > root->data)
        root->right = deleteNode(root->right, key);
    else {
        if (root->left == NULL) {
            struct node *temp = root->right;
            free(root);
            return temp;
        } else if (root->right == NULL) {
            struct node *temp = root->left;
            free(root);
            return temp;
        }
        struct node *temp = minValueNode(root->right);
        root->data = temp->data;
        root->right = deleteNode(root->right, temp->data);
    }
    return root;
}

void inorder(struct node *root) {
    if (root == NULL) return;
    inorder(root->left);
    printf("%d ", root->data);
    inorder(root->right);
}

int main() {
    struct node *root = NULL;
    int ch, x;
    while (1) {
        printf("\n1.Insert 2.Search 3.Delete 4.Inorder 5.Exit: ");
        scanf("%d", &ch);
        if (ch == 5) break;
        switch (ch) {
        case 1:
            printf("Enter value: ");
            scanf("%d", &x);
            root = insert(root, x);
            break;
        case 2:
            printf("Enter key: ");
            scanf("%d", &x);
            if (search(root, x))
                printf("Found\n");
            else
                printf("Not found\n");
            break;
        case 3:
            printf("Enter value to delete: ");
            scanf("%d", &x);
            root = deleteNode(root, x);
            break;
        case 4:
            inorder(root);
            printf("\n");
            break;
        default:
            printf("Invalid choice\n");
        }
    }
    return 0;
}
