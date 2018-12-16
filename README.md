# HEeee
Just another repository
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>


typedef struct NODE
{
	int xh;
	char* name;
	char* phone;
	struct NODE* pNext;
}Node;

typedef struct PAGE
{
	int currentPage;  //当前第几页
	int onePageItem;  //一页有多少条数据
	int totalItem;    //共多少条数据
	int totalPage;    //共多少页
}Page;
int GetXh();

void AddNode(Node** pHead,Node** pEnd,Node* temp);  //链表添加
void InsertNode(int xh,Node** pHead,Node** pEnd,Node* temp);    //链表插入
Node* GetNode();   //链表删除
Node* GetNodeIn();  
void DelNode(int xh,Node** pHead,Node** pEnd); //删除节点
char* GetName();  
char* GetPhone();
void InitInfo(int count,Node** pHead,Node** pEnd);  //初始化信息

Page* GetPage(Node* pHead);
void ShowMenu(Page* page);      
void ShowInfo(Node* pHead,Page* page);

char GetKey();

void OperatePage(Node* pHead,Page* page);
void LookTXL(Node* pHead);
char* getstring();

void Chaxun(Node* pHead);


char g_key;
int g_menu_type;

void DelTXL(Node** pHead,Node** pEnd);
void Updata(Node* pHead);
int main()
{
	Node* head = NULL;
	Node* end = NULL;
	char c;
	InitInfo(80,&head,&end);

	while(1)
	{	
		printf("1.查看通讯录\n");
		printf("2.添加联系人\n");
		printf("3.查询\n");
		printf("4.删除\n");
		printf("5.修改\n");
		printf("q.退出\n");
		//1.显示主菜单


		//2.输入
		c = GetKey();
		//3.根据输入去调用对应的函数
		switch(c)
		{
		case '1': //分页查看
			g_menu_type = 1;
			LookTXL(head);
			break;
		case '2': //添加
			AddNode(&head,&end,GetNodeIn());
			break;
		case '3': //查询
			g_menu_type = 3;
			Chaxun(head);
			break;
		case '4': //删除
			g_menu_type = 4;
			DelTXL(&head,&end);
			break;
		case '5': //修改
			g_menu_type = 5;
			Updata(head);
			break;
		case 'q': //退出
			return  0;
			break;
		}
	
	
	}


	return 0;
}


void AddNode(Node** pHead,Node** pEnd,Node* temp)
{


	//2.把节点添加到链表上
	//当前是否有链表
	if(*pHead == NULL)
	{
		//没有： 新来的节点既是头节点又是尾节点，头尾指针指向新来的节点
		*pHead = temp;
	}
	else
	{
		//有：1.连上：尾节点里的pnext指针装的是新来的节点的地址
		//	2.尾指针指向新来的节点
		(*pEnd)->pNext = temp;
	}

	*pEnd = temp;	
}

int GetXh()
{
	static int i = 1;
	return i++;
}


void InsertNode(int xh,Node** pHead,Node** pEnd,Node* temp)
{
	Node* bj = *pHead; //相当于 把主函数head 的值给了bj  bj现在装的就是头结点的地址
	//Node ** bj = pHead;



	//首先判断 是否是头插入
	if(xh == (*pHead)->xh)
	{
		temp->pNext = *pHead;
		*pHead = temp;
		return;
	}

	//中间插入
	while(bj->pNext != NULL)
	{
		if(bj->pNext->xh == xh)
		{
			//1 -- 2 --3 --4  我要在3前插入节点
			//bj现在指向2，让新来的节点的pnext 指针 = &3； 2的pnext指针装的是新来的节点的地址
			temp->pNext = bj->pNext;
			bj->pNext = temp;
			return;
		}
		bj = bj->pNext;
	}


	(*pEnd)->pNext = temp;
	*pEnd = temp;
}

Node* GetNode()
{
	Node* temp = (Node*)malloc(sizeof(Node));
	temp->xh = GetXh();
	temp->name = GetName();
	temp->phone = GetPhone();
	temp->pNext = NULL;

	return temp;
}

Node* GetNodeIn()
{
	Node* temp = (Node*)malloc(sizeof(Node));
	temp->xh = GetXh();
	printf("请输入新添加的联系人的名字:");
	temp->name = getstring();
	printf("请输入新添加的联系人的电话:");
	temp->phone = getstring();
	temp->pNext = NULL;

	return temp;
}

void DelNode(int xh,Node** pHead,Node** pEnd)
{
	Node* pDel = NULL;
	Node* bj = *pHead;
	//1.判断是否是头删除
	if(xh == (*pHead)->xh)
	{
		pDel = *pHead;
		*pHead = (*pHead)->pNext;
		free(pDel);
		pDel = NULL;
		return ;
	}

	//2.中间删除
	while(bj->pNext != NULL)
	{
		if(bj->pNext->xh == xh)
		{
			//1--2--3--4；假如要删除的是3 当前bj指向2
			pDel = bj->pNext;
			//1--2--4
			bj->pNext = bj->pNext->pNext;
			free(pDel);
			pDel = NULL;

			//1--2--3--4 假如删除的是4 当前bj指向3
			if(bj->pNext == NULL)
			{
				*pEnd = bj;
			}	
			return;
		}
		bj = bj->pNext;
	}


}

char* GetName()
{

	char* name = (char*)malloc(sizeof(char)*6);
	int i;
	
	for(i=0;i<5;i++)
	{
		name[i] = rand()%26 + 'a';
	}
	name[5] = '\0';
	return name;
}

char* GetPhone()
{
	char* phone = (char*)malloc(12);
	int i;
	switch(rand()%4)
	{
	case 0:
		strcpy_s(phone,12,"137");
		break;
	case 1:
		strcpy_s(phone,12,"139");
		break;
	case 2:
		strcpy_s(phone,12,"187");
		break;
	case 3:
		strcpy_s(phone,12,"189");
		break;
	
	}

	for(i=3;i<11;i++)
	{
		phone[i] = rand()%10 + '0';
	}

	phone[11] = '\0';
	return phone;
}

void InitInfo(int count,Node** pHead,Node** pEnd)
{
	int i;
	for(i=0;i<count;i++)
	{
		AddNode(pHead,pEnd, GetNode());
	}
}

Page* GetPage(Node* pHead)
{
	Page* page = (Page*)malloc(sizeof(Page));
	page->currentPage = 0;
	page->onePageItem = 10;
	//链表有几个节点 就有几条数据

	page->totalItem = 0;
	while(pHead != NULL)
	{
		page->totalItem++;
		pHead = pHead->pNext;
	}

	//if(page->totalItem % page->onePageItem == 0)
	//{
	//	page->totalPage = page->totalItem/page->onePageItem;
	//}
	//else
	//{
	//	page->totalPage = page->totalItem/page->onePageItem+1;
	//}

	// A ？ B ： C；
	page->totalPage = page->totalItem % page->onePageItem == 0 ? page->totalItem/page->onePageItem : page->totalItem/page->onePageItem+1;
	return page;
}

void ShowMenu(Page* page)
{
	switch(g_menu_type)
	{
	case 1:
		printf("当前第%d页 共%d页 共%d条 w上一页 s下一页  b返回主菜单\n",page->currentPage,page->totalPage,page->totalItem);
		break;
	case 3:
		printf("当前第%d页 共%d页 共%d条 w上一页 s下一页 c重新查询 b返回主菜单\n",page->currentPage,page->totalPage,page->totalItem);
		break;
	case 4:
		printf("当前第%d页 共%d页 共%d条 w上一页 s下一页 c重新查询 d删除 b返回主菜单\n",page->currentPage,page->totalPage,page->totalItem);
		break;
    case  5:
		printf("当前第%d页 共%d页 共%d条 w上一页 s下一页 c重新查询 u修改信息 b返回主菜单\n",page->currentPage,page->totalPage,page->totalItem);
		break;
	}
}

void ShowInfo(Node* pHead,Page* page)
{
	int begin = (page->currentPage-1) * page->onePageItem +1;
	int end = page->currentPage * page->onePageItem;
	int count = 0;
	while(pHead != NULL)
	{
		count++;
		if(count >= begin && count <= end)
		{
			printf("%d %s %s\n",pHead->xh,pHead->name,pHead->phone);
		}

		pHead = pHead->pNext;
	}
}


void OperatePage(Node* pHead,Page* page)
{
	char key = 's';
	while(1)
	{	
		switch(key)
		{
		case 'w':
			if(page->currentPage == 1)
			{
				printf("当前已经是第一页了\n");
			}
			else
			{
				page->currentPage--;
				ShowInfo(pHead,page);
				ShowMenu(page);
			}
			break;
		case 's':
			if(page->currentPage == page->totalPage)
			{
				printf("当前已经是最后一页了\n");
			}
			else
			{
				page->currentPage++;
				ShowInfo(pHead,page);
				ShowMenu(page);
			}
			break;
		case 'b':
		case 'd':
		case 'c':
		case 'u':
			return;
			break;
		default:
			printf("按错了\n");
			break;
		}

		g_key = key = GetKey();


	}

}

char GetKey()
{

	//1.输入时带的'\n':  s\n；  '\n'也取走了，并且返回‘s’;
	//2.如果输入缓冲区已经有一个'\n'存在了，
	char c;
	char z = -1;
	int a = 1;
	while( (c=getchar()) != '\n'|| a== 1)
	{
		a = 0;
		z = c;
	}

	return z;

} 


void LookTXL(Node* pHead)
{
	Page*page = GetPage(pHead);
	OperatePage(pHead,page);
    
}



char* getstring()
{
		char c;
	char* str= (char*)malloc(5);
	int count = 0;
	int size = 5;
	char* newStr = NULL;
	char* bj = str;
	while( (c=getchar()) != '\n')
	{
		//1.把c存到空间里,来个变量记录一下存了几个
		*str = c;
		str++;
		count++;
		//2.判断是否还够存，如果够存 继续1 if
		if(count+1 == size)
		{
			//2.1.把'\0'存进去 变成字符串
			*str = '\0';
			//2.2.申请一个新的空间
			size += 5;
			newStr = (char*)malloc(size);
			//2.3.把旧空间里的东西拷贝到新空间里（strcpy）
			strcpy_s(newStr,size,bj);
			//2.4.释放旧的空间
			free(bj);
			str = newStr+count;
			bj = newStr;
		}
	}
	*str = '\0';
	return bj;
}


void Chaxun(Node* pHead)
{
	char key;
	char* keyword;
	Node* node;
	Node* newHead = NULL;
	Node* newEnd = NULL;
	Node* bj = pHead;
	while(1)
	{
		while(1)
		{
			printf("请输入要查询的关键字:\n");
			keyword = getstring();
			printf("按a确定，其他键重新输入:");

			key = GetKey();
			if(key == 'a')
			{
				break;
			}
		}


		//2.1 根据关键字去链表中找对应的节点（联系人）的信息 支持前缀模糊查询 strncmp
		pHead = bj;
		while(pHead)
		{
			if(strncmp(keyword,pHead->name,strlen(keyword)) == 0 ||strncmp(keyword,pHead->phone,strlen(keyword)) == 0  )
			{
				//2.2创建一个新的节点(申请空间)
				node = (Node*)malloc(sizeof(Node));
				//2.3把head的信息复制过去
				node->xh = pHead->xh;
				node->name = pHead->name;
				node->phone = pHead->phone;
				node->pNext = NULL;

				//2.4吧新的节点添加到新的链表上
				AddNode(&newHead,&newEnd,node);
			}
			pHead  = pHead->pNext;
		}


		////3.1 判断是否找到了
		if(newHead == NULL)
		{
			printf("没找到\n");
		}
		else
		{
			LookTXL(newHead); //退出有2种可能 c b
			//TODO: 释放链表
			newHead = NULL;
			newEnd = NULL;

			//如果按c 不用管 按b return
			if(g_key == 'b' || g_key == 'd' || g_key == 'u')
			{
				return ;
			}
		}
	}
}	

void DelTXL(Node** pHead,Node** pEnd)
{
	//1.调用查询功能
	int xh;
	//Chaxun(*pHead);
	//if(g_key == 'b')
	//{
	//	return;
	//}
	//请输入要删除的编号
	printf("请输入要删除的编号:");
	xh = atoi(getstring());
	DelNode(xh,pHead,pEnd);
}	


void Updata(Node* pHead)
{
	int xh;
	//Chaxun(pHead);
	//if(g_key == 'b')
	//{
	//	return;
	//}

	//请输入要删除的编号
	printf("请输入要修改的编号:");
	xh = atoi(getstring());
	while(pHead)
	{
	
		if(pHead->xh == xh)
		{
			printf("请输入修改后的名字：");
			free(pHead->name);
			pHead->name = getstring();
			printf("请输入修改后的电话:");
			free(pHead->phone);
			pHead->phone = getstring();
			break;
		
		}
		pHead = pHead->pNext;
	}


}
