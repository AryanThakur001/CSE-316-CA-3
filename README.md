#include <stdio.h>

// Structure to represent a student
struct Student {
    int id;
    int food_time;
    int remaining_time;
    int turnaround_time;
    int waiting_time;
};

// Function to calculate average turnaround and waiting time
void calculateAverageTimes(struct Student students[], int n) {
    int total_turnaround_time = 0;
    int total_waiting_time = 0;

    for (int i = 0; i < n; i++) {
        students[i].turnaround_time = students[i].waiting_time + students[i].food_time;
        total_turnaround_time += students[i].turnaround_time;
        total_waiting_time += students[i].waiting_time;
    }

    double average_turnaround_time = (double)total_turnaround_time / n;
    double average_waiting_time = (double)total_waiting_time / n;

    printf("Average Turnaround Time: %.2f\n", average_turnaround_time);
    printf("Average Waiting Time: %.2f\n", average_waiting_time);
}

int main() {
    // Initialize students with their IDs, food times, and remaining times
    struct Student students[] = {
        {2132, 2, 2, 0, 0},
        {2102, 4, 4, 0, 0},
        {2453, 8, 8, 0, 0},
    };

    int n = sizeof(students) / sizeof(students[0]);

    int current_time = 0;

    while (1) {
        int longest_remaining_time = -1;
        int selected_student = -1;

        // Find the student with the longest remaining time
        for (int i = 0; i < n; i++) {
            if (students[i].remaining_time > 0) {
                if (longest_remaining_time == -1 || students[i].remaining_time > longest_remaining_time) {
                    longest_remaining_time = students[i].remaining_time;
                    selected_student = i;
                }
            }
        }

        // If all students are served, break the loop
        if (selected_student == -1)
            break;

        // Update waiting time for other students
        for (int i = 0; i < n; i++) {
            if (i != selected_student && students[i].remaining_time > 0) {
                students[i].waiting_time++;
            }
        }

        // Reduce remaining time for the selected student
        students[selected_student].remaining_time--;

        // Increment current time
        current_time++;
    }

    calculateAverageTimes(students, n);

    return 0;
}
