### 动态链表
```c
#include <stdio.h>
#incklude <stdlib.h>
#define LEN sizeof(struct Student)

struct Student
{
    int age;
    float score;
    char* name;
    struct Student* next;
};
int n;
struct Student* create(void)
{
    struct Student* head, *p1, *p2;
    n = 0;
    p1 = p2 = (struct Student*)malloc(LEN);
    scanf("%d %f %s", &p1.age, &p1.score, &p1.name);
    head = NULL;
    while (p1.age != 0)
    {
        n++;
        if (n == 1) head = p1;
        else p2->next = p1;
        p2 = p1;
        p1 = (struct Student*)malloc(LEN);
        scanf("%d %f %s", &p1.age, &p1.score, &p1.name);
    }
    p2->next = NULL;
    return head;
}

int main()
{
    struct Student *head = create();
    printf(":%d %.2f %s\n", head->age, head->score, head->name);
    return 0;
}
```