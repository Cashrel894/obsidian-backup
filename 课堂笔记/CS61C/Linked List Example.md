#CS61C 
```c
#include <string.h>
struct Node {
	char *data;
	Node *next;
};

int main() {
	struct Node *head = NULL;
	add_to_front(&head, "abc");
}

void add_to_front(struct Node **head, char *data) {
	struct Node *node = (struct Node *) malloc(sizeof(struct Node));
	node->data = (char *) malloc(strlen(data) + 1);
	strcpy(node->data, data);
	node->next = *head;
	*head = node;
}
```