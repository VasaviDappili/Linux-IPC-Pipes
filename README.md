# Linux-IPC--Pipes
Linux-IPC-Pipes
# Developed by: DAPPILI VASAVI
# Register number: 212223040030


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h> 
#include <sys/stat.h> 
#include <string.h> 
#include <fcntl.h> 
#include <unistd.h>
#include <sys/wait.h>

void server(int, int); 
void client(int, int); 

int main() { 
    int p1[2], p2[2], pid, *waits; 
    pipe(p1); 
    pipe(p2); 
    pid = fork(); 

    if (pid == 0) { 
        close(p1[1]); 
        close(p2[0]); 
        server(p1[0], p2[1]); 
        return 0;
    } 
    close(p1[0]); 
    close(p2[1]); 
    client(p1[1], p2[0]); 
    wait(waits); 
    return 0; 
} 

void server(int rfd, int wfd) { 
    int n; 
    char fname[2000]; 
    char buff[2000];
    n = read(rfd, fname, 2000);
    fname[n] = '\0';
    int fd = open(fname, O_RDONLY);
    sleep(10); 
    if (fd < 0) 
        write(wfd, "can't open", 9); 
    else 
        n = read(fd, buff, 2000); 
    write(wfd, buff, n); 
}

void client(int wfd, int rfd) {
    int n; 
    char fname[2000];
    char buff[2000];
    printf("ENTER THE FILE NAME :");
    scanf("%s", fname);
    printf("CLIENT SENDING THE REQUEST .... PLEASE WAIT\n");
    sleep(10);
    write(wfd, fname, 2000);
    n = read(rfd, buff, 2000);
    buff[n] = '\0';
    printf("THE RESULTS OF CLIENTS ARE ...... \n"); 
    write(1, buff, n);
}

```
## OUTPUT
![image](https://github.com/user-attachments/assets/58c14182-f297-486a-a6f5-b47a9a5f5081)


## C Program that illustrate communication between two process using named pipes using Linux API system calls
```
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>

int main() {
    // Try to create the FIFO and check if it already exists
    if (mkfifo("/tmp/my_fifo", 0777) == -1) {
        perror("Error"); // Will print "File exists" if already created
    } else {
        printf("FIFO created\n");
    }
    return 0;
}

```


## OUTPUT
![image](https://github.com/user-attachments/assets/58246ca4-f93f-4aa5-af50-077393447f01)



# RESULT:
The program is executed successfully.
