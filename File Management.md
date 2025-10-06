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
11.Develop a C program to check if a directory named "Test" exists in the current directory?
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<dirent.h>
int main(){
        const char *dirname="Test";
        DIR *dir;
        dir=opendir(dirname);
        if(dir){
                printf("Directory %s exists in the current directory\n",dirname);
                closedir(dir);
        }
        else{
                printf("Directory '%s' does not exist.\n", dirname);
        }
}
12.Implement a C program to create a new directory named "Backup" in the parent directory?
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        if(execlp("mkdir","mkdir","-p","Backup",NULL)==-1){
                perror("Error");
                exit(1);
        }
}
13.Write a C program to recursively list all files and directories in a given directory?
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<dirent.h>
void listfilesrecursively(const char *basepath,int depth){
        struct dirent *dp;
        DIR *dir=opendir(basepath);
        if(!dir){
                printf("Error");
                exit(1);
        }
        while((dp=readdir(dir))!=NULL){
                if((strcmp(dp->d_name,".")==0)||(strcmp(dp->d_name,"..")==0)){
                        continue;
                }
                for(int i=0;i<depth;i++){
                        printf(" ");
                }
                printf("%s\n",dp->d_name);
                char path[100];
                snprintf(path,sizeof(path),"%s/%s",basepath,dp->d_name);
                struct stat statbuf;
                if(stat(path,&statbuf)==0 && S_ISDIR(statbuf.st_mode)){
                        listfilesrecursively(path,depth+1);
                }
        }
        closedir(dir);
}
14.Develop a C program to delete all files in a directory named "Temp"?
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<dirent.h>
#include<unistd.h>
int main(){
        const char *dirname="Test";
        struct dirent *entry;
        DIR *dir;
        dir=opendir(dirname);
        if(dir==NULL){
                printf("Error");
                exit(1);
        }
        while((entry=readdir(dir))!=NULL){
                char filepath[100];
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
                        continue;
                snprintf(filepath,sizeof(filepath),"%s/%s",dirname,entry->d_name);
                if(unlink(filepath)==0)
                        printf("Deleted:%s\n",filepath);
                else
                        perror("Error\n");
        }
        closedir(dir);
}
15.Implement a C program to count the number of lines in a file named "data.txt"?
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        char buffer[1024];
        int size,count=0;
        fd=open("file1.c",O_RDONLY,0644);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        while((size=read(fd,buffer,sizeof(buffer)))>0){
                for(int i=0;i<size;i++){
                        if(buffer[i]=='\n'){
                                count++;
                        }
                }
        }
        if(size<0){
                printf("Error");
                close(fd);
                exit(1);
        }
        close(fd);
        printf("No of Lines:%d",count);
}
16.Write a C program to append "Goodbye!" to the end of an existing file named "message.txt"?
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("message.txt",O_WRONLY|O_APPEND,0666);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        if(write(fd,"Good Bye",sizeof("Good Bye")-1)<0){
                printf("Error");
                close(fd);
                exit(1);
        }
        close(fd);
}
17.Implement a C program to change the permissions of a file named "file.txt" to readonly?
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<fcntl.h>
#include<sys/stat.h>
int main(){
        if(chmod("file1.txt",S_IRUSR|S_IRGRP|S_IROTH)==0){
                printf("Permissions are changed\n");
        }
        else{
                printf("Error");
                exit(1);
        }
}
18.Write a C program to change the ownership of a file named "file.txt" to the user "user1"?
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<fcntl.h>
int main(){
        struct passwd *pwd=getpwnam("user1");
        if(pwd==NULL){
                printf("Not found");
                exit(1);
        }
        uid_t uid = pwd->pw_uid;
        if(chown("file.txt",uid,-1)==0)
                printf("Ownership changed\n");
        else
                printf("Not changed\n");
}
19.Develop a C program to get the last modified timestamp of a file named "file.txt"?
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
#include<sys/stat.h>
#include<time.h>
int main(){
        struct stat filestat;
        if(stat("file.txt",&filestat)<0){
                printf("Error");
                exit(1);
        }
        printf("Last modified time of %s : %s ","file.txt",ctime(&filestat.st_mtime));
}
20.Implement a C program to create a temporary file and write some data to it?
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("file.txt",O_WRONLY|O_CREAT,0666);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        char ch[]="Hello world";
        if(write(fd,ch,sizeof(ch))<0){
                printf("Error in writing to file");
                exit(1);
        }
        close(fd);
}
21.Write a C program to check if a given path refers to a file or a directory?
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<sys/stat.h>
int main(){
        char path[100];
        struct stat pathstat;
        printf("Enter the path:");
        scanf("%s",path);
        if(stat(path,&pathstat)!=0){
                printf("error");
                exit(1);
        }
        if(S_ISREG(pathstat.st_mode)){
                printf("%s is a regular file",path);
        }
        else if(S_ISDIR(pathstat.st_mode)){
                printf("%s is a directory",path);
        }
        else{
                printf("%s is neither a file nor a directory",path);
        }
}
22.Develop a C program to create a hard link named "hardlink.txt" to a file named "source.txt"?
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        if(link("source.txt","Hardlink.txt")==0){
                printf("Hardlink Hardlink.txt is created successfully pointing to source.txt\n");
        }
        else{
                perror("Error");
                exit(1);
        }
}
23.Implement a C program to read and display the contents of a CSV file named "data.csv"?
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd,len;
        char str[100];
        fd=open("data.csv",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        while((len=read(fd,str,sizeof(str)-1))>0){
                str[len]='\0';
                printf("%s",str);
        }
        close(fd);
}
24.Write a C program to get the absolute path of the current working directory?
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        char cwd[1024];
        if(getcwd(cwd,sizeof(cwd))!=NULL){
                printf("Current working directory:%s",cwd);
        }
        else{
                perror("Error");
                exit(1);
        }
}
25.Develop a C program to get the size of a directory named "Documents"?
26.Implement a C program to recursively copy all files and directories from one directory to another?
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
#include<dirent.h>
#include<errno.h>
#include<sys/stat.h>
void copyfiles(const char *src,const char *des){
        int fdin,fdout;
        ssize_t nread;
        char buffer[4096];
        fdin=open(src,O_RDONLY);
        if(fdin<0){
                perror("Open source file");
                exit(1);
        }
        fdout=open(des,O_WRONLY|O_CREAT|O_TRUNC,0644);
        if(fdout<0){
                perror("open destination file");
                close(fdin);
                exit(1);
        }
        while((nread=read(fdin,buffer,sizeof(buffer)))>0){
                if(write(fdout,buffer,nread)!=nread){
                        perror("write");
                        close(fdin);
                        close(fdout);
                }
        }
        if(nread<0)
                perror("read");
        close(fdin);
        close(fdout);
}
void copydirectory(const char *src,const char *des){
        DIR *dir;
        struct dirent *entry;
        struct stat statbuf;
        char srcpath[1024],despath[1024];
        if(mkdir(des,0755)==-1 && errno != EEXIST){
                perror("mkdir");
                exit(1);
        }
        dir=opendir(src);
        if(!dir){
                perror("Opendir");
                exit(1);
        }
        while((entry=readdir(dir))!=NULL){
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
                        continue;
                snprintf(srcpath,sizeof(srcpath),"%s/%s",src,entry->d_name);
                snprintf(despath,sizeof(despath),"%s/%s",des,entry->d_name);
                if(stat(srcpath,&statbuf)==-1){
                        perror("staf");
                        exit(1);
                }
                if(S_ISDIR(statbuf.st_mode))
                        copydirectory(srcpath,despath);
                else if(S_ISREG(statbuf.st_mode))
                        copyfiles(srcpath,despath);
        }
        closedir(dir);
}
int main(int argc,char *argv[]){
        if(argc != 3){
                fprintf(stderr,"usage:%s <sourcedir> <destinationdir>\n",argv[0]);
                exit(1);
        }
        copydirectory(argv[1],argv[2]);
        printf("Copied directory from %s to %s\n",argv[1],argv[2]);
}
27.Write a C program to get the number of files in a directory named "Images"?
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<dirent.h>
int main(){
        DIR *dir;
        struct dirent *entry;
        int count=0;
        dir=opendir("Images");
        if(dir==0){
                perror("Error");
                exit(1);
        }
        while((entry=readdir(dir))!=NULL){
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
                        continue;
                if(entry->d_type == DT_REG)
                        count++;
        }
        closedir(dir);
        printf("Number of files : %d",count);
}

