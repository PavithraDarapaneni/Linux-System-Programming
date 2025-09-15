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
## Write a C program to copy the contents of one file to another?
```c
#include <stdio.h>
#include <stdlib.h>
int main()
 {
    FILE *src, *dest;
    char ch;
    src = fopen("source.txt", "r");
    if (src == NULL)
	 {
        printf("Error! Could not open source file.\n");
        return 1;
    }
    dest = fopen("destination.txt", "w");
    if (dest == NULL)
	{
        printf("Error! Could not open/create destination file.\n");
        fclose(src);
        return 1;
    }
    while ((ch = fgetc(src)) != EOF)
	 {
        fputc(ch, dest);
    }
    printf("File copied successfully.\n");
    fclose(src);
    fclose(dest);
    return 0;
}
```

## Develop a C program to move a file from one directory to another?
```c
#include <stdio.h>
int main()
{
    int result;
    result = rename("myfile.txt", "Test/myfile.txt");
    if (result == 0)
	 {
        printf("File moved successfully.\n");
    }
 else {
        perror("Error moving file");
    }
    return 0;
}
```

## Implement a C program to list all files in the current directory?
```c
#include <stdio.h>
#include <dirent.h>
int main()
{
    DIR *d;
    struct dirent *dir;
    d = opendir(".");
    if (d == NULL) {
        perror("Unable to open directory");
        return 1;
    }
    printf("Files in current directory:\n");
    while ((dir = readdir(d)) != NULL) {
        printf("%s\n", dir->d_name);
    }
    closedir(d);
    return 0;
}
```
## Write a C program to get the size of a file named "file.txt"?
```c
#include <stdio.h>
int main()
{
    FILE *file;
    long size;
    file = fopen("file.txt", "r");
    if (file == NULL)
	 {
        printf("Error! Could not open file.\n");
        return 1;
    }
    fseek(file, 0, SEEK_END);
    size = ftell(file);
    printf("Size of 'file.txt' = %ld bytes\n", size);
    fclose(file);
    return 0;
}
```

