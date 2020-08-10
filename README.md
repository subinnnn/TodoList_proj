# TodoList_proj
2019 - C programming team project

#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include<string.h>
#include<stdlib.h>

#define MAX_NUM 100
#define SWAP(x,y,t) ((t)=(x), (x)=(y), (y)=(t))
#define RED "\x1b[31m"
#define BLUE "\x1b[34m"
#define YELLOW "\x1b[33m"
#define RESET "\x1b[0m"

//to-do-list함수
void print_main();
void print(int input);
void print_all();
int confirm();
void sort();
void sort_date();
void sort_priority();
int degenerate_();
void search();
int insert();
void elf();
void check();
void modified();
void save();
void delete_();
int open(FILE*);


//calendar함수
int* myatoi(int, int);
void calendar();
int day_of_week(int, int);
void print_calendar(int, int, int, int todo[]);
void print_head(int, int);

FILE* fp;         

typedef struct {
    char did;       //O & X로 했는 지 안했는 지 구별
    char date[30];  //날짜 저장
    char todo[50];  //해야할 일 저장
    char priority;  //우선순위 저장
}TODO;

TODO* todo[MAX_NUM];

int len = 0; //Variable that save struct length

int main(int argc, char const* argv[]) {

    int input;
    open(fp);
    while (1) {

        printf("\n\t\t\t┌──────────────────────────────┐");
        printf("\n\t\t\t│          * MENU *            │");
        printf("\n\t\t\t│  1. Insert                   │");
        printf("\n\t\t\t│  2. Check                    │");
        printf("\n\t\t\t│  3. Search                   │");
        printf("\n\t\t\t│  4. Modified                 │");
        printf("\n\t\t\t│  5. Delete                   │");
        printf("\n\t\t\t│  6. Print                    │");
        printf("\n\t\t\t│  7. Save                     │");
        printf("\n\t\t\t│  8. Exit                     │");
        printf("\n\t\t\t└──────────────────────────────┘");
        printf("\n\t\t\t PRESS NUMBER : ");

        scanf("%d", &input);

        switch (input) {
        case 1: insert(); break;
        case 2: check(); break;
        case 3: search(); break;
        case 4: modified(); break;
        case 5: delete_(); break;
        case 6: print_main(); break;
        case 7: save(); break;
        case 8: return 0;
        default: printf("\n\t\t\t =========== PRESS AGAIN ! ===========\n"); break;
        }
    }
    return 0;
}


int degenerate_() { //모든 제어 경로에서 값을 반환하지않는다니까 수정하기 **
    if (len == 0) {
        printf("\n\t\t   =========== TODO LIST IS EMPTY ! ===========\n");
        return 1;
    }
}


int insert() {

    todo[len] = (TODO*)malloc(sizeof(TODO));

    char idate[30];
    char itodo[50];
    char ipriority;

    printf("\n\t\t\t========= ENTER TO DO ==========\n");
    printf("\t\t\tDATE : ");
    scanf("%s", idate);
    strcpy(todo[len]->date, idate);

    printf("\t\t\tTODO : ");
    scanf(" %[^\n]", itodo);        //엔터를 제외한 모든 문자열 입력받기
    strcpy(todo[len]->todo, itodo);
    printf("\t\t\tDo you want to set the priority?(Y/N) : ");

    scanf(" %c", &ipriority);    //scanf버퍼가 '\n'을 입력 받아버려서 오류가 났던 거라 버퍼 한칸 띄어줌

    if (ipriority == 'Y' || ipriority == 'y') {
        printf("\t\t\tPRIORITY(1~9) : ");
        scanf(" %c", &todo[len]->priority);
    }
    else if (ipriority == 'N' || ipriority == 'n')
        todo[len]->priority = '-';
    else {
        printf("\n\t\t\t=========== EROOR : PRIORITY ! ===========\n");
        return 0;
    }

    todo[len]->did = 'X';
    len++;

    print(1);
    printf("\t\t\t========= FINISH INPUT =========\n");

    return 0;
}


void check() {

    if (degenerate_() == 1)
        return;

    elf();
    int index = confirm();

    if (index == -1);

    else if (todo[index]->did == 'X') {

        todo[index]->did = 'O';

        printf(RED "\t\t   %s %s %c is COMPLETE!!!\n", todo[index]->date, todo[index]->todo, todo[index]->priority);
        printf(RESET);
        elf();
    }
    else
        todo[index]->did = 'X';

    print(1);
}


void elf() {

    if (degenerate_() == 1)
        return;

    int cnt = 0;
    int i;

    for (i = 0; i < len; i++) {
        if (todo[i]->did == 'O')
            cnt++;
    }

    if (cnt == len) {
        printf(YELLOW);
        printf("\t\t         ┌────────────────┐\n");
        printf("\t\t         │ Completion !!! │\n");
        printf("\t\t         │                │ =@[‘v’]@= ☆\n");
        printf("\t\t         └────────────────┘  0=|-|=b ☆ ★\n");
        printf(RESET);
    }
}


int confirm() {

    degenerate_();
    if (degenerate_() == 1)
        return 0;

    int ok, i;

    printf("\t\t  ┌──────────────────────────────────────────┐\n");
    printf("\t\t  │            * CHOISE INDEX *              │\n");

    for (i = 0; i < len; i++) {

        printf("\t\t  │(%d)", i + 1);
        printf(RED);
        printf(" %c", todo[i]->did);

        printf(RESET);
        printf(" %-10s %-20s %2c  │\n", todo[i]->date, todo[i]->todo, todo[i]->priority);
    }

    printf("\t\t  │(0) Back                                  │\n");
    printf("\t\t  └──────────────────────────────────────────┘\n");

    while (1) {
        printf("\t\t   PRESS INDEX :  ");
        scanf("%d", &ok);

        if ((ok - 1) >= len) {
            printf("\n\t\t      =========== PRESS AGAIN ! ===========\n\n");
            continue;
        }
        else break;
    }
    return ok - 1;

}


void modified() {

    if (degenerate_() == 1)
        return;

    char order;

    while (1) {
        printf("\n\t\t   ========= WHAT DO YOU WANT EDIT? ========\n");
        int index = confirm();

        if (index == -1) break;

        else {
            printf("\t\t    ┌──────────────────────────────────────┐\n");
            printf("\t\t    │        * WHAT DO YOU CHANGE *        │\n");
            printf("\t\t    │ 1. DATE                              │\n");
            printf("\t\t    │ 2. TODO                              │\n");
            printf("\t\t    │ 3. PRIORITY                          │\n");
            printf("\t\t    └──────────────────────────────────────┘\n");
            printf("\t\t     PRESS NUMBER : ");
            scanf(" %c", &order);

            if (order > '3') continue;      //잘못된 번호를 입력하면 수정 선택화면으로 돌아감

            printf("\t\t     HOW TO CHANGE : ");    //바꿀 문자 알아볼 출력 추가

            switch (order) {
            case '1': scanf(" %s", todo[index]->date); break;
            case '2': scanf(" %[^\n]", todo[index]->todo); break;
            case '3': scanf(" %s", &todo[index]->priority); break;
            default: printf("\n\n\t\t    ------- ERROR : INVALID INPUT -------\n"); break;
            }
            sort_date();        //날짜가 바뀌었을 경우 날짜순으로 정렬해놓는다
        }
        break;
    }
    print(1);
}

void search() {

    if (degenerate_() == 1)
        return;

    int input;
    int c = 0;

    printf("\t\t    ┌──────────────────────────────────────┐\n");
    printf("\t\t    │          * HOW TO SEARCH *           │\n                                  │");
    printf("\t\t    │ 1. DATE                              │\n                                  │");
    printf("\t\t    │ 2. TODO                              │\n                                  │");
    printf("\t\t    │ 3. PRIORITY                          │\n                                  │");
    printf("\t\t    └──────────────────────────────────────┘\n");


    while (1) {
        printf("\t\t     PRESS NUMBER : ");
        scanf("%d", &input);

        if (input == 1 || input == 2 || input == 3) 
            break;
        else {
            printf("\t\t      =========== PRESS AGAIN ! ===========\n");
            continue;
        }
    }

    if (input == 1) {
        int k = 0;
        char sd[30];
        char nsd[30];

        printf("\t\t     ENTER DATE : ");
        scanf(" %[^\n]", sd);

        for (int i = 0; i < sizeof(sd) / sizeof(char); i++) {
            if (!(sd[i] == ' '))
                nsd[k++] = sd[i];
        }

        printf("\n\t\t     >>>>'%s' SEARCH RESULTS\n\n", nsd);

        for (int i = 0; i < len; i++) {
            if (strstr(todo[i]->date, nsd) != NULL) {   // str1에 str2와 일치하는 문자열이 없으면 null반환
                printf(RED);
                c = printf("\t\t     %c ", todo[i]->did);       
                printf(RESET);

                printf("%-10s %-20s %-1c\n", todo[i]->date, todo[i]->todo, todo[i]->priority);
            }
            else
                ;
        }
        
        if (c == 0) // ? 잘 모르겠지만 검색 실수 했을 경우의 예외처리
            printf("\t\t     DOES NOT EXIST...\n");
    }

    else if (input == 2) {

        char st[50];
        char lst[50], cst[50];

        printf("\t\t     ENTER TODO : ");
        scanf(" %[^\n]", st);
        strcpy(lst, st);

        // **************************************************************************이거 왜한거야
        for (int i = 0; i < sizeof(st) / sizeof(char); i++) {
            if (st[i] >= 65 && st[i] <= 90)
                lst[i] = st[i] + 32;

            else if (st[i] >= 97 && st[i] <= 122)
                lst[i] = st[i];
        }

        for (int i = 0; i < sizeof(st) / sizeof(char); i++) {
            if (st[i] >= 65 && st[i] <= 90)
                lst[i] = st[i];

            else if (st[i] >= 97 && st[i] <= 122)
                lst[i] = st[i] - 32;
        }

        printf("\n\t\t     >>>>'%s' SEARCH RESULTS\n\n", st);

        for (int i = 0; i < len; i++) {

            if (strstr(todo[i]->todo, st) != NULL || strstr(todo[i]->todo, lst) != NULL || strstr(todo[i]->todo, cst) != NULL) {
                printf(RED);
                c = printf("\t\t     %c ", todo[i]->did);
                printf(RESET);

                printf("%-10s %-20s %-1c\n", todo[i]->date, todo[i]->todo, todo[i]->priority);
            }
            else
                ;
        }
        if (c == 0)
            printf("\t\t     DOES NOT EXIST...\n");

    }

    else if (input == 3) {
        char sp;
        printf("\t\t     ENTER PRIORITY : ");
        scanf(" %c", &sp);

        printf("\n\t\t     >>>>'%c' SEARCH RESULTS\n\n", sp);

        for (int i = 0; i < len; i++) {

            if (todo[i]->priority == sp) {
                printf(RED);
                c = printf("\t\t     %c ", todo[i]->did);
                printf(RESET);

                printf("%-10s %-20s %-1c\n", todo[i]->date, todo[i]->todo, todo[i]->priority);
            }
            else
                ;
        }
        if (c == 0)
            printf("\t\t     DOES NOT EXIST...\n");
    }
}


void sort() {
    TODO* temp = (TODO*)malloc(sizeof(TODO));

    for (int i = 0;i < len - 1;i++)        //버블정렬 사용
        for (int j = len - 1;j > i;j--)
            if (todo[j-1]->did > todo[j]->did)      //****************if (todo[j-1]->did > todo[j]->did) 버블정렬 이건데,,
                SWAP(todo[j - 1], todo[j], temp);
}


void sort_date() {
    TODO* temp = (TODO*)malloc(sizeof(TODO));

    for (int i = 0; i < len; i++)    //버블정렬 사용
        for (int j = 0; j < len - 1; j++)
            if (strcmp(todo[j]->date, todo[j + 1]->date) > 0) 
                SWAP(todo[j], todo[j + 1], temp);
    sort();
}


void sort_priority() {      //우선순위 순으로 정렬 안됨** 수정하기

    TODO* temp = (TODO*)malloc(sizeof(TODO));

    for (int i = 0; i < len; i++)
        for (int j = 0; j < len - 1; j++)
            if (todo[j]->priority > todo[j + 1]->priority)
                SWAP(todo[j], todo[j + 1], temp);

    sort();
}


void print_all() {

    if (degenerate_() == 1)
        return;

    int i;

    printf("\t\t   ┌──────────────────────────────────────────┐\n");
    printf("\t\t   │             * MY TODO LIST *             │\n");

    for (i = 0; i < len; i++) {
        printf("\t\t   │");
        printf(RED);
        printf("  %c", todo[i]->did);
        printf(RESET);

        printf(" %-10s %-20s %2c    │\n", todo[i]->date, todo[i]->todo, todo[i]->priority);
    }
    printf("\t\t   └──────────────────────────────────────────┘\n");
}


void print_main() {

    int input;

    printf("\t\t    ┌──────────────────────────────────────┐\n");
    printf("\t\t    │              * MENU *                │\n");
    printf("\t\t    │ 1. BY DATE                           │\n");
    printf("\t\t    │ 2. BY PRIORITY                       │\n");
    printf("\t\t    │ 3. CALENDAR                          │\n");
    printf("\t\t    └──────────────────────────────────────┘\n");

    while (1) {    //number를 잘못입력할 경우를 대비

        printf("\t\t     PRESS NUMBER : ");
        scanf("%d", &input);

        if (input > 3) {
            printf("\t\t    =========== PRESS AGAIN ! ===========\n");
            continue;
        }
        break;
    }
    print(input);
}


void print(int input) {

    switch (input) {
    case 1: sort_date(); print_all(); break;
    case 2: sort_priority(); print_all(); break;
    case 3: sort_date(); calendar(); break;
    default: break;
    }

}


void save() {

    sort_date();

    fp = fopen("todo.txt", "wt");    // 쓰기 모드로 파일 오픈
    for (int i = 0; i < len; i++)   // 라인 단위로 파일에 입력하기 위해 \n를 중간중간 넣음
        fprintf(fp, "%c\n%s\n%s\n%c\n", todo[i]->did, todo[i]->date, todo[i]->todo, todo[i]->priority);

    fclose(fp);

    printf("\n\t\t\t========== FILE SAVED! =========\n");
}


void delete_() {

    int i;

    if (degenerate_() == 1)
        return;

    printf("\n\t\t   ======== WHAT DO YOU WANT DELETE? ========\n");

    for (i = confirm(); i < len; i++) {
        if (i == -1) 
            break;     //i에 back이 입력되었다면 곧바로 finish

        todo[i] = todo[i + 1];
    }

    if (i != -1) 
        len--;         //Back키를 누른 게 아니라면 전체 len의 길이를 감소시킴

    print(1);

    printf("\n\t\t\t========= FINISH DELETE =========\n");
}


int open(FILE* fp) {

    int k;

    fp = fopen("todo.txt", "r+t");         
    if (fp == NULL) return 0;

    todo[len] = (TODO*)malloc(sizeof(TODO));        

    while (0 < fscanf(fp, "%c\n%s\n%[^\n]\n%c\n", &todo[len]->did, todo[len]->date, todo[len]->todo, &todo[len]->priority)) //fscanf 리턴값 : 성공 시 변환 성공 개수, 오류 시 EOF.
        todo[++len] = (TODO*)malloc(sizeof(TODO));     

    k = fclose(fp);
    if (k != 0) return 0;        // fclose 리턴값 : 성공 시 0, 실패 시 EOF

    return 0;
}


//======================= 달력 관련 함수 =========================//


const char mon[13][10] = { "January","February","March","April","May","June","July","August",   // const 배열을 사용해 값을 수정할 수 없도록 함
"September","October","November","December" };


int* myatoi(int y, int m) {     //y랑 m이랑 같은 구조체 배열이 있다면 day만 추출해서 배열을 만든 후 반환

    char aday[20], tmp[5], tm[3];
    char ay[5], am[3];          //int형 변수를 string으로 만들어 저장할 변수

    static int iday[31];
    int iy = y, im = m;         //int형 변수의 본체 변형을 막기 위한 임시 변수
    int i, j = 0;

    sprintf(ay, "%d", iy);      //itoa는 비쥬얼에만 있는 함수라서 문자열로 변형해주는 sprintf 사용
    sprintf(am, "%d", im);      //am이 가리키는 배열에 쓰는건데 sprintf함수는 자동적으로 str 맨마지막에 NULL문자를 붙이기 때문에 한칸의 여유를 두어야 한다.

    for (i = 0; i < len; i++) {
        if (todo[i]->did != 'O') strcpy(aday, todo[i]->date);//O인 일정만 달력에 표시
        strncpy(tm, aday + 4, 2);   //달과 일,년도를 strstr로 구분하기 어려우므로 tm에 aday만을 저장

        if (strstr(aday, ay) != NULL && strstr(tm, am) != NULL) {
            strncpy(tmp, aday + 6, 2);

            iday[j++] = atoi(tmp);
        }
    }

    if (i == len) iday[j] = 0;  //calendar를 실행시 전에 있던 iday 배열을 초기화 하기위해 존재
                                //배열의 마지막 인덱스(len)가 0이 되며 오름차순 배열의 null값 역할
                                //calender를 여러번 실행시키는 경우나 마지막 원소 delete후 실행시키는 경우 유용
    return iday;
}


void calendar()
{
    int year;   //년
    int month;  //월
    int day;    //해당월 1일의 요일
    int* itodo; //todo가 있는 날들이 모인 int형 포인터. 배열을 반환받아야 해서 int*타입

    do {
        printf("\t\t     ====== INPUT DATE YOU WANT SEE ======\n");
        printf("\t\t      Year: ");
        scanf("%d", &year);
        printf("\t\t      Month: ");
        scanf("%d", &month);       

        if (month >= 1 && month <= 12)
            break;
        else
            printf("\t\t     ERROR : THIS ISN'T DAY.\n ");

    } while (1);

    itodo = myatoi(year, month);    //itodo 배열에 원하는 년도와 달에 일정이 있는 날짜 저장
    day = day_of_week(year, month); //해당월 시작요일 구하기(%7을 해줌으로써 알 수 있다)
    print_head(year, month);        //달력 상단부 출력    
    print_calendar(day, year, month, itodo);    //달력 날짜 출력       
}


int day_of_week(int year, int month) {  //해당 월 1일이 무슨요일인지 알기위해 총 일수를 구하는 함수

    int temp = 0;  
    int i;                 

    for (i = 1;i < year;i++) {  //년도별 일수
        if ((i % 4 == 0) && (i % 100 != 0) || (i % 400 == 0))
            temp += 366;
        else
            temp += 365;
    }

    for (i = 1;i < month;i++) { //매 달 일수
        if (i == 2) {     //2월일 경우 윤년 검사
            if ((year % 4 == 0) && (year % 100 != 0) || (year % 400 == 0))
                temp += 29;
            else
                temp += 28;
        }

        switch (i) {

        case 1: case 3: case 5: case 7: case 8: case 10: case 12:
            temp += 31;     //한달이 31일인 경우
            break;

        case 4: case 6: case 9: case 11:
            temp += 30;     //한달이 30일인 경우
            break;
        }
    }
    temp = temp + 1;     //마지막으로 일수를 더해 총 일 수를 구한다
    return temp % 7;     //1=월,2=화...6=토,0=일
}


void print_calendar(int sd, int year, int month, int todo[]) {  //sd는 start day 무슨 요일에 시작하는 지를 의미

    int i, j, k = 0;
    int temp;           //해달 월의 총 일수

    switch (month) {    //한달의 일수를 구한다

    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        temp = 31;     //한달이 31일인 경우
        break;

    case 4: case 6: case 9: case 11:
        temp = 30;     //한달이 30일인 경우
        break;

    case 2:         //2월 윤년 판정
        if ((year % 4 == 0) && (year % 100 != 0) || (year % 400 == 0))
            temp = 29;  //2월 29일 윤년
        else
            temp = 28;  //2월 28일 평년
    }

    printf("\t\t\t");

    for (i = 1;i <= sd;i++)     //1일에 해당하는 요일이 나올 때까지 빈칸 프린트
        printf("     ");

    j = sd;

    for (i = 1;i <= temp;i++) {
        if (j == 7) {   //요일별로 출력되도록 줄바꿈
            printf("\n\t\t\t");
            j = 0;      //일주일이 끝나면 j를 0으로 초기화
        }

        if (j == 0) printf(RED);    //일요일은 빨간색으로 출력
        else if (j == 6) printf(BLUE);  //토요일은 파란색으로 출력

        if (i == todo[k]) {    //todo가 있는 날짜는 *로 출력됨    //** 여기 모르겠어, 메뉴에서 print > 달력 출력 > 년 입력 : 2020 > 월 입력 : 8 >> 20200810과 20208011을 같은 8월로 인식함 수정하기**
            printf("%2s   ", "*");
            while (i + 1 > todo[k]) k++;
        }

        else printf("%2d   ", i);
        j++;

        printf(RESET);      //달력 날짜가 초기화 되면 색을 RESET해줌
    }
    printf("\n\n");
}


void print_head(int year, int month) {

    printf("\n\t\t\t\t【 %d / %s 】\n\n", year, mon[month - 1]);
    printf(RED "\t\t        Sun");  //일요일을 빨간색으로 출력
    printf(RESET "  Mon  Tue  Wen  Thu  Fri  ");    

    printf(BLUE "SAT\n");   //토요일을 파란색으로 출력
    printf(RESET);

}
