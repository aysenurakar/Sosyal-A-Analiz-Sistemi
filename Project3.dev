#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_USERS 100

typedef struct User {
    int id;
    int friendCount;
    struct User** friends;
    int visited;
} User;

User* userList[MAX_USERS];
int userCount = 0;

User* findUser(int id) {
	int i;
    for (i = 0; i < userCount; i++)
        if (userList[i]->id == id)
            return userList[i];
    return NULL;
}

User* createUser(int id) {
    User* user = (User*)malloc(sizeof(User));
    user->id = id;
    user->friendCount = 0;
    user->friends = (User**)malloc(sizeof(User*) * MAX_USERS);
    user->visited = 0;
    return user;
}

void addFriendship(int id1, int id2) {
    User* u1 = findUser(id1);
    User* u2 = findUser(id2);
    if (!u1 || !u2) return;
    u1->friends[u1->friendCount++] = u2;
    u2->friends[u2->friendCount++] = u1;
}

void loadFromFile(const char* filename) {
    FILE* file = fopen(filename, "r");
    if (!file) {
        printf("Dosya açılamadı.\n");
        return;
    }

    char line[100];
    while (fgets(line, sizeof(line), file)) {
        if (strncmp(line, "USER", 4) == 0) {
            int id;
            sscanf(line, "USER %d", &id);
            userList[userCount++] = createUser(id);
        }
        else if (strncmp(line, "FRIEND", 6) == 0) {
            int id1, id2;
            sscanf(line, "FRIEND %d %d", &id1, &id2);
            addFriendship(id1, id2);
        }
    }
    fclose(file);
}

void resetVisited() {
	int i;
    for ( i = 0; i < userCount; i++)
        userList[i]->visited = 0;
}

void dfs(User* user, int depth) {
    if (depth < 0 || user->visited) return;
    user->visited = 1;
    printf("Kullanici ID: %d\n", user->id);
    int i;
    for ( i = 0; i < user->friendCount; i++)
        dfs(user->friends[i], depth - 1);
}

void ortakArkadaslar(int id1, int id2) {
    User* u1 = findUser(id1);
    User* u2 = findUser(id2);
    if (!u1 || !u2) return;

    printf("Ortak arkadaslar: ");
    int i,j;
    for (i = 0; i < u1->friendCount; i++)
        for ( j = 0; j < u2->friendCount; j++)
            if (u1->friends[i]->id == u2->friends[j]->id)
                printf("%d ", u1->friends[i]->id);
    printf("\n");
}

void dfsMark(User* user) {
    if (user->visited) return;
    user->visited = 1;
    printf("Kullanici ID: %d\n", user->id);
    int i;
    for ( i = 0; i < user->friendCount; i++)
        dfsMark(user->friends[i]);
}

void toplulukTespiti() {
    resetVisited();
    int count = 0;
    int i;
    for (i = 0; i < userCount; i++) {
        if (!userList[i]->visited) {
            printf("\nTopluluk %d:\n", ++count);
            dfsMark(userList[i]);
        }
    }
}

int etkialani(User* user) {
    int count = 0;
    resetVisited();
    void dfsCount(User* u) {
        if (u->visited) return;
        u->visited = 1;
        count++;
        int i;
        for ( i = 0; i < u->friendCount; i++)
            dfsCount(u->friends[i]);
    }
    dfsCount(user);
    return count - 1; // kendisi hariç
}

// ===== RBTREE =====
typedef enum { RED, BLACK } Color;

typedef struct RBNode {
    int id;
    Color color;
    struct RBNode *left, *right, *parent;
} RBNode;

RBNode* root = NULL;

RBNode* newNode(int id) {
    RBNode* node = (RBNode*)malloc(sizeof(RBNode));
    node->id = id;
    node->color = RED;
    node->left = node->right = node->parent = NULL;
    return node;
}

void leftRotate(RBNode** root, RBNode* x) {
    RBNode* y = x->right;
    x->right = y->left;
    if (y->left) y->left->parent = x;
    y->parent = x->parent;
    if (!x->parent) *root = y;
    else if (x == x->parent->left) x->parent->left = y;
    else x->parent->right = y;
    y->left = x;
    x->parent = y;
}

void rightRotate(RBNode** root, RBNode* y) {
    RBNode* x = y->left;
    y->left = x->right;
    if (x->right) x->right->parent = y;
    x->parent = y->parent;
    if (!y->parent) *root = x;
    else if (y == y->parent->left) y->parent->left = x;
    else y->parent->right = x;
    x->right = y;
    y->parent = x;
}

void fixInsert(RBNode** root, RBNode* z) {
    while (z->parent && z->parent->color == RED) {
        if (z->parent == z->parent->parent->left) {
            RBNode* y = z->parent->parent->right;
            if (y && y->color == RED) {
                z->parent->color = BLACK;
                y->color = BLACK;
                z->parent->parent->color = RED;
                z = z->parent->parent;
            } else {
                if (z == z->parent->right) {
                    z = z->parent;
                    leftRotate(root, z);
                }
                z->parent->color = BLACK;
                z->parent->parent->color = RED;
                rightRotate(root, z->parent->parent);
            }
        } else {
            RBNode* y = z->parent->parent->left;
            if (y && y->color == RED) {
                z->parent->color = BLACK;
                y->color = BLACK;
                z->parent->parent->color = RED;
                z = z->parent->parent;
            } else {
                if (z == z->parent->left) {
                    z = z->parent;
                    rightRotate(root, z);
                }
                z->parent->color = BLACK;
                z->parent->parent->color = RED;
                leftRotate(root, z->parent->parent);
            }
        }
    }
    (*root)->color = BLACK;
}

void insertRB(RBNode** root, int id) {
    RBNode* z = newNode(id);
    RBNode* y = NULL;
    RBNode* x = *root;
    while (x != NULL) {
        y = x;
        if (z->id < x->id) x = x->left;
        else x = x->right;
    }
    z->parent = y;
    if (y == NULL) *root = z;
    else if (z->id < y->id) y->left = z;
    else y->right = z;
    fixInsert(root, z);
}

RBNode* searchRB(RBNode* root, int id) {
    if (!root || root->id == id) return root;
    if (id < root->id) return searchRB(root->left, id);
    return searchRB(root->right, id);
}

// === Ana Fonksiyon ===
int main() {
    loadFromFile("C:\\Users\\aysen\\OneDrive\\Desktop\\veriseti.txt");

    // RBTREE oluştur
    int i;
    for (i = 0; i < userCount; i++)
        insertRB(&root, userList[i]->id);

    int secim;
    printf("Sosyal Ag Analiz Menusu:\n");
    printf("1 - DFS ile mesafedeki arkadaslari bul\n");
    printf("2 - Ortak arkadaslari bul\n");
    printf("3 - Topluluklari tespit et\n");
    printf("4 - Etki alanini hesapla\n");
    printf("5 - Kullanici ID'sini RBTree ile ara\n");
    printf("Seciminiz (1-5): ");
    scanf("%d", &secim);

    if (secim == 1) {
        int id, mesafe;
        printf("Kullanici ID: "); scanf("%d", &id);
        printf("Mesafe: "); scanf("%d", &mesafe);
        resetVisited();
        dfs(findUser(id), mesafe);
    } else if (secim == 2) {
        int a, b;
        printf("Ilk kullanici ID: "); scanf("%d", &a);
        printf("Ikinci kullanici ID: "); scanf("%d", &b);
        ortakArkadaslar(a, b);
    } else if (secim == 3) {
        toplulukTespiti();
    } else if (secim == 4) {
        int id;
        printf("Kullanici ID: "); scanf("%d", &id);
        int etkisi = etkialani(findUser(id));
        printf("Kullanici %d'nin etki alani: %d\n", id, etkisi);
    } else if (secim == 5) {
        int id;
        printf("Aranacak ID: "); scanf("%d", &id);
        RBNode* res = searchRB(root, id);
        if (res) printf("RBTree: Kullanici %d bulundu.\n", id);
        else printf("RBTree: Kullanici %d bulunamadi.\n", id);
    } else {
        printf("Gecersiz secim.\n");
    }

    return 0;
}
