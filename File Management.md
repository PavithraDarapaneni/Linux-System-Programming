## Write a C program to create a new text file and write "Hello, World!" to it?
```c 
#include<stdio.h>
int main()
{
	FILE *fp;
	fp = fopen("output.txt","w");
	if(fp==NULL)
	{
		printf("Error opening file!\n");
		return 1;
	}
	fprintf(fp,"Hello,World!\n");
	fclose(fp);
	printf("Data Writtern to file successfully\n");
	return 0;
}
```

## Develop a C program to open an existing text file and display its contents?
```c
#include <stdio.h>
int main() 
{
    FILE *file;
    char ch;
    file = fopen("output.txt", "r");
    if (file == NULL) {
        printf("Error! Could not open file.\n");
        return 1;
    }
    while ((ch = fgetc(file)) != EOF) {
        putchar(ch); 
    }
    fclose(file);
    return 0;
}
```
## Implement a C program to create a new directory named "Test" in the current directory?
```c
#include <stdio.h>
#include <stdlib.h>
#ifdef _WIN32
int main()
{
int status;
 if (status == 0) {
        printf("Directory 'Test' created successfully.\n");
    } 
else {
        perror("Error creating directory");
    }

    return 0;
}
```

## Write a C program to check if a file named "sample.txt" exists in the current directory?
```c
#include <stdio.h>
int main() {
    FILE *file;
    file = fopen("sample.txt", "r");
    if (file == NULL)
	{
        printf("File 'sample.txt' does NOT exist in the current directory.\n");
    }
 else {
        printf("File 'sample.txt' exists in the current directory.\n");
        fclose(file); 
    }
    return 0;
}
```

## Develop a C program to rename a file from "oldname.txt" to "newname.txt"?
```c
#include <stdio.h>
int main()
 {
    int result;
    result = rename("oldname.txt", "newname.txt");
    if (result == 0)
	 {
        printf("File renamed successfully.\n");
   	 }
else {
        perror("Error renaming file");
    }
    return 0;
}
```

## Implement a C program to delete a file named "delete_me.txt"?
```c
#include <stdio.h>
int main()
 {
    int result;
    result = remove("delete_me.txt");
    if (result == 0)
	 {
        printf("File 'delete_me.txt' deleted successfully.\n");
	}
else {
        perror("Error deleting file");
    }
    return 0;
}
```
