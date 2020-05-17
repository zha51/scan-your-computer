# scan-your-computer
扫描电脑进程信息

简单单线程扫描

// PortScanf.cpp : 定义控制台应用程序的入口点。
//
#define WIN32_LEAN_AND_MEAN
#include "stdafx.h"
#include <WinSock2.h>
#pragma comment(lib, "Ws2_32")
int scant(char *Ip, int StartPort, int EndPort)
{  
    WSADATA wsa;
    SOCKET s;
    struct sockaddr_in server;
 
    int CurrPort;    //当前端口
    int ret;
 
    WSAStartup(MAKEWORD(2, 2), &wsa);    //使用winsock函数之前，必须用WSAStartup函数来装入并初始化动态连接库
 
    server.sin_family = AF_INET;    //指定地址格式，在winsock中只能使用AF_INET
    server.sin_addr.s_addr = inet_addr(Ip); //指定被扫描的IP地址
 
    for (CurrPort = StartPort; CurrPort <= EndPort; CurrPort++)
    {
        s = socket(PF_INET, SOCK_STREAM, IPPROTO_TCP);
        server.sin_port = htons(CurrPort); //指定被扫描IP地址的端口号
        ret = connect(s, (struct sockaddr *)&server, sizeof(server)); //连接
 
        if (0 == ret) //判断连接是否成功
        {
            printf("%s:%d Success O(∩_∩)O~~\n", Ip, CurrPort);
            closesocket(s);
        }
        else {
            printf("%s:%d Failed\n", Ip, CurrPort);
        }
    }  
    printf("Cost time:%f second\n", CostTime); //输出扫描过程中耗费的时间
    WSACleanup();    //释放动态连接库并释放被创建的套接字
    return 1;
}
 
int main()
{
    scant("127.0.0.1", 75, 100);
     
    return 0;
    
    多线程扫描
    
    typedef struct _tagValue
{
    int start;
    int end;
}PortNums;
 
void _cdecl beginThreadFunc1(LPVOID lpParam) {
    PortNums *pnInt = (PortNums*)lpParam;
    scan("127.0.0.1", pnInt->start, pnInt->end);
}
 
int a()
{
    PortNums m1;
    m1.start = 70;
    m1.end = 500;
 
    PortNums m2;
    m2.start = 501;
    m2.end = 1000;
 
    _beginthread(beginThreadFunc1, 0, &m1);
    _beginthread(beginThreadFunc1, 0, &m2);
 
 
    getchar();
    return 0;
}
