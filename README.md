# Student-Result-Management-System-C-Code-
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Student {
    int roll;
    char name[50];
    int m1, m2, m3;
    float percent;
    char grade;
};

void calculate(struct Student *s) {
    int total = s->m1 + s->m2 + s->m3;
    s->percent = total / 3.0;

    if (s->percent >= 90)
        s->grade = 'A';
    else if (s->percent >= 75)
        s->grade = 'B';
    else if (s->percent >= 60)
        s->grade = 'C';
    else
        s->grade = 'F';
}

void addStudent() {
    struct Student s;
    FILE *fp = fopen("students.dat", "ab");

    printf("\nEnter Roll Number: ");
    scanf("%d", &s.roll);

    printf("Enter Name: ");
    scanf(" %[^\n]", s.name);

    printf("Enter Marks in 3 Subjects: ");
    scanf("%d %d %d", &s.m1, &s.m2, &s.m3);

    calculate(&s);
    fwrite(&s, sizeof(s), 1, fp);

    fclose(fp);
    printf("\nStudent added successfully!\n");
}

void searchStudent() {
    struct Student s;
    int roll, found = 0;
    FILE *fp = fopen("students.dat", "rb");

    printf("\nEnter Roll Number to Search: ");
    scanf("%d", &roll);

    while (fread(&s, sizeof(s), 1, fp)) {
        if (s.roll == roll) {
            printf("\n--- Student Details ---\n");
            printf("Roll: %d\nName: %s\n", s.roll, s.name);
            printf("Marks: %d %d %d\n", s.m1, s.m2, s.m3);
            printf("Percentage: %.2f\nGrade: %c\n", s.percent, s.grade);
            found = 1;
            break;
        }
    }

    fclose(fp);
    if (!found)
        printf("\nStudent not found!\n");
}

int main() {
    int choice;

    do {
        printf("\n===== Student Result Management System =====");
        printf("\n1. Add Student");
        printf("\n2. Search Student");
        printf("\n3. Exit");
        printf("\nEnter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addStudent();
                break;
            case 2:
                searchStudent();
                break;
            case 3:
                printf("\nExiting...\n");
                break;
            default:
                printf("\nInvalid choice!\n");
        }
    } while (choice != 3);

    return 0;
}
