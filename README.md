# NU202
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

void NEW(int *p);//实现新手教程中与小怪的战斗
void BOSS(int *p);//实现与BOSS的战斗
int BLESS(int t);//判断是否获得随机
void writeFile(char* fileStr);//将作战结果记录于一个文件
int founction(int t);//添加一个随机数，用于达到祝福的随机效果
void FEN(int f);//添加分割线
int back;//设置全局变量，用于writeFile函数中的判断语句

int main()
{
    int a[3],pick,i,ret;
    printf("勇士欢迎来到冒险大陆！\n现在先为自己取个名字吧：\n");
    char name[20];
    gets(name);
    printf("你的名字是“%s”",name);//可以替换名称，以玩家输入的名称作为主角
    FEN(1);
    printf("初来乍到，先选择你的职业吧。\n");
    printf("【1】剑士 HP(150) ATK(25) DEF(20)\n【2】弓箭手 HP(100) ATK(50) DEF(5)\n【3】盾士 HP(200) ATK(15) DEF(25)\n");
    printf("输入序号来选择：");
    for(i=0;; i++) //对输入非法字符的检查与处理
    {
        ret=scanf("%d",&pick);
        if(ret!=1)
        {
            printf("格式错误，请重新输入！");
            fflush(stdin);
        }
        else
        {
            break;
        }
    }
    switch(pick)//用于玩家选择职业，并对各职业的血量、攻击、防御初始化
    {
    case 1:
        a[0]=150;
        a[1]=25;
        a[2]=20;
        printf("你已选定【剑士】\n");
        break;
    case 2:
        a[0]=100;
        a[1]=50;
        a[2]=5;
        printf("你已选定【弓箭手】\n");
        break;
    case 3:
        a[0]=200;
        a[1]=15;
        a[2]=25;
        printf("你已选定【盾士】\n");
        break;
    }
    FEN(1);
    NEW(a);
    BOSS(a);
    writeFile("D:\\战斗记录。txt");
    return 0;
}
void NEW(int *p)
{
    int hp0=50,f0=25,d0=3;
    int instruct,i,ret;
    printf("初入冒险，开始你的第一次冒险吧！\n");
    printf("当你走出村门时，发现了菜鸡一只，你决定把它当作你的练手对象。\n");
    FEN(1);
    while(p[0]>0&&hp0>0)
    {
        printf("轮到你的回合\n请选择【1】攻击还是【2】防御：\n");
        for(i=0;; i++) //对非法字符的处理
        {
            ret=scanf("%d",&instruct);
            if(ret!=1)
            {
                printf("格式错误，请重新输入！\n");
                fflush(stdin);
            }
            if(instruct==1||instruct==2)
            {
                break;
            }
            else
            {
                printf("无此选项，请重新输入！\n");
            }
        }
        if(instruct==1)
        {
            if(hp0-(p[1]-d0)<0)//当敌人血量小于0时，将血量变为0，避免血量为负
            {
                hp0=0;
            }
            if(hp0-(p[1]-d0)>0)
            {
                hp0=hp0-(p[1]-d0);//若血量仍为正，则正常加减
            }
            printf("你对菜鸡发起了攻击，造成了%d点伤害，菜鸡HP变为%d\n",p[1],hp0);
            FEN(1);     
        }
        if(instruct==2)
        {
            p[0]=p[0]+15;//加血
            printf("你使用了回血道具，生命值回复15点\n");
        }
        if(hp0>0)
        {
            printf("你的回合结束了，轮到菜鸡行动了！\n");

            if(f0-p[2]<=0)//由于菜鸡攻击小于角色防御力，避免出现倒加血的现象
            {
                printf("菜鸡未能击穿你的护甲，没有对你造成任何伤害\n");
            }
            if(f0-p[2]>0)//攻击大于防御力，则正常加减
            {
                p[0]=p[0]-(f0-p[2]);
                printf("菜鸡对你造成了%d点伤害\n",f0-p[2]);
                FEN(1);
            }
        }
    }
    if(p[0]<=0)
    {
        printf("你竟然被菜鸡打败了，看来冒险不适合你，洗洗睡吧！\n");//可重新挑战
        FEN(1);
    }
    if(hp0<=0&&p[0]>0)
    {
        printf("菜鸡都被你打败了，看来你就是那个天选之人！\n");
        p[0]=p[0]+20;
        p[1]=p[1]+15;
        p[2]=p[2]+5;//提升角色的数值
        printf("你的属性获得了提升：HP+20=%d  ATK+15=%d  DEF+5=%d\n",p[0],p[1],p[2]);
        printf("你继续向远处走去。\n");
        FEN(1);
    }
}
void BOSS(int *p)
{
    int hp1=200,d1=25,instruct,count,i,ret;
    int z=BLESS(1);
    FEN(1);
    printf("你的表现已经惊动了大魔王！他发现你了！\n");
    if(z==2)//加入判断语句，决定是否有女神的祝福
    {
        printf("女神的祝福没有降临到你的身上，你必须孤军奋战！\n准备战斗吧！勇士！");
    }
    if(z==1)
    {
        printf("女神的祝福降临了！你获得了随机buff加持。\n准备战斗吧！勇士！");
    }
    while(p[0]>0&&hp1>0)
    {
        srand(time(NULL));
        int t=rand()%10+30;//将敌人的攻击变为随机值
        FEN(1);
        printf("轮到你的回合\n请选择【1】攻击还是【2】防御：\n");
        for(i=0;; i++) //对非法字符的处理
        {
            ret=scanf("%d",&instruct);
            if(ret!=1)
            {
                printf("格式错误，请重新输入！\n");
                fflush(stdin);
            }
            if(instruct==1||instruct==2)
            {
                break;
            }
            else//避免除1、2、3的数字
            {
                printf("无此选项，请重新输入！\n");
            }
        }
        if(instruct==1)
        {
            if(hp1-(p[1]-d1)<0)//同菜鸟
            {
                hp1=0;
            }
            if(hp1-(p[1]-d1)>0)
            {
                hp1=hp1-(p[1]-d1);
            }
            printf("你对魔王发起了攻击，造成了%d点伤害，魔王HP变为%d\n",p[1]-d1,hp1);
            if(hp1<=0&&p[0]>0)
            {
                printf("你成功击败了魔王！你已经成为了一名合格的冒险者！\n");
                back=1;
            }
        }
        if(instruct==2&&z==1)
        {
            count=founction(count);//设置祝福的随机效果
            if(count%3==0||count%5==0)
            {
                p[0]=p[0]+30;
                printf("受到女神的回血祝福，生命值得到提升:HP+30，生命值变为%d\n",p[0]);
            }
            if(count%5==3)
            {
                p[1]=p[1]+10;
                printf("受到女神的增伤祝福，攻击力得到提升:ATK+10，攻击力变为%d\n",p[1]);
            }
            if(count%6==2)
            {
                p[2]=p[2]+5;
                printf("受到女神的增防祝福，防御力得到提升:DEF+5,防御力变为%d\n",p[2]);
            }
            if(count%3!=0&&count%5!=3&&count%6!=2&&count%5!=0)
            {
                printf("魔王干扰了女神对你的感应，祝福失败，下一回合试试？\n");
            }
        }
        if(instruct==2&&z==2)//设置未获得祝福时二技能的效果
        {
            p[0]=p[0]+15;
            printf("你使用了回血道具，生命值回复15点\n");
        }
        FEN(1);
        printf("你的回合结束了，轮到魔王行动了！\n");
        if(p[0]-(t-p[2])<0)//同菜鸟
        {
            p[0]=0;
        }
        if(p[0]-(t-p[2])>0)
        {
            p[0]=p[0]-(t-p[2]);//魔王攻击后，勇士的血量变化
        }
        if(t-p[2]<=0)//同菜鸟
        {
            printf("皮糙肉厚的你抗住了魔王的攻击\n");
        }
        else
        {
            printf("魔王对你造成了%d点伤害,你的生命值变为%d\n",t-p[2],p[0]);
        }
    }
    if(hp1<=0&&p[0]>0)
    {
        printf("你成功击败了魔王！你已经成为了一名合格的冒险者！\n");//可添加组成结算画面的函数
        back=1;
    }
    if(p[0]<=0&&hp1>0)
    {
        printf("很遗憾，你被魔王击败了！\n");//可让玩家选择是否继续挑战魔王
        back=0;
    }
}
int BLESS(int t)
{
    int answer,i,ret;
    printf("你发现了幸运女神的祭坛，上面有一道谜语，答对了或许会有惊喜！\n");
    printf("南京学校决赛圈吃鸡的学校是哪一个？\n");//添加一个谜语
    printf("《1》南京医科大学  《2》南京中医药大学 《3》中国药科大学\n");
    printf("请输入答案序号：");
    for(i=0;; i++) //对非法字符的处理
    {
        ret=scanf("%d",&answer);
        if(ret!=1)
        {
            printf("格式错误，请重新输入：");
            fflush(stdin);
        }
        if(answer==1||answer==2||answer==3)
        {
            break;
        }
        else
        {
            printf("无此选项，请重新输入：");
        }
    }
    if(answer==1)
    {
        printf("恭喜你答对，获得女神祝福！\n");
        return 1;
    }
    printf("答错了喔，没获得女神祝福！\n");
    return 2;
}
void writeFile(char* fileStr)
{
    FILE *fp;
    fp = fopen(fileStr,"a+");
    if(fp == NULL)
    {
        printf ("something was wrong !");
        return ;
    }
    if(back==1)
    {
        fputs ("victory!",fp);
        fputs (" ",fp);
        fclose(fp);
        printf ("*********战绩写入成功***********");
    }
    else
    {
        fputs ("defeat!",fp);
        fputs (" ",fp);
        fclose(fp);
        printf ("*********战绩写入成功***********");
    }
}
int founction(int s)
{
    srand(time(NULL));
    s=rand()%32;
    return s;
}
void FEN(int f)
{
    printf("\n");
    printf("-=-=-=-==-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-");
    printf("\n");
}
