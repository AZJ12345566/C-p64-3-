# C-p64-3-
C语言学习笔记 p64 动态内存分配(3)
#include<stdio.h>
#include<stdlib.h>
#include<windows.h>
int main()
{
    int* p=(int*)malloc(40);
    if(p==NULL)
    {
        return 0;
    }
    //使用空间
    int* p2=realloc(p,80);
    if(p2!=NULL)
    {
        p=p2;
    }
    free(p)
    p=NULL;
    return 0;
}

int main
{
    int* p=realloc(NULL,40);//相当于malloc(40)
    return 0;
}


//常见动态内存的错误
int main()
{
    //1.对空指针解引用操作
    int* p=malloc(40);
    //对p进行相关的判断
    *p=10;//有可能对NULL指针解引用失败-对NULL指针详解引用

    //2.对动态开辟内存的越界访问
    int*p=(int*)malloc(40);//10个int
    if(p==NULL)
    {
        return 0;
    }
    int i=0;
    //越界
    for(i=0;i<=10;i++)
    {
        *(p+i)=i;
    }
    free(p);
    p=NULL;

    //3.对非动态开辟内存使用free释放
    int a=10;
    int* p=&a;
    //...
    free(p);
    p=NULL;

    //4.使用free释放动态开辟内存的一部分
    int* p=(int*)malloc(40);
    if(p==NULL)
    {
        return 0;
    }
    int i=0;
    for(i=0;i<10;i++)
    {
        *p++=i;//这个循环体已经是指针p的指向空间发生变化
    }
    //回收空间
    free(p);
    p=NULL;

    //5.对同一块动态内存的多次释放
    int* p=(int*)malloc(40);
    if(p==NULL)
    {
        return 0;
    }
    //使用
    //释放
    free(p);
    p=NULL;
    //...
    free(p);//为了防止这种情况发生，要及时将p赋为空指针

    //6.对动态开辟内存的忘记释放(内存泄漏)
    while(1)
    {
        malloc(1);//这里就在一直开辟空间了
        Sleep(1000);
    }
    reutrn 0;
}



void GetMemory(char* p)//这里的p知识str的一份临时拷贝，在这里指向一个内存，但出了这块区域则无效了
{
    p=(char*)malloc(100);
}
void Test(void)
{
    char* str=NULL;
    GetMemory(str);//str以值传递的形式传给GetMemory函数，p是GetMemory的形参，只在函数内部有效
    strcpy(str,"hello world");//等GetMemory函数返回之后，动态开辟内存尚未释放并且无法找到，造成内存泄漏，所以程序崩溃
    printf(str);
}
int main()
{
    Test();
    return 0;
}

//上一个代码的正确版本1
void GetMemory(char** p)
{
    *p=(char*)malloc(100);
}
void Test(void)
{
    char* str=NULL;
    GetMemory(&str);
    strcpy(str,"hello world");
    printf(str);
    free(str);
    str=NULL;
}
int main()
{
    Test();
    return 0;
}

//改正版本2
void GetMemory(char* p)
{
    p=(char*)malloc(100);
    return p;
}
void Test(void)
{
    char* str=NULL;
    str=GetMemory(str);
    strcpy(str,"hello world");
    printf(str);
    free(str);
    str=NULL;
}
int main()
{
    Test();
    return 0;
}
