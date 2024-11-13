#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define max 100

struct University {
    int univ_Id;
    char univ_code[10];
    char univ_name[20];
    char univ_address[30];
    char univ_email[15];
    char univ_website[10];
} typedef uni;

uni university[max];
int university_count = 0;
const char *filename = "university_data.txt"; // Text file to store data


void save_to_file() {
    FILE *file = fopen(filename, "w"); // Open in write mode (text)
    if (file == NULL) {
        printf("Error opening file for writing.\n");
        return;
    }

    fprintf(file, "%d\n", university_count); // Write count of universities
    for (int i = 0; i < university_count; i++) {
        fprintf(file, "%d %s %s %s %s %s\n", university[i].univ_Id,
                university[i].univ_code, university[i].univ_name, 
                university[i].univ_address, university[i].univ_email, 
                university[i].univ_website); // Write each university's data
    }

    fclose(file);
    printf("Data saved to file successfully.\n");
}


void  V_Dart_university_load_from_file() {
    FILE *file = fopen(filename, "r"); // Open in read mode (text)
    if (file == NULL) {
        printf("No previous data found. Starting fresh.\n");
        return;
    }

    fscanf(file, "%d", &university_count); // Read the count of universities
    for (int i = 0; i < university_count; i++) {
        fscanf(file, "%d %s %s %s %s %s", &university[i].univ_Id,
               university[i].univ_code, university[i].univ_name, 
               university[i].univ_address, university[i].univ_email, 
               university[i].univ_website); // Read each university's data
    }

    fclose(file);
    printf("Data loaded from file successfully.\n");
}

void getuniversitydetails(uni *u) {
    printf("Enter University ID: ");
    scanf("%d", &u->univ_Id);
    printf("Enter University Code: ");
    scanf("%s", u->univ_code);
    printf("Enter University Name: ");
    scanf("%s", u->univ_name);
    printf("Enter University Address: ");
    scanf("%s", u->univ_address);
    printf("Enter University Email: ");
    scanf("%s", u->univ_email);
    printf("Enter University Website: ");
    scanf("%s", u->univ_website);
}

void showuniversitydetails(uni *u) {
    printf("%d %s %s %s %s %s\n", u->univ_Id, u->univ_code, u->univ_name, u->univ_address, u->univ_email, u->univ_website);
}

// Bubble Sort for Sorting the universities by univ_Id
void  V_Dart_university_bubble_sort() {
    int i, j;
    uni temp;
    for (i = 0; i < university_count - 1; i++) {
        for (j = 0; j < university_count - 1 - i; j++) {
            if (university[j].univ_Id > university[j + 1].univ_Id) {
                temp = university[j];
                university[j] = university[j + 1];
                university[j + 1] = temp;
            }
        }
    }
}
void  V_Dart_university_quick_sort(int low, int high) {
    if (low < high) {
        int pivot = university[high].univ_Id;
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (university[j].univ_Id < pivot) {
                i++;
                uni temp = university[i];
                university[i] = university[j];
                university[j] = temp;
            }
        }
        uni temp = university[i + 1];
        university[i + 1] = university[high];
        university[high] = temp;
        int pi = i + 1;
         V_Dart_university_quick_sort(low, pi - 1);
         V_Dart_university_quick_sort(pi + 1, high);
    }
}

int  V_Dart_university_linear_search(int id) {
    for (int i = 0; i < university_count; i++) {
        if (university[i].univ_Id == id) {
            return i;  // University found at index i
        }
    }
    return -1;  // University not found
}


int  V_Dart_university_binary_search(int id) {
    int low = 0, high = university_count - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (university[mid].univ_Id == id) {
            return mid;
        }
        if (university[mid].univ_Id < id) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1; // University not found
}

void V_Dart_university_create() {
    if (university_count >= max) {
        printf("University List is Full\n");
        return;
    }
    uni u;
    getuniversitydetails(&u);
    university[university_count++] = u;
    save_to_file();  // Save data after adding
    printf("University Created Successfully!\n");
}

void V_Dart_university_delete() {
    if (university_count == 0) {
        printf("University List not found\n");
        return;
    }
    int id;
    printf("Enter University ID to Delete: ");
    scanf("%d", &id);
    for (int i = 0; i < university_count; i++) {
        if (university[i].univ_Id == id) {
            for (int j = i; j < university_count - 1; j++) {
                university[j] = university[j + 1];
            }
            university_count--;
            save_to_file();  // Save data after deletion
            printf("University deleted successfully!\n");
            return;
        }
    }
    printf("University with ID %d not found.\n", id);
}

void V_Dart_university_update() {
    if (university_count == 0) {
        printf("University List not found\n");
        return;
    }
    int id;
    printf("Enter University ID to Update: ");
    scanf("%d", &id);
    for (int i = 0; i < university_count; i++) {
        if (university[i].univ_Id == id) {
            printf("Enter details of ID %d again\n", id);
            getuniversitydetails(&university[i]);
            save_to_file();  // Save data after updating
            printf("University updated successfully!\n");
            return;
        }
    }
    printf("University with ID %d not found.\n", id);
}

void V_Dart_university_retrieve() {
    if (university_count == 0) {
        printf("University List is Empty\n");
        return;
    }
    for (int i = 0; i < university_count; i++) {
        showuniversitydetails(&university[i]);
    }
}

void  V_Dart_university_display_time_complexities() {
    printf("\nTime Complexities:\n");
    printf("1. Bubble Sort: O(n^2)\n");
    printf("2. Quick Sort: O(n log n) (on average)\n");
    printf("3. Linear Search: O(n)\n");
    printf("4. Binary Search: O(log n) (requires sorted data)\n");
}

int main() {
     V_Dart_university_load_from_file();  

    int choice;
    do {
        printf("\n--- University Management System ---\n");
        printf("1. Create University\n");
        printf("2. Delete University\n");
        printf("3. Update University\n");
        printf("4. Retrieve University List\n");
        printf("5. Sort Universities (Bubble Sort)\n");
        printf("6. Sort Universities (Quick Sort)\n");
        printf("7. Search University (Linear Search)\n");
        printf("8. Search University (Binary Search)\n");
        printf("9. Display Time Complexities\n");
        printf("10. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        
        switch (choice) {
            case 1:
                V_Dart_university_create();
                break;
            case 2:
                V_Dart_university_delete();
                break;
            case 3:
                V_Dart_university_update();
                break;
            case 4:
                V_Dart_university_retrieve();
                break;
            case 5: {
                clock_t start = clock();
                 V_Dart_university_bubble_sort();
                clock_t end = clock();
                double time_taken = ((double)end - start) / CLOCKS_PER_SEC;
                printf("Bubble Sort Time: %f seconds\n", time_taken);
                break;
            }
            case 6: {
                clock_t start = clock();
                 V_Dart_university_quick_sort(0, university_count - 1);
                clock_t end = clock();
                double time_taken = ((double)end - start) / CLOCKS_PER_SEC;
                printf("Quick Sort Time: %f seconds\n", time_taken);
                break;
            }
            case 7: {
                int id;
                printf("Enter University ID to search: ");
                scanf("%d", &id);
                clock_t start = clock();
                int result =  V_Dart_university_linear_search(id);
                clock_t end = clock();
                double time_taken = ((double)end - start) / CLOCKS_PER_SEC;
                if (result != -1)
                    printf("University found at index: %d\n", result);
                else
                    printf("University not found\n");
                printf("Linear Search Time: %f seconds\n", time_taken);
                break;
            }
            case 8: {
                int id;
                printf("Enter University ID to search: ");
                scanf("%d", &id);
                clock_t start = clock();
                int result =  V_Dart_university_binary_search(id);
                clock_t end = clock();
                double time_taken = ((double)end - start) / CLOCKS_PER_SEC;
                if (result != -1)
                    printf("University found at index: %d\n", result);
                else
                    printf("University not found\n");
                printf("Binary Search Time: %f seconds\n", time_taken);
                break;
            }
            case 9:
                 V_Dart_university_display_time_complexities();
                break;
            case 10:
                printf("Exiting...\n");
                break;
            default:
                printf("Invalid choice! Please enter a valid option.\n");
        }
    } while (choice != 10);

    return 0;
}
