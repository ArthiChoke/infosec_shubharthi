# Bandit Writeup

## Level 0  
Installed tldr, command:  
```bash
sudo apt install tldr
```

Learnt the format of the ssh command required to login  
Login command:  
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

---

## Level 0 to 1  
This level involved opening a file named readme from home directory  
I used the command `cat readme` while present in the home directory  
Password obtained: `ZjLTtM6FvyykRnDzrFNX07za6jp5lF`  
I typed `exit` command to log out of bandit0  
I then logged on to the next level by  
```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```
I used the given password to log in

---

## Level 1 to 2  
First I used `cat -`  
It started to echo back whatever I typed next, for example I typed ls and pressed enter and ls appeared twice, same thing happened when I typed exit  
I learnt that `-` (when used as an argument to a command) is a symbol used to represent standard input, that is input from keyboard, so cat was reading my inputs and echoing them back to me  
So I used Ctrl+C to interrupt the current running command  
I learnt that to open dashed files I have to use the complete location of the file that is,  
```bash
cat ./-
```
or  
```bash
cat < -
```

Password obtained: `Z6G3JGPfGtUtdEtDvGMiUXD5Ycac29MfX`

---

## Level 2 to 3  
I had to open a file whose name was “spaces in this filename”  
Command used:  
```bash
cat "spaces in this filename"
```
Password obtained: `MW0K8N8JUsl1o41PRuEDfQfqXLPSImx`

---

## Level 3 to 4  
I had to open a hidden file inside `./inhere`  
I used `ls -a` to know the name of required hidden file  
Name was `.hidden`  
```bash
cat .hidden
```
Password obtained: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

---

## Level 4 to 5  
I had to open a human readable file of size 1033 bytes that was not executable  
Commands used:  
```bash
find . -type f -size 1033c ! -executable
cat ./maybehere07/.file2
```
Password obtained: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

---

## Level 5 to 6  
I had to find a file owned by user bandit7 and group bandit6 with 33 bytes  
Command:  
```bash
find / -user bandit7 -group bandit6 -size 33c
```
It showed permission denied in many places but still gave the file location:  
`/home/bandit5/inhere/maybehere07/.file2`
Password obtained: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

---

## Level 6 to 7  
I had to find a file with a specific name using the `find` command  
I learned about `-name`, `-size`, `-user`, `-group`, `-executable`, etc.
Used cd / to go to root
Command:
```bash
find -type f -size 33c -user bandit7 -group bandit6
```
Password obtained: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

---

## Level 7 to 8  
Used `grep` command to filter lines from a file that had a certain string  
Learnt how to use `grep` efficiently  
Command:  
```bash
grep "millionth" data.txt
```
Password obtained: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

---

## Level 8 to 9  
Had to find a line in a file where all characters are the same  
Learnt regex usage with `grep`  
Used:  
```bash
grep -E '^.{1}$' data.txt
```

---

## Level 9 to 10  
Had to find a line with one different character among all identical ones  
Used `grep` and pattern matching  
Command:  
```bash
grep -E '^(.)\1*$' -v data.txt
```

---

## Level 10 to 11  
Had to decode base64 encoded password  
Used:  
```bash
base64 -d data.txt
```

---

## Level 11 to 12  
File was encrypted with `xxd`, had to reverse and decode  
Commands used:  
```bash
xxd -r data.txt > newfile
strings newfile
```

---

## Level 12 to 13  
File was a gzip inside a bzip2 inside a tar inside another archive  
Used `file`, `mv`, `bunzip2`, `tar`, `zcat`, and so on  
Step-by-step extracted until password was found

---

***








# Russian Roulette in C

#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node *next, *prev;
};

// Function prototypes
void insert_end(struct node **start, struct node **end, int value);
void delete_next(struct node **current, struct node **start, struct node **end);
void display(struct node *start);

int main() {
    struct node *start = NULL, *end = NULL, *current = NULL;
    int i;
    int n;

    printf("\n\n\t-- Russian Roulette -- \n\n");

    printf("How many players are joining us today?!");

    // Create circular doubly linked list with 10 players
    scanf("%d",&n);
    for (i = 1; i <= n; i++) {
        insert_end(&start, &end, i);
    }

    display(start);
    printf("\n\tPlayers are ready. Press Enter to start killing...\n");
    getchar();

    current = start;
    while (start != end) {
        delete_next(&current, &start, &end);
        display(start);
        printf("\nPress Enter to continue...\n");
        getchar();
        current = current->next;
    }

    printf("\n\t💀 Winner: Player %d 💀\n", start->data);

    // Free the last remaining node
    free(start);
    return 0;
}

// Insert a node at the end of the circular list
void insert_end(struct node **start, struct node **end, int value) {
    struct node *new_node = (struct node*) malloc(sizeof(struct node));
    new_node->data = value;

    if (*start == NULL) {
        new_node->next = new_node->prev = new_node;
        *start = *end = new_node;
    } else {
        new_node->prev = *end;
        new_node->next = *start;
        (*end)->next = new_node;
        (*start)->prev = new_node;
        *end = new_node;
    }
}

// Delete the next node after the current one
void delete_next(struct node **current, struct node **start, struct node **end) {
    struct node *victim = (*current)->next;

    if (victim == *start)
        *start = victim->next;
    if (victim == *end)
        *end = victim->prev;

    victim->prev->next = victim->next;
    victim->next->prev = victim->prev;

    printf("\n\tPlayer %d has been eliminated.\n", victim->data);
    free(victim);
}

// Display the current list of players
void display(struct node *start) {
    if (start == NULL) return;

    struct node *temp = start;
    printf("\n\tPlayers alive: ");
    do {
        printf("%d ", temp->data);
        temp = temp->next;
    } while (temp != start);
    printf("\n");
}









# Russian Roulette in Python

# Create a circular list of players
def insert_players(n):
    players = []
    for i in range(1, n + 1):
        players.append(i)
    return players

# Delete the next player in the circle
def delete_next(players, current_index):
    victim_index = (current_index + 1) % len(players)
    victim = players.pop(victim_index)
    print(f"\n\tPlayer {victim} has been eliminated.")
    return victim_index % len(players)  # New current index

# Display the current list of players
def display(players):
    print("\n\tPlayers alive:", ' '.join(map(str, players)))

# Main game logic
def main():
    print("\n\n\t-- Russian Roulette -- \n")
    n = int(input("How many players are joining us today? "))
    players = insert_players(n)

    display(players)
    input("\n\tPlayers are ready. Press Enter to start killing...\n")

    current_index = 0
    while len(players) > 1:
        current_index = delete_next(players, current_index)
        display(players)
        input("\nPress Enter to continue...\n")

    print(f"\n\t Winner: Player {players[0]} \n")

if __name__ == "__main__":
    main()









# Binary Sort in C

#include <stdio.h>

int binarySearch(int a[], int item, int low, int high)
{
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (item == a[mid])
            return mid + 1;
        else if (item > a[mid])
            low = mid + 1;
        else
            high = mid - 1;
    }

    return low;
}

void insertionSort(int a[], int n)
{
    int i, loc, j, k, selected;

    for (i = 1; i < n; ++i) {
        j = i - 1;
        selected = a[i];

        // find location where selected should be inserted
        loc = binarySearch(a, selected, 0, j);

        // Move all elements after location to create space
        while (j >= loc) {
            a[j + 1] = a[j];
            j--;
        }
        a[j + 1] = selected;
    }
}

int main()
{
    int a[]
        = { 37, 23, 0, 17, 12, 72, 31, 46, 100, 88, 54 };
    int n = sizeof(a) / sizeof(a[0]), i;

    insertionSort(a, n);

    printf("Sorted array: \n");
    for (i = 0; i < n; i++)
        printf("%d ", a[i]);

    return 0;
}






# To Do List that saves in a file C code

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_TASKS 100
#define MAX_LENGTH 256
#define FILENAME "todo.txt"

// Functionss
void loadTasks(char tasks[][MAX_LENGTH], int *taskCount);
void saveTasks(char tasks[][MAX_LENGTH], int taskCount);
void addTask(char tasks[][MAX_LENGTH], int *taskCount);
void viewTasks(char tasks[][MAX_LENGTH], int taskCount);
void deleteTask(char tasks[][MAX_LENGTH], int *taskCount);

int main() {
    char tasks[MAX_TASKS][MAX_LENGTH];
    int taskCount = 0;
    int choice;

    loadTasks(tasks, &taskCount);

    while (1) {
        printf("\nTo-Do List Manager\n");
        printf("1. View Tasks\n");
        printf("2. Add Task\n");
        printf("3. Delete Task\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar(); // To consume the newline

        switch (choice) {
            case 1:
                viewTasks(tasks, taskCount);
                break;
            case 2:
                addTask(tasks, &taskCount);
                break;
            case 3:
                deleteTask(tasks, &taskCount);
                break;
            case 4:
                saveTasks(tasks, taskCount);
                printf("Goodbye!\n");
                return 0;
            default:
                printf("Invalid choice. Try again.\n");
        }
    }
}

// Load tasks from file part
void loadTasks(char tasks[][MAX_LENGTH], int *taskCount) {
    FILE *file = fopen(FILENAME, "r");
    if (file == NULL) return;

    while (fgets(tasks[*taskCount], MAX_LENGTH, file) != NULL) {
        // Remove newline character
        tasks[*taskCount][strcspn(tasks[*taskCount], "\n")] = 0;
        (*taskCount)++;
    }
    fclose(file);
}

// Saving tasks to file part
void saveTasks(char tasks[][MAX_LENGTH], int taskCount) {
    FILE *file = fopen(FILENAME, "w");
    if (file == NULL) {
        perror("Failed to save tasks");
        return;
    }

    for (int i = 0; i < taskCount; i++) {
        fprintf(file, "%s\n", tasks[i]);
    }
    fclose(file);
}

// Adding a new task part
void addTask(char tasks[][MAX_LENGTH], int *taskCount) {
    if (*taskCount >= MAX_TASKS) {
        printf("Task limit reached.\n");
        return;
    }

    printf("Enter the task: ");
    fgets(tasks[*taskCount], MAX_LENGTH, stdin);
    tasks[*taskCount][strcspn(tasks[*taskCount], "\n")] = 0;
    (*taskCount)++;

    saveTasks(tasks, *taskCount); // Save immediately
    printf("Task added.\n");
}

// Viewing all tasks part
void viewTasks(char tasks[][MAX_LENGTH], int taskCount) {
    if (taskCount == 0) {
        printf("No tasks found.\n");
        return;
    }

    printf("Your Tasks:\n");
    for (int i = 0; i < taskCount; i++) {
        printf("%d. %s\n", i + 1, tasks[i]);
    }
}

// Delete a task by its number part
void deleteTask(char tasks[][MAX_LENGTH], int *taskCount) {
    int num;

    if (*taskCount == 0) {
        printf("No tasks to delete.\n");
        return;
    }

    viewTasks(tasks, *taskCount);
    printf("Enter task number to delete: ");
    scanf("%d", &num);
    getchar(); // Consume newline

    if (num < 1 || num > *taskCount) {
        printf("Invalid task number.\n");
        return;
    }

    // Shift tasks up
    for (int i = num - 1; i < *taskCount - 1; i++) {
        strcpy(tasks[i], tasks[i + 1]);
    }
    (*taskCount)--;

    saveTasks(tasks, *taskCount); // Save immediately
    printf("Task deleted.\n");
}
