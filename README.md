# C-programming

# **Heap ADT**
Queue에서는 어떤 data가 들어오면 순서대로 처리된다. 즉, 줄서기와 같이 data가 들어온 순서대로 정렬되고 빠져나갈 때도 먼저 들어온 data순으로 처리된다. 하지만 Queue에서는 우리가 원하는 data를 다른 특정 data보다 우선적으로 정렬하게 할 순 없다. 이 때 heap구조를 이용하여 우리가 원하는 data에 우선순위를 부여한다면, 그 data의 위치를 이미 정렬된 다른 data들 보다 우선적으로(순서대로가 아닌) 배치할 수 있다. 지금부터 볼 priority heap구조는 고객정보를 다루는알고리즘이다. 
예를 들어 Time sale 기간에 고객들이 줄을 섰다고 가정하자. 일반적인 Queue 구조에서는 줄을 선 고객부터 차례대로 입장할 것이다. 하지만 만약 VIP인 고객이라면 줄을 늦게 서더라도 우선순위를 부여해야할 것이다. 따라서 heap구조를 이용해 고객정보에 우선순위크기를 부여하여 줄을 선 순서와 상관없이 우선순위가 큰 사람부터 입장할 수 있도록 만들것이다. 만약 우선순위가 같은 사람이 줄을 섰다면, 줄을 선 순서대로 입장하게 된다.
# <heap.h> 부터 살펴보자
# HEAP 구조체 선언
→heap 구조체 멤버로 heapAry, last, size,compare함수, maxSize를 선언한다.
# HEAP* heapCreate(int maxSize, int(*compare)(void* argu1, void* argu2))
→동적 메모리 영역에 HEAP구조체 포인터를 할당한다.
→heap 구조체를 초기화하고 heapAry에는 calloc을 사용하여 할당과 동시에 초기화를 한다. heap->maxSize는 할당공간의 개수를 의미하고 sizeof(void*)는 할당 공간의 크기를 의미한다.
# void reheapUp(HEAP* heap, int childLoc)
→reheapUp함수의 매개변수로 선언된 childLoc이 만약 제 값이 있다면 if문을 실행하게 된다. 이때 이중포인터인 heapAry는 구조체포인터 HEAP멤버의 heapAry와 같다.
→parent는 childnode의 윗단계인 node를 의미한다. Parent에 (childLoc-1)/2한 주소값을 준다.
→heapAry[childLoc]:childLoc이 배열의 위치를 의미하므로 heapAry[childLoc]은 그 위치의 data값이다.
마찬가지로 heapAry[parent] int형 parent의 배열 위치에 해당하는 data값을 의미한다.
→compare함수를 통해 두 data를 비교했을 때 childLoc에 저장된 data값이 더 크다면(즉,childnode값이 더 크면) 현재 parentnode 값은 저장하고 parentnode 자리에 childenode값을 넣어주어 두 data의 위치를 바꾼다.
→이렇게 함으로써 첫번째 reheapUp이 이루어지고 재귀함수 구조를 통해서 처음 childLoc에 있던 data값은 다른 data들과의 크기비교를 통해 heap 구조에서 자기자리에 맞는 위치에 배열될 것이다.
# void reheapDown(HEAP* heap, int root)
→last는 int형 변수로 배열의 마지막 위치를 나타낸다.
→leftData = heap->heapAry[root * 2 + 1] : 만약 root*2+1의 위치가 배열의 마지막 값보다 같거나 작으면 leftData는 배열의 root*2+1위치에 있는 값으로 저장된다.
→rightData = heap->heapAry[root * 2 + 2] : 만약 root*2+2의 위치가 배열의 마지막 값보다 같거나 작으면 rightData는 배열의 root*2+2의 위치에 있는 값으로 저장된다.
→if ((!rightData) || heap->compare(leftData, rightData) > 0) : compare함수는 통해 아까전에 저장했던 leftData값과 rightData값을 비교했을 때 leftData가 더크면 1을 반환하고 rightData가 더 크면 -1을 반환한다. 지금 if 문에서는 만약 leftData가 더 크다면을 가정한다.
→만약 leftData값이 더 크면 largeLoc에는 root*2+1의 배열 위치값을 저장하고 rightData값이 더크면 largeLoc에는 root*2+2의 배열 위치값을 저장한다.
# bool heapDelete(HEAP* heap, void** dataOutPtr)
→보통 heap에서의 삭제는 root노드를 삭제한다. Root를 삭제하고 나면 두 subtree가 연결이 안된 상태이므로 배열의 가장 마지막을 root로 이동시키고 아까 전의 reheapDown 구조를 통해서 다시 완성된 heap구조를 만들어준다.
# <HeapTest.c>
# CUSTOMER구조체 선언과 사용될 함수들
→ CUSTOMER구조체 멤버로 고객 이름(name)과 부여될 우선순위(priority, 값이 클수록 우선순위가 높다.), 그리고 우선순위로부터 계산될 point를 선언하였다.(우선순위가 높은 사람일수록 point가 높다.)
# main함수
→ HEAP구조체 포인터인 prQueue를 선언하고 동적메모리 영역에서 할당받는다. 실제 이 프로그램은 processPQ함수를 통해 실행된다!
# int compareCus(void* cus1, void* cus2)
→compare함수를 통해 두 매개변수 값을 비교해보자.
→ CUSTOMER 구조체인 c1과 c2를 선언한다.
→매개변수로 받은 void포인터형 cus1과 cus2를 CUSTOMER구조체 포인터형으로 형변환을 하고 그게 가리키는 값이 구조체 c1,c2가 되도록 c1,c2를 초기화한다.
→만약 c1구조체의 point멤버 값이 c2구조체의 point멤버값보다 작으면 -1, 같으면 0, 크면 1을 반환하도록 함수를 설정한다.
# void processPQ(HEAP* prQueue)
-option이 ‘e’인 경우
→ 이제 priority heap구조의 작동중심이라 할 수 있는 processPQ함수를 분석해보자!
→ 조금 있다가 볼 menu()함수를 실행하고 난 return값으로 option변수를 초기화한다. 만약 option이 ‘e’였다면 getCus()함수를 실행한다. getCus()함수는 새로 삽입할 고객의 이름과 우선순위를 입력받고 customer 구조체에 그 값을 저장하여 그 구조체를 반환하는 함수이다.
→ getCus()함수로부터 새로운 정보가 입력된 customer구조체를 생성하였다. 따라서 numCusomer의 값을 1증가시켜주고 입력받은 priority값으로부터 point값을 계산하여 저장한다.( customer->point = customer->priority * 1000 + (1000 - numCustomer)) 옆의 계산식을 보면 priority가 클수록 point가 크다는 것을 알 수 있다. 또한 만약 priority값이 같을 경우 numCustomer에 따라 point값이 달라진다. 즉 우선순위가 같을 경우 먼저 줄을 선 사람이 더 큰 point값을 가진다.
→ 정보가 잘 저장되었는지 확인하기 위해서 저장된 정보를 출력하는 printf()함수를 써보았다.
→ 고객이름(name), 우선순위(priority), point가 저장된 customer구조체를 heap에 삽입해보자. heapInsert(HEAP* heap, void* dataPtr)함수를 통해 prQueue HEAP구조체 포인터에 customer 구조체 포인터가 저장된다. 즉 heap->heapAry[heap->last]=dataPtr 문장을 통해 크기가 20인 heapAry배열의 마지막에 cutomer구조체가 삽입되고, reheapUp을 통하여 point가 큰 고객부터 heap구조로 배열이 정렬된다.
→즉 어떤 고객이 먼저 왔더라도(먼저 삽입됐더라도) point가 큰 고객이 먼저 입장하게 된다. 왜냐하면 reheapUp을 실행할 때 compareCus가 실행되는데, 이때 point값으로 배열들을 비교하기 때문이다.
→만약 heapInsert가 제대로 안이루어져 false를 반환했다면, 에러가 났다는 메시지와 함께 eixt(101)로 코드를 종료한다.

-option이 ‘d’인 경우
→option값이 ‘d’이면 heapDelete함수가 실행된다. point크기순으로 정렬된 heapAry에서 root값이 (point 값이 가장큰) 삭제된다.
→delete가 제대로 이루어지지 않으면, 에러메시지를 출력한다.
→만약 delete가 제대로 실행됐다면, 삭제된 customer 이름을 출력하면서 삭제가 잘 되었는지 확인한다. 그런 후 numCustomer수를 1감소시킨다.
→option이 ‘q’이면 do-while반복문을 종료한다.

# char menu()
→menu()함수는 return 값으로 opiton을 반환한다. option값이 ‘e’, ‘d’, ‘q’이면 valid 값으로 true를 반환하고 이외의 다른 것 문자이면 다시 입력하라는 메시지와 함께 valid는 false로 반환한다. Do-while 반복문은 while(!valid) 이므로 valid가 true이면 반복문이 종료된다.

#  CUSTOMER* getCus()
→getCus()는 processPQ에서 option값이 ‘e’일 시 customer구조체에 고객정보를 입력받기 위한 함수이다.
→CUSTOMER 구조체 포인터를 동적할당 받고 만약 동적할당이 이루어지지 않았으면 overflow가 일어났다는 메시지를 출력한다. 그리고 eixt(200)으로 알고리즘을 종료한다.
→동적할당이 제대로 이루어졌으면 scanf함수를 통해 고객 이름과 우선순위 정보를 입력받고 customer 포인터 구조체를 반환한다.

# STACK


1)STACK_NODE* bottomStack;

→구조체 멤버로 void 포인터 dataPtr 과 자기참조구조체포인터 link 를 가지는 bottomStack
구조체포인터를 선언한다.

2) if(stack->count == 0)
return NULL;

→stack 이 참조하는 count 변수의 int 값이 0 이면 bottom 함수는 NULL 을 반환한다.

3) else if (stack->count == 1)
return stack->top->dataPtr;

→stack 이 참조하는 count 변수의 int 값이 1 이면 stack 이 참조하는 top(즉 첫번째 node)의
dataPtr(void 형 구조체)을 반환한다.

4) else{
for (int i = 1;i < stack->count;i++) {
bottomStack = stack->top->link;
stack->top = stack->top->link;
}
return bottomStack->dataPtr;
}

→count 값이 0 또는 1 이 아니면 위의 반복문을 시행한다. i 가 1 에서부터 count 값보다 1 작은 수까지
반복문이 실행된다. 

5) return bottomStack->dataPtr;

→반복문을 끝내고 난 후 bottomStack 이 참조하는 node 의 dataPtr 을 반환한다.(즉, 스택의 맨
아랫값을 반환한다.)

# QUEUE
# circular queue를 이용한 피보나치 수열의 실행

배열 기반 큐는 배열이 분명히 비어 있는 상태임에도 불구하고 rear포인터를 배열의 끝에서 더 이상 오른쪽으로 이동시킬 수 없기 때문에 데이터를 추가할 수 없다. 이러한 문제점을 해결하기 위해 배열의 머리와 끝을 연결한 circular(원형)구조의 queue를 사용한다. 
→원형 큐에서는 Empty상태와 Full상태를 구별하기 위해 저장공간 하나를 잃는다. 이러한 성질갖는 원형 큐를 이용하여 피보나치 수열을 저장하고(Enqueue) 반환하는(Dequeue) 코드를 살펴보자!

# circularqueueADT.h
→먼저 circularqueueADT.h에 원형 큐를 실행하기 위한 함수들을 저장해 놓았다. 위의 함수들 중에서 NextPosldx에 의해 원형 큐의 전체적인 틀이 완성된다. 첫번째 if문이 의미하는 것은 만약 현재의 위치(pos)가 100-1과 같으면, 즉 현재 위치가 큐배열의 마지막이라면 0을 반환하고 그렇지 않으면 현재의 위치에서 오른쪽으로 한 칸 이동한 값을 반환한다. NextPosldx함수에 의해 배열의 마지막은 비워두게 되는 원형 queue가 되는 것이다.

# 피보나치 수열 저장하고 반환하기
→QUEUE구조체 변수 q를 생성하고 QueueInit함수를 통해 q의 front와 rear를 0의 값으로 초기화한다.
→위의 예시는 for반복문을 거치면 피보나치 수열의 1항부터 19항까지의 수들이 Enqueue를 통해 원형 큐에 값이 저장된다. 여기서 주의해야 할 점은 배열의 크기가 100이므로 저장공간을 하나 잃은 99개의 데이터만이 저장될 수 있다. 따라서 i가 1부터 시작하였으므로 100보다 크면 오류가 발생한다. 만약 for문의 범위가 (int i=1; i<100; i++)이었다면 크기가 100인 원형 큐 배열에서 저장공간을 하나 잃고 가장 많은 데이터(99개)가 저장된다.
→Enqueue를 통해 피보나치 수열 데이터를 원형 큐에 저장하였다. 이제 Dequeue를 이용하여 원형 큐가 Empty가 아닐 때까지 데이터를 반환한다.
→system(“PAUSE”);는 visual studio 2017버전을 깔았더니 cmd창이 바로 꺼져서 그것을 방지하기 위해 적어주었다!

# General linear list ADT
General linear list를 이용하여 매장고객의 고객등록날짜, 이름, 고객등급을 알려주는 프로그램을 작성하였다. 위의 매장고객 정보들을 CUSTOMER구조체의 멤버로 작성하고 buildList함수를 통해 만든 List의 NODE의 dataPtr부가 그 구조체를 참조하게 된다. List가 생성이 되었다면 process함수를 통해 choice에 따라 search함수와 printfList함수, insertCustomer함수가 실행된다. 따라서 미리 고객들의 정보가 저장된 메모장 파일에서 내가 원하는 고객의 정보만(search함수) 얻을 수도 있고 또는 매장전체 고객의 정보(printList함수)를 한번에 얻을 수도 있는 코드를 작성하였다. 또한 추가할 고객정보를 입력하여 효율적인 매장고객 관리 리스트를 작성하였다.

# buildLIst()
→createList를 통하여 list를 생성한다. 밑의 if문을 통해 list가 정상적으로 생성되었는지 확인한다.
→헤더파일 <stdio.h>에 미리 저장돼있는 구조체 유형인 FILE*로부터 fpData변수를 생성하고 “customers.txt”파일을 열고 읽기모드로 한다. 마찬가지로 if문을 통해 fpData가 정상적으로 생성되었는지 확인한다. 
→fscanf를 통해 “customers.txt”파일의 수를 성공적으로 읽어 왔다면, 동적할당을 하여 CUSTOMER 구조체 포인 터변수(pCus)를 생성한다. 마찬가지로 if문을 통해 동적 할당이 제대로 이루어졌는지 확인한다.
→pCus의 regDate에 아까 fscanf를 통하여 읽은 regDateIn을 삽입한다.
→fgetc를 통해 fpData에서 문자 1개를 읽어온다. 만약 ‘ ’(띄어쓰기)이면 while문을 통과하고 ‘ ” ’(큰따옴표)이면 while문을 통과한다. 따라서 “customers.txt”파일의 커서는 고객이름 앞에 위치하게 될 것이다.
→fscanf를 통해 fpData의 문자를 읽어오고 pCus의 name에 저장한다. 마찬가지로 위와 동일한 while문을 반복하여 고객등급을 pCus의 customerRating에 저장한다.
→addNode함수를 통하여 NODE의 dataPtr부가 구조체 포인터변수(즉 pCus)를 참조하는 NODE를 추가한다. 결국 새로 생성된 NODE는 그 NODE의 dataPtr부가 고객1의 정보를 저장하고 있는 형태일 것이다.
→fpData로부터 문자 1개를 읽어왔을 시 그 문자가 엔터이면 위의 while문을 반복하고 또다시 새로운 NODE가 만들어지며 고객2,고객
3….의 정보를 저장 할 것이다.

# Process(List* list)
→getChoice함수를 통해 choice를 받는다.(choice는 P,S,Q중
에 하나로 getChoice함수는 뒤에서 살펴보자!)
→choice=p: printList함수가 실행되어 “customers.txt”에 저
장되어 있는 모든 고객정보를 불러온다.(printList에서는
traverse함수 이용!)
→choice=S: search함수가 실행되어 날짜8자리수를 입력 받
고 만약 그 날짜가 텍스트파일 안에 존재하면 구조체 pCus
에 저장된 원하는 고객 정보(고객등록날짜, 고객이름, 고객
등급)를 불러온다
→choice=I: insertCustomers함수가 실행되어 고객정보저장
날짜와 고객이름, 고객등급을 입력 받은 다음 fprintf를 사
용하여 “customers.txt”에 세 정보를 저장한다. 즉 새로운
고객정보를 삽입한다.
→choice=Q: do-while문이 종료된다.

# getChoice()
→사용자로부터 문자 하나를 입력 받아 choice
에 저장한다(scanf ). ClearLineFromReadBuffer함
수를 사용한 이유는 예를들어 choice로 P를 입
력하고 엔터를 쳤을 때 printList함수가 실행되
고나서 자꾸 “Invalid choice, Please try again”문
장이 떴기 때문이다. 즉 엔터도 문자로 읽어서
while문장이 실행되고, 엔터가 default값이므로
오류문구가 뜬 것이다. 따라서
ClearLineFromReadBuffer함수를 통해 엔터를 받
을 때까지 입력버퍼에 남아있는 값들을 읽어주
게 만들어서 정상적으로 코드가 실행되게 하였
다.

# Main()실행문
→printfInstr를 통해 이 프로그래밍의 목적을 설명한다.
→buildList를 통해 “customers.txt”파일로부터 읽은 고객1,고객2,고객3,…..의 정보를 담고있는
list를 생성한다.
→process를 통해 사용자로부터 choice문자 하나를 입력 받고 search함수, printfList함수 또는
insertCustomer함수를 실행하여 고객의 정보를 출력한다.
