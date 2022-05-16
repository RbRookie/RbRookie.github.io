---
title: 数据结构
date: 2022-5-12 20:04:17
categories: 数据结构
tags:
  - 数据结构
  - Java 
---

#  Java数据结构

<!-- more -->

## 稀疏数组和队列

#### 稀疏数组

稀疏数组： 用来压缩数据用的，有些数组很多默认值是0，所以稀疏数组

数组处理方法：

1）记录数组一共有几行几列，多少个不同的值，

2）把具有不同数组的元素的行列以及值记录在一个小规模的数组中，从而压缩数据

##### 图示：

![image-20210330200935239](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image-20210330200935239.4seatkewbwo0.png)

## 队列

##### 定义

- 队列是一个**有序列表**，可以用**数组**或是**链表**来实现。
- 遵循**先入先出**的原则。即：先存入队列的数据，要先取出。后存入的要后取出

##### 数组队列模拟思路

- 队列本身是有序列表，若使用数组的结构来存储队列的数据，则队列数组的声明如下图, 其中 maxSize 是该队列的最大容量
- 因为队列的输出、输入是分别从前后端来处理，因此需要两个变量 front及 rear分别记录队列前后端的下标，front 会随着数据输出而改变，而 rear则是随着数据输入而改变。如图所示。

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.2ufe76vz0uo0.png)

##### 入队出队操作

当我们将数据存入队列时称为”addQueue”，addQueue 的处理需要有两个步骤：

- 将尾指针往后移：rear+1 , 当 **front == rear** 时，队列为空
- 若尾指针 rear 小于队列的最大下标 maxSize-1，则将数据存入 rear所指的数组元素中，否则无法存入数据。**rear == maxSize - 1**时，队列满

**注意**：front指向的是队列首元素的**前一个位置**  ,而且需要注意的事情，将队列做成环形队列可以避免数组队列只能使用一次的情况。

##### 环形队列思路

思路如下:

1.  front 变量的含义做一个调整： front 就指向队列的第一个元素, 也就是说 arr[front] 就是队列的第一个元素
2.  front 的初始值 = 0
3.  rear 变量的含义做一个调整：rear 指向队列的最后一个元素的后一个位置. 因为希望空出一个空间做为约定.
4.  rear 的初始值 = 0
5.  当队列满时，条件是 **(rear + 1) % maxSize == front** 【满】
6.  对队列为空的条件， rear == front 空
7.  当我们这样分析， 队列中有效的数据的个数  **(rear + maxSize - front) % maxSize  // rear = 1 front = 0** 
8.  我们就可以在原来的队列上修改得到，一个环形队列

##### 实现代码

```java
// 数组队列实现demo
// 一次性队列
public class ArraryQueue {
    public static void main(String[] args) {
        Queue queue = new Queue(3);
        char key = ' ';
        Scanner scanner = new Scanner(System.in);
        boolean loop = true;
        while (loop){
            System.out.println("------------请输入需要的操作------------");
            System.out.println("s(show): 显示队列");
            System.out.println("e(exit): 退出程序");
            System.out.println("a(add): 添加数据到队列");
            System.out.println("g(get): 从队列取出数据");
            System.out.println("h(head): 查看队列头的数据");
            key = scanner.next().charAt(0);
            switch (key) {
                case 's':
                    queue.showQueue();
                    break;
                case 'a':
                    System.out.println("请添加一个数字");
                    int n = scanner.nextInt();
                    queue.addQueue(n);
                    break;
                case 'g':
                    try {
                        int res = queue.getQueue();
                        System.out.printf("取出的数据时%d\n", res);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try {
                        int res = queue.headQueue();
                        System.out.printf("取出的头数据是%d", res);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    scanner.close();
                    loop = false;
                default:
                    break;
                }
            }
        System.out.println("程序已经退出~~");
    }
}

class Queue{
    private int maxSize;
    private int front;
    private int rear;
    private int[] arr;


    public Queue(int arrMaxSize){
        maxSize = arrMaxSize;
        arr = new int[maxSize];
        front = -1;
        rear = -1;
    }

    public boolean isFull(){
        return rear == maxSize-1;
    }

    public boolean isEmpty(){
        return rear == front;
    }

    public void addQueue(int n){
        if(isFull()){
            System.out.println("队列满，不能假数据");
            return;
        }
        arr[++rear] = n;
    }

    public int getQueue(){
        if(isEmpty()){
            throw new RuntimeException("队列空，不能取数据！！！");
        }
        return arr[++front];
    }

    public void showQueue(){
        if(isEmpty()){
            System.out.println("队里空的");
            return;
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.printf("arr[%d]=%d\n", i, arr[i]);
        }
    }
    // 显示队列的头数据
    public int headQueue(){
        if (isEmpty()){
            throw new RuntimeException("队列空的，没有数据");
        }
        return arr[front + 1];
    }
}


//循环队列类的实现demo
class CircleArray {
	private int maxSize; // 表示数组的最大容量
	//front 变量的含义做一个调整： front 就指向队列的第一个元素s, 也就是说 arr[front] 就是队列的第一个元素 
	//front 的初始值 = 0
	private int front; 
	//rear 变量的含义做一个调整：rear 指向队列的最后一个元素的后一个位置. 因为希望空出一个空间做为约定.
	//rear 的初始值 = 0
	private int rear; // 队列尾
	private int[] arr; // 该数据用于存放数据, 模拟队列
	
	public CircleArray(int arrMaxSize) {
		maxSize = arrMaxSize;
		arr = new int[maxSize];
	}
	
	// 判断队列是否满
	public boolean isFull() {
		return (rear  + 1) % maxSize == front;
	}
	
	// 判断队列是否为空
	public boolean isEmpty() {
		return rear == front;
	}
	
	// 添加数据到队列
	public void addQueue(int n) {
		// 判断队列是否满
		if (isFull()) {
			System.out.println("队列满，不能加入数据~");
			return;
		}
		//直接将数据加入
		arr[rear] = n;
		//将 rear 后移, 这里必须考虑取模
		rear = (rear + 1) % maxSize;
	}
	
	// 获取队列的数据, 出队列
	public int getQueue() {
		// 判断队列是否空
		if (isEmpty()) {
			// 通过抛出异常
			throw new RuntimeException("队列空，不能取数据");
		}
		// 这里需要分析出 front是指向队列的第一个元素
		// 1. 先把 front 对应的值保留到一个临时变量
		// 2. 将 front 后移, 考虑取模
		// 3. 将临时保存的变量返回
		int value = arr[front];
		front = (front + 1) % maxSize;
		return value;

	}
	
	// 显示队列的所有数据
	public void showQueue() {
		// 遍历
		if (isEmpty()) {
			System.out.println("队列空的，没有数据~~");
			return;
		}
		// 思路：从front开始遍历，遍历多少个元素
		// 动脑筋
		for (int i = front; i < front + size() ; i++) {
			System.out.printf("arr[%d]=%d\n", i % maxSize, arr[i % maxSize]);
		}
	}
	
	// 求出当前队列有效数据的个数
	public int size() {
		// rear = 2
		// front = 1
		// maxSize = 3 
		return (rear + maxSize - front) % maxSize;   
	}
	
	// 显示队列的头数据， 注意不是取出数据
	public int headQueue() {
		// 判断
		if (isEmpty()) {
			throw new RuntimeException("队列空的，没有数据~~");
		}
		return arr[front];
	}
}
```

## 链表

#### 1、单向链表

链表在内存中存储

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.36ev3d46g7c0.png)

**特点**

- 链表是以节点的方式来存储,**是链式存储**
- 每个节点包含 data 域 和 next 域。next域用来指向下一个节点
- 链表的各个节点不一定是连续存储的
- 链表分**带头节点的链表**和**没有头节点的链表**，根据实际的需求来确定

带头结点的**逻辑示意图**

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.6u4hv727ux80.png)

#### 实现思路

**创建（添加）**

- 先创建一个Head头节点，表示单链表的头
- 后面我们每添加一个节点，就放在链表的最后

**遍历**

- 通过一个辅助变量，来遍历整个链表

**有序插入**

- 先遍历链表，找到应该插入的位置
- 要插入的节点的next指向插入位置的后一个节点
- 插入位置的前一个节点的next指向要插入节点
  - 插入前要判断是否在队尾插入

**根据某个属性节点修改值**

- 先遍历节点，找到修改的位置
  - 如果未找到修改节点，则不修改

**删除某个节点**

- 先遍历节点，找到要删除节点的前一个节点
- 进行删除操作

**求倒数第n个节点的信息**

- 遍历链表，求出链表的**有效长度**length（不算头结点）
- 遍历链表到第length-n的节点

**翻转链表**

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.2p7tjdu9buw0.png)

- 创建一个新的头结点，作为新链表的头

- 从头遍历旧链表，将遍历到的节点插入新链表的头结点之后

- 注意需要用到

  两个暂存节点

  - 一个用来保存正在遍历的节点

  - 一个用来保存正在遍历节点的下一个节点

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.60mgor41yus0.png)

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.17ni1vuwezng.png)

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.3gow3asbbps0.png)

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.4stgpeuirss0.png)

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.51b70fxfj7k0.png)

**逆序打印**

- 遍历链表，将遍历到的节点入栈
- 遍历完后，进行出栈操作，同时打印出栈元素

#### 代码实现

```java
package link;

import java.util.Stack;

/**
 * @author RookieRb
 */

public class SingleLinkTest {
    public static void main(String[] args) {
        LinkedList linkedList = new LinkedList();
        linkedList.travel();
        System.out.println();
        //创建学生节点;
        Student node1 = new Student(1, "wanghui");
        Student node3 = new Student(3, "liuchengcheng");
        linkedList.addNode(node1);
        linkedList.addNode(node3);
        linkedList.travel();
        System.out.println();

        //
        System.out.println("有序插入");
        Student node2 = new Student(2, "helllo");
        linkedList.addByOrder(node2);
        linkedList.travel();
        System.out.println();

        //
        System.out.println("修改学生信息");
        node2 = new Student(2,"anhui");
        linkedList.changeNode(node2);
        linkedList.travel();
        System.out.println();

        //
        System.out.println("删除学生信息");
        linkedList.delateNode(node2);
        linkedList.travel();
        System.out.println();

        //
        System.out.println("获得倒数节点");
        System.out.println(linkedList.getNode(2));
        System.out.println();

        //
        System.out.println("翻转链表");
        LinkedList newLinkedList = linkedList.reverselist();
        newLinkedList.reverseTraverse();
        System.out.println();

        //
        System.out.println("倒序遍历链表");
        newLinkedList.reverseTraverse();

    }

}

class LinkedList{
    private Student head = new Student(0,"");

    /**
     * 按照顺序添加链表
     * @param node
     */
    public void addNode(Student node){
        Student temp = head;
        while (true){
            if (temp.next == null){
                break;
            }
            temp = temp.next;
        }
        temp.next = node;
    }

    /**
     * 遍历链表
     */
    public void travel(){
        if(head.next == null){
            System.out.println("链表为空");
        }
        Student temp = head;
        while(true){
            if (temp == null){
                break;
            }
            System.out.println(temp);
            temp = temp.next;
        }
    }

    /**
     * 按照排序方式
     * @param node
     */
    public void addByOrder(Student node){
        Student temp = head;
        while (temp.next != null && temp.next.id < node.id){
            temp = temp.next;
        }
        node.next = temp.next;
        temp.next = node;
    }
    /**
     * 修改节点
     */
    public void changeNode(Student node){
        if (head ==null){
            System.out.println("链表为空，请添加链表");
            return;
        }
        Student temp = head;
        while (temp.next != null && temp.id != node.id){
            temp = temp.next;
        }
        if (temp.id != node.id){
            System.out.println("没找到");
            return;
        }
        temp.name = node.name;
        return;
    }
/**
 * 删除节点
 */
    public void delateNode(Student node){
        if (head ==null){
            System.out.println("链表为空，请添加链表");
            return;
        }
        Student temp = head;
        while (temp.next != null && temp.next.id != node.id){
            temp = temp.next;
        }
        if(temp.next.id !=node.id){
            System.out.println("没找到");
            return;
        }
        temp.next = temp.next.next;
    }
    public Student getNode(int index){
        if(head.next == null) {
            System.out.println("链表为空!");
        }
        Student temp = head.next;
        //用户记录链表长度，因为head.next不为空，此时已经有一个节点了
        //所以length初始化为1
        int length = 1;
        while(temp.next != null) {
            temp = temp.next;
            length++;
        }
        if(length < index) {
            throw new RuntimeException("链表越界");
        }

        temp = head.next;
        for(int i = 0; i<length-index; i++) {
            temp = temp.next;
        }
        return temp;
    }
    public LinkedList reverselist() {
        if (head.next == null && head.next.next == null) {
            System.out.println("无须反转");
        }
        LinkedList newlink = new LinkedList();
        newlink.head = new Student(0, "");
        Student temp = head.next;
        Student nexttemp = head.next.next;
        while (true) {
            temp.next = newlink.head.next;
            newlink.head.next = temp;
            //移动到下一个节点
            temp = nexttemp;
            nexttemp = nexttemp.next;
            if (temp.next == null) {
                //插入最后一个节点
                temp.next = newlink.head.next;
                newlink.head.next = temp;
                head.next = null;
                return newlink;
            }
        }
    }

    public void reverseTraverse() {
        if(head == null) {
            System.out.println("链表为空");
        }

        Student temp = head.next;
        //创建栈，用于存放遍历到的节点
        Stack<Student> stack = new Stack<>();
        while(temp != null) {
            stack.push(temp);
            temp = temp.next;
        }

        while (!stack.isEmpty()) {
            System.out.println(stack.pop());
        }
    }

}



class Student{
    int id;
    String name;
    Student next;

    public Student(int id, String name){
        this.id = id;
        this.name = name;
    }


    @Override
    public String toString() {
        return "student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

## 栈

#### 1、定义

- 栈是一个**先入后出**的有序列表
- 栈(stack)是限制线性表中元素的插入和删除只能在线性表的**同一端进行**的一种特殊线性表。允许插入和删除的一端，为变化的一端，称为栈顶，另一端为固定的一端，称为栈底
- 最先放入的元素在栈底，且最后出栈。最后放入的元素在栈顶，且最先出栈

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.7h65jlgyh8o0.png" alt="image" style="zoom:50%;" />

#### 2、应用场景

- 子程序递归调用。如JVM中的虚拟机栈
- 表达式转换（中缀转后缀）与求值
- 二叉树的遍历
- 图的深度优先遍历

#### 3、实现

**用数组实现**

**思路**

- 定义top表示栈顶，初始值为-1

- 入栈的操作，先让top++，再放入数组

- 出栈操作，先取出元素，在让top–

- top == -1时，栈空

- top == maxSize-1时，栈满

**代码实现**

```java
public class Demo1 {
   public static void main(String[] args) {
      ArrayStack stack = new ArrayStack(5);
      //压栈
      stack.push(1);
      stack.push(2);
      stack.push(3);
      stack.push(4);
      stack.push(5);
      //出栈
      System.out.println(stack.pop());
   }
}

class ArrayStack {
   private final int maxSize;
   int[] stack;
   private int top;

   public ArrayStack(int maxSize) {
      this.maxSize = maxSize;
      stack = new int [this.maxSize];
      top = -1;
   }

   private boolean isEmpty() {
      return top == -1;
   }

   private boolean isFull() {
      return top == maxSize-1;
   }

   public void push(int i) {
      if(isFull()) {
         throw new StackOverflowError("栈满");
      }
      //压栈
      top++;
      stack[top] = i;
   }

   public int pop() {
      if(isEmpty()) {
         throw new EmptyStackException();
      }
      int retNum = stack[top];
      top--;
      return retNum;
   }
}
```



#### 4、应用

#### 表达式求值

**思路**

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.77477mlpza40.png)

- 准备一个索引index来帮助我们遍历表达式
- 如果index位置上的元素是一个数字，就直接入栈
- 如果index位置上的元素是一个符号
  - 如果符号栈为空，直接入栈
  - 如果符号栈不为空
    - index位置上的符号的优先级小于或等于栈顶符号的优先级，则弹出两个数栈中的元素和符号栈中的一个符号，并且进行计算。将运算结果放入数栈中，并将index位置上的符号压入符号栈
    - index位置上的符号的优先级大于符号栈栈顶符号的优先级，则将该符号压入符号栈
- 当表达式遍历完毕后，就弹出数栈中的2个数字和符号栈中的1个符号进行运算，并将运行结果入栈
- 最终数栈中只有一个值，这个值便是运算结果
- 注意：
  - 读取的是字符，所以存入数字前需要减去0的ASCII码
  - 如果数字是多位数，需要一直读，读到下一位不是数字为止，然后将读到的字符进行拼接，然后一起压入数栈

**代码实现**

```java
public class ComputerStack {
    public static void main(String[] args) {
        String formula = "12+3*15+3/3";
        //索引，用来读取字符串中的元素
        int index = 0;
        //保存读取到的数字和符号
        int number1 = 0;
        int number2 = 0;
        int thisChar = ' ';
        //用于拼接数字
        StringBuilder spliceNumber = new StringBuilder();
        //数栈和符号栈
        ArrayStack2 numberStack = new ArrayStack2(10);
        ArrayStack2 operationStack = new ArrayStack2(10);
        //保存运算结果
        int result;

        //开始读取字符串中的元素
        for (index = 0; index < formula.length(); index++) {
            thisChar = formula.charAt(index);
            if (operationStack.isOperation(thisChar)) {
                if(operationStack.comparePriority(thisChar)) {
                    operationStack.push(thisChar);
                } else {
                    int popChar = operationStack.pop();
                    number2 = numberStack.pop();
                    number1 = numberStack.pop();
                    //获得运算结果
                    result = operationStack.calculation(number1, number2, popChar);
                    operationStack.push(thisChar);
                    numberStack.push(result);
                }
            } else {
                //如果是数字，就一直读取
                while(thisChar>='0' && thisChar<='9') {
                    //可能该数字为多位数，所以不能只存入一位数字
                    spliceNumber.append(thisChar - '0');
                    System.out.println("拼接字符换 " + spliceNumber);
                    index++;
                    //如果已经读了最后一个数字了，就停下来
                    if(index >= formula.length()) {
                        break;
                    }
                    thisChar = formula.charAt(index);
                }
                int number = Integer.parseInt(spliceNumber.toString());
                numberStack.push(number);
                //初始化spliceNumber
                spliceNumber = new StringBuilder();
                index--;
            }
        }

        while(!operationStack.isEmpty()) {
            int popChar = operationStack.pop();
            number2 = numberStack.pop();
            number1 = numberStack.pop();
            //获得运算结果
            result = operationStack.calculation(number1, number2, popChar);
            numberStack.push(result);
        }

        System.out.println(numberStack.pop());
    }
}

class ArrayStack2 {
    private final int maxSize;
    int[] stack;
    private int top;

    public ArrayStack2(int maxSize) {
        this.maxSize = maxSize;
        stack = new int[this.maxSize];
        top = -1;
    }

    public boolean isEmpty() {
        return top == -1;
    }

    public boolean isFull() {
        return top == maxSize - 1;
    }

    public void push(int i) {
        if (isFull()) {
            throw new StackOverflowError("栈满");
        }
        //压栈
        top++;
        stack[top] = i;
    }

    public int pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        int retNum = stack[top];
        top--;
        return retNum;
    }

    public void  traverse() {
        for(int thiChar : stack) {
            System.out.println(thiChar);
        }
    }

    /**
     * 判断符号的优先级
     *
     * @param operation 传入运算符
     * @return 返回优先级
     */
    public int getPriority(int operation) {
        if (operation == '*' || operation == '/') {
            return 2;
        } else if (operation == '+' || operation == '-') {
            return 1;
        } else if (operation >= '0' && operation <= '9') {
            return 0;
        } else {
            return -1;
        }
    }

    /**
     * 比较栈顶元素和传入字符的优先级大小
     *
     * @param operation 传入字符
     * @return true则是传入字符优先级大于栈顶字符，false反之
     */
    public boolean comparePriority(int operation) {
        if (isEmpty()) {
            return true;
        } else {
            int priority1 = getPriority(operation);
            int priority2 = getPriority(stack[top]);
            return priority1 > priority2;
        }
    }

    /**
     * 判断该位置是不是一个符号
     *
     * @param operation 该位置的符号
     * @return 判断结果
     */
    public boolean isOperation(int operation) {
        return operation == '*' || operation == '/' || operation == '-' || operation == '+';
    }

    /**
     * @param number1   第一个运算的数字
     * @param number2   第二个运算的数字
     * @param operation 运算符
     * @return
     */
    public int calculation(int number1, int number2, int operation) {
        switch (operation) {
            case '+':
                return number1+number2;
            case '-':
                return number1-number2;
            case '*':
                return number1*number2;
            case '/':
                return number1/number2;
            default:
                System.out.println(operation);
                throw new RuntimeException("符号读取错误！");
        }
    }
}
```

#### 前缀、中缀、后缀表达式（逆波兰表达式）

**前缀表达式定义**

从右至左扫描表达式，遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数字，用运算符对他们做相应的计算（栈顶元素和次顶元素），并将结果入栈；重复过程知道表达式最左端，最后运算出的值就是表达式的结果。

**例子**：

（3+4）✖5-6，对应的前缀表达式就是 -✖+3 4 5 6，

**中缀表达式**

就是常见的运算表达式，**例如**，（3+4）✖5-6

**后缀表达式**

从左至右扫描表达式，遇到数字时，将数字压入堆栈，遇到运算符时，弹出栈顶的两个数字，用运算符对他们做相应的计算（栈顶元素和次顶元素），并将结果入栈；重复过程知道表达式最左端，最后运算出的值就是表达式的结果。

#### **设计**

**中缀表达式转后缀表达式**

- **从左向右**读取中缀表达式，并且创建**栈s**和**队列q**

- 如果读到的元素的数字，就直接入队放入q中

- 如果读到的是

  运算符

  （运算符判定）

  - 如果s为空，则将该运算符压入s
  - 如果s不为空
    - 如果该运算符为**左括号**，则直接压入s
    - 如果该运算符为**右括号**，则将s中的元素依次出栈并入队到q中，**直到遇见左括号为止**（括号不放入q中）
    - 如果该运算符的优先级**高于**s栈顶的运算符，则将该元素压入s
    - 如果该运算符的优先级**小于等于**s栈顶的运算符，则弹出s栈顶的元素，并将其放入q中，该运算符重**新判定**入栈操作（运算符判定步骤）

- 如果中缀表达式已经读取完毕，则将s中的元素依次出栈，放入q中

- q中的元素依次出队，该顺序即为后缀表达式

**代码**

```java
public class Demo4 {
    static Queue<String> queue = new LinkedList<>();
    static Stack<String> stack = new Stack<>();

    public static void main(String[] args) {
        //中缀表达式，加上空格，方便取出
        String infixExpression = "1 + ( ( 2 + 3 ) * 4 ) - 5";
        String[] expressionArr = infixExpression.split(" ");//按照空格取出
        //用来保存该运算符的类型
        int type;
        //取出的字符串
        String element;
        //弹出栈的字符串
        String stackEle;
        for(int i=0; i<expressionArr.length; i++) {
            element = expressionArr[i];
            type = judgeOperator(element);
            if(type == 0) {
                //数字，直接入队
                queue.add(element);
            }else if(type == 1) {
                //左括号，直接压栈
                stack.push(element);
            }else if(type == 3) {
                //如果右括号，弹出栈顶元素，直到遇见左括号位置再停下来
//                do {
//                    stackEle = stack.pop();
//                    if(stackEle.equals("(")) {
//                        break;
//                    }
//                    queue.add(stackEle);
//                    //弹出栈中的左括号
//                }while (!stackEle.equals("("));
                while (true){
                    stackEle = stack.pop();
                    if(stackEle.equals("(")) {
                        break;
                    }
                    queue.add(stackEle);
                }
            }else if(type == 2) {
                if(stack.isEmpty()) {
                    //如果栈为空，直接入栈
                    stack.push(element);
                    continue;
                }
                int priority1 = getPriority(element);
                //获得栈顶元素，并判断其优先级
                stackEle = stack.peek();
                int priority2 = getPriority(stackEle);
                if(priority2 == 0) {
                    //为左括号，运算符直接入栈
                    stack.push(element);
                }else if(priority1 > priority2) {
                    //该运算符优先级高于栈顶元素优先级，则入栈
                    stack.push(element);
                }else {
                    stackEle = stack.pop();
                    queue.add(stackEle);
                    //重新判断该运算符
                    i--;
                }
            }
        }
        //把最后一个元素出栈并入队
        stackEle = stack.pop();
        queue.add(stackEle);
        //保存队列长度，因为出队过程中队列的长度会被改变
        int length = queue.size();
        for(int i=0; i<length; i++) {
            element = queue.remove();
            System.out.print(element);
        }
    }

    /**
     * 判断该运算符是不是加减乘除
     * @param operation 运算符
     * @return true则该运算符为加减乘除
     */
    public static boolean firstJudge(String operation) {
        return operation.equals("*") || operation.equals("/") || operation.equals("+") || operation.equals("-");
    }


    /**
     * 判断该字符串的类型
     * @param operation 要判断的字符串
     * @return 3->右括号 2->加减乘除运算符 1->左括号
     */
    public static int judgeOperator(String operation) {
        if(operation.equals(")")) {
            return 3;
        }
        if(firstJudge(operation)) {
            return 2;
        }
        if(operation.equals("(")) {
            return 1;
        } else {
            return 0;
        }
    }

    /**
     * 判断运算符优先级
     * @param operator 要判断的运算符
     * @return 2代表乘除，1代表加减，0代表左括号
     */
    public static int getPriority(String operator) {
        if(operator.equals("*") || operator.equals("/")) {
            return 2;
        }else if(operator.equals("+") || operator.equals("-")) {
            return 1;
        } else {
            return 0;
        }

    }
}

```

## 递归

#### 1、概念

**递归就是方法自己调用自己**,每次调用时**传入不同的变量**.递归有助于编程者解决复杂的问题,同时可以让代码变得简洁。并且递归用到了**虚拟机栈**

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.11gz0s24mtrk.png)

#### 2、能解决的问题

**数学问题**

- 八皇后问题
- 汉诺塔
- 求阶乘
- 迷宫问题
- 球和篮子
- 各种排序算法

#### 3、规则

- 方法的变量是独立的，不会相互影响的

- 如果方法中使用的是引用类型变量(比如数组)，就会共享该引用类型的数据

- 递归**必须向退出递归的条件逼近**，否则就是无限递归，出现 StackOverflowError

- 当一个方法执行完毕，或者遇到 return，就会返回，**遵守谁调用，就将结果返回给谁**，同时当方法执行完毕或

  者返回时，该方法也就执行完毕

#### 4、迷宫问题

**思路**

- 用一个二维矩阵代表地图

  - 1代表边界
  - 0代表未做过该地点
  - 2代表走过且能走得通
  - 3代表走过但走不通

- 设置起点和终点以及每个地点的

  行走策略

  - 行走策略指**在该点所走的方向的顺序**，如 右->下->左->上（调用寻找路径的方法，使用递归）

- 每次行走时假设该点能够走通，然后按照策略去判断，如果所有策略判断后都走不通，则**该点走不通**

**图解**

初始地图

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.3ssoyaymqj00.png)

**行走路径**

策略：右下左上

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.1lumm1imq9vk.png)

**代码**

```java
public class RecursionTest {
    public static void main(String[] args) {
        //得到地图
        int length = 7;
        int width = 6;
        int[][] map = getMap(length, width);

        //设置一些障碍
        map[1][2] = 1;
        map[2][2] = 1;

        //打印地图
        System.out.println("地图如下");
        for(int i=0; i<length; i++) {
            for(int j=0; j<width; j++) {
                System.out.print(map[i][j]+" ");
            }
            System.out.println();
        }

        //走迷宫
        getWay(map, 1, 1);

        //行走路径
        System.out.println("行走路径");
        for(int i=0; i<length; i++) {
            for(int j=0; j<width; j++) {
                System.out.print(map[i][j]+" ");
            }
            System.out.println();
        }

    }

    /**
     * 创建地图
     * @param length 地图的长
     * @param width 地图的宽
     * @return 创建好的地图
     */
    public static int[][] getMap(int length, int width) {
        int[][] map = new int[length][width];
        //先将第一行和最后一行设置为1（边界）
        for(int i=0; i<width; i++) {
            map[0][i] = 1;
            map[length-1][i] = 1;
        }
        //再将第一列和最后一列设置为1
        for(int i=0; i<length; i++) {
            map[i][0] = 1;
            map[i][width-1] = 1;
        }
        return map;
    }

    /**
     * 开始走迷宫
     * @param map 地图
     * @param i 起点横坐标
     * @param j 七点纵坐标
     * @return 能否走通，true能走通，false反之
     */
    public static boolean getWay(int[][] map, int i, int j) {
        int length= map.length;
        int width = map[0].length;
        //假设右下角为终点
        if(map[length-2][width-2] == 2) {
            //走通了，返回true
            return true;
        } else {
            if(map[i][j] == 0) {
                //假设改路能走通
                map[i][j] = 2;
                //行走策略 右->下->左->上
                if(getWay(map, i, j+1)) {
                    return true;
                }else if(getWay(map, i+1, j)) {
                    return true;
                }else if(getWay(map, i-1, j)) {
                    return true;
                }else if(getWay(map, i, j-1)) {
                    return true;
                }
                //右下左上都走不通
                map[i][j] = 3;
            }else {
                //改路已经被标记过了，不用再走了，直接返回false
                return false;
            }
        }
        return false;
    }
}

```

#### 5、八皇后问题

八皇后问题，是一个古老而著名的问题，是**回溯算法**的典型案例。该问题是国际西洋棋棋手马克斯·贝瑟尔于1848年提出：在 8×8格的国际象棋上摆放八个皇后，使其不能互相攻击，即：任意两个皇后都**不能处于同一行、同一列或同一斜线上**，问有多少种摆法(92)。

**思路**

- 将第一个皇后放在第一行第一列

- 将第二个皇后放在第二行第一列，判断是否会和其他皇后相互攻击，若会相互攻击，则将其放到第三列、第四列…知道不会相互攻击为止

- 将第三个皇后放在第三行第一列，判断是否会和其他皇后相互攻击，若会相互攻击，则将其放到第三列、第四列…知道不会相互攻击为止，并**以此类推**，**在摆放的过程中，有可能会改动前面所放的皇后的位置**

- 当得到一个正确的解时，就会回溯到上一行，由此来找出第一个皇后在第一行第一列的所有解

- 再将第一个皇后放到第一行第二列，并重复以上四个步骤

- **注意**：

  - 棋盘本身应该是用二维数组表示，但是因为皇后所在的行数是固定的，所以可以简化为用一个一维数组来表示。其中的值代表皇后所在的列

  - 数组下标代表皇后所在行数，所以判断是否在同一行列斜线上时，只需要判断是否在同一列和同一斜线上即可

    - 是否同列判断：值是否相同

    - 是否同一斜线：行号-行号是否等于列号-列号，且**列号相减要取绝对值**

      [^1]: 判断是否在同一条斜线上的方法

**代码**

```java
public class EightQueen {
    /**
     * 创建皇后所放位置的数组，数组的下标代表行号，数组中的值代表所在的列号
     */
    static int sum = 0;
    int  max = 8;
    int[] arr = new int[max];
    public static void main(String[] args) {
        EightQueen demo = new EightQueen();
        //放入第一个皇后，开始求后面的皇后
        demo.check(0);
        System.out.println("一共有"+sum+"种放法");
    }

    /**
     * 打印数组元素
     */
    public void print() {
        for(int i = 0; i<arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
        sum++;
        System.out.println();
    }

    /**
     * 判断该位置的皇后与前面几个是否冲突
     * @param position 需要判断的皇后的位置
     * @return true代表冲突，false代表不冲突
     */
    public boolean judge(int position) {
        for(int i = 0; i<position; i++) {
            //如果两个皇后在同一列或者同一斜线，就冲突
            //因为数组下标代表行数，所以不会存在皇后在同一行
            //所在行数-所在行数 如果等于 所在列数-所在列数，则两个皇后在同一斜线上
            if(arr[i] == arr[position] || (position-i) == Math.abs(arr[position]-arr[i])) {
                return true;
            }
        }
        return false;
    }

    /**
     * 检查该皇后应放的位置
     * @param queen 要检查的皇后
     */
    public void check(int queen) {
        if(queen == max) {
            //所有的皇后都放好了，打印并返回
            print();
            return;
        }
        //把皇后放在每一列上，看哪些不会和之前的冲突
        for(int i = 0; i<max; i++) {
            //把第queen+1个皇后放在第i列
            arr[queen] = i;
            if(!judge(queen)) {
                //不冲突，就去放下一个皇后
                check(queen+1);
            }
        }
    }
}

```

## 五、排序

#### 1、常见的排序算法

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/排序导图.1h6ufpnuf19c.png" alt="排序导图" style="zoom: 75%;" />

#### 2、算法的时间复杂度

#### 时间频度和时间复杂度

**时间频度T(n)**

一个算法执行所耗费的时间，从理论上是不能算出来的，必须上机运行测试才能知道。但我们不可能也没有必要对每个算法都上机测试，只需知道哪个算法花费的时间多，哪个算法花费的时间少就可以了。并且一个算法花费的时间与算法中**语句的执行次数成正比例**，哪个算法中语句执行次数多，它花费时间就多。**一个算法中的语句执行次数称为语句频度或时间频度**。记为T(n)。

**时间复杂度O(n)**

一般情况下，算法中基本操作重复执行的次数是问题规模n的某个函数，用T(n)表示，若有某个辅助函数f(n),使得当n趋近于无穷大时，T（n)/f(n)的极限值为不等于零的常数，则称f(n)是T(n)的同数量级函数。记作T(n)=O(f(n)),称O(f(n)) 为算法的渐进时间复杂度，简称时间复杂度。

在T(n)=4n²-2n+2中，就有f(n)=n²，使得T（n)/f(n)的极限值为4，那么O(f(n))，也就是时间复杂度为O(n²)

- 对于不是只有常数的时间复杂度**忽略时间频度的系数、低次项常数**
- 对于只有常数的时间复杂度，将常数看为1

#### 常见的时间复杂度

**常数阶 O(1)**



```java
int i = 1;
i++;
```

无论代码执行了多少行，只要没有循环等复杂的结构，时间复杂度都是O(1)

**对数阶O(log~2~n)**

```java
while(i<n) {
    i = i*2;
}
```

**线性阶O(n)**

```java
for(int i = 0; i<n; i++) {
	i++;
}
```

这其中，循环体中的代码会执行n+1次，时间复杂度为O(n)

**线性对数阶O（nlog~2~n）**

```java
for(int i = 0; i<n; i++) {
    j = 1;
	while(j<n) {
		j = j*2;
	}
}
```

此处外部为一个循环，循环了n次。内部也是一个循环，但内部f循环的时间复杂度是log~2~n。所以总体的时间复杂度为线性对数阶O(nlog~2~n)

**平方阶O(n^2^)**

```java
for(int i = 0; i<n; i++) {
	for(int j = 0; j<n; j++) {
		//循环体
	}
}
```

**立方阶O(n^3^)**

```java
for(int i = 0; i<n; i++) {
	for(int j = 0; j<n; j++) {
		for(int k = 0; k<n; k++) {
			//循环体
		}
	}
}
```

可以看出平方阶、立方阶的复杂度主要是否循环嵌套了几层来决定的

#### 3、排序算法的时间复杂度

| 排序算法 | 平均时间 | 最差时间     | 稳定性 | 空间复杂度 |                     备注                     |
| -------- | -------- | ------------ | ------ | ---------- | :------------------------------------------: |
| 冒泡排序 | O(n^2^)  | O(n^2^)      | 稳定   | O(1)       |                  n较小时好                   |
| 交换排序 | O(n^2^)  | O(n^2^)      | 不稳定 | O(1)       |                  n较小时好                   |
| 选择排序 | O(n^2^)  | O(n^2^)      | 不稳定 | O(1)       |                  n较小时好                   |
| 插入排序 | O(n^2^)  | O(n^2^)      | 稳定   | O(1)       |               大部分已有序时好               |
| 基数排序 | O(n*k)   | O(n*k)       | 稳定   | O(n)       | 二维数组（桶）、一维数组（桶中首元素的位置） |
| 希尔排序 | O(nlogn) | O(ns)(1<s<2) | 不稳定 | O(1)       |                 s是所选分组                  |
| 快速排序 | O(nlogn) | O(n^2^)      | 不稳定 | O(logn)    |                  n较大时好                   |
| 归并排序 | O(nlogn) | O(nlogn)     | 稳定   | O(1)       |                  n较大时好                   |
| 堆排序   | O(nlogn) | O(nlogn)     | 不稳定 | O(1)       |                  n较大时好                   |

#### 4、冒泡排序

**算法步骤**

- 比较相邻的元素。如果第一个比第二个大，就交换他们两个。

- 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，**最后的元素会是最大的数**。

- 针对所有的元素重复以上的步骤，**除了最后一个**。

- 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

- 一共进行了**数组元素个数-1**次大循环，且每次大循环中需要比较的元素越来越少。

- 优化：如果在某次大循环，发现没有发生交换，则证明已经有序

  **代码**

```java
public class BubbleSort {
    public static void main(String[] args) {
        int []arr = {8, 9 ,6, 5, 16, 0};
        for (int i = 1; i <arr.length; i++) {
            boolean flag = true;
            for(int j = 0;j < arr.length - i;j++){ //j < arr.length - i 是每过一个循环，都将这次所选择最大的数冒泡到数组的最后，所以
                if(arr[j] > arr[j+1]){
                    int tmp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = tmp;
                    flag = false;
                }
            }
            if (flag){
                break;
            }
        }
        for (int i: arr){
            System.out.println(i);
        }
    }
}

```

#### 5、选择排序

**算法步骤**

- 遍历整个数组，找到最小（大）的元素，放到数组的起始位置。
- 再遍历剩下的数组，找到剩下元素中的最小（大）元素，放到数组的第二个位置。
- 重复以上步骤，直到排序完成。
- 一共需要遍历数组元素个数-1次，当找到第二大（小）的元素时，可以停止。这时最后一个元素必是最大（小）元素。

```java
public class InsertSort {
    public static void main(String[] args) {
        int []arr = {8, 9 ,6, 5, 16, 0};
        for (int i =0; i < arr.length -1; i++){
            int min = i;
            for (int j = i+1; j < arr.length; j++){
                if (arr[min] > arr[j]){ // 每次和保存的值min比较，不可以和i比较，这样数据会错乱；
                    min = j;
                }
            }

                int tmp = arr[i];
                arr[i] = arr[min];
                arr[min] = tmp;
        }
        for (int i: arr){
            System.out.println(i);
        }
    }
}

```

#### 6、插入排序

**算法步骤**

- 将待排序序列第一个元素看做一个**有序序列**，把第二个元素到最后一个元素当成是**未排序序列**。
- 从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。

**代码**

```java
/**
 * @ClassName: InsertSort
 * @Author: Rokkie丶zimiao
 * @Date: 2021/5/23 21:23
 * @Description: 将待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。
 *               从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。（如果待插入
 *               的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。
 */
public class InsertSort {
    public static void main(String[] args) {
        int []arr = {8, 9 ,6, 5, 16, 0};
        for (int i = 1; i <arr.length ; i++) {
            int tmp = arr[i];
            int j = i;
            while (j>0 && tmp<arr[j-1]){
                arr[j] = arr[j -1];
                j--;
            }
            if(j != i){
                arr[j] = tmp;
            }
        }
        for (int i: arr){
            System.out.println(i);
        }
    }
}

```

#### 7、希尔排序

**回顾：插入排序存在的问题**

当最后一个元素为整个数组的最小元素时，需要将前面的有序数组中的每个元素都向后移一位，这样是非常花时间的。

所以有了希尔排序来帮我们将数组从无序变为整体有序再变为有序。

**算法步骤**

- 选择一个增量序列t1（一般是数组长度/2），t2（一般是一个分组长度/2），……，tk，**其中 ti > tj**, tk = 1；
- 按增量序列个数 k，对序列进行 k 趟排序；
- 每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

**示意图**

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.27t3hwt5lbr4.png" alt="image" style="zoom:80%;" />

**代码**

```java
public class ShellSort {
    public static void main(String[] args) {
      int []arr = {8, 9 ,6, 5, 16, 0, 7};
      int tmp;
      // 将数组分组，分成gap组，每个组内插入排序；
      for (int gap = arr.length/2; gap > 0; gap = gap /2){
          for (int i = gap; i< arr.length;i++){
              tmp = arr[i];
              int j = i;
              //判断数组是否越界
              while (j - gap >= 0 && tmp < arr[j - gap]){
                  arr[j] = arr[j - gap];
                  j -= gap;
              }
              if(j != i) {
                  arr[j] = tmp;
              }
          }
      }
      for (int i: arr){
          System.out.println(i);
      }
    }
}
```

#### 8、快速排序

**算法步骤**

- 从数列中挑出一个元素，称为 “基准”（pivot）;
- 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
- 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序； 

**例子**

具体讲解请看：[快速排序——JAVA实现（图文并茂）](https://blog.csdn.net/qq_26122557/article/details/79458649?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162244356216780269821396%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162244356216780269821396&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-79458649.first_rank_v2_pc_rank_v29&utm_term=%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8Fjava&spm=1018.2226.3001.4187)

**示意图**

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.5k0zf98xs9g0.png)

**代码**

```java 
public class QuickSort {
    public static void main(String[] args) {
        int []arr = {8, 9 ,6, 5, 16, 0, 7};
        quaick qick = new quaick();
        qick.quickSort(arr, 0 ,arr.length-1);
        for (int i: arr){
            System.out.println(i);
        }
    }
}

/**
 *  快速排序的思想
 */
class quaick{
    int i, j , temp, t;
    public void quickSort(int []arr, int left ,int right){
        //设立递归调用结束标志;
        if (left>right){
            return;
        }
        i = left;
        j = right;
        temp = arr[left]; //基准位

        while (i<j){//保证左指针小于右指针，直到等于才出循环
            while (j>i&&temp<=arr[j]){//寻找小于基准数的位置
                j --;
            }
            while (i<j&&temp>=arr[i]){//寻找大于基准数的位置
                i++;
            }
            if (i<j){//交换两个数
                t = arr[j];
                arr[j] = arr[i];
                arr[i] = t;
            }
        }
        //成功会合后，找到下一个基准数，此时数组一分为二，依次递归
        arr[left] = arr[i];
        arr[i] = temp;
        //递归调用
        quickSort(arr, left, j-1);
        quickSort(arr, j+1, right);
    }
}

```

#### 9、归并排序

**算法步骤**

归并排序用到了**分而治之**的思想，其难点是**治**

- 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列
- 设定两个指针，最初位置分别为两个已经排序序列的起始位置
- 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置
- 重复上一步 直到**某一指针达到序列尾**
- 将另一序列剩下的所有元素直接复制到合并序列尾

**图解**

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.7lhq6dv1wfg0.png)

**代码**

```java
public class Demo6 {
   public static void main(String[] args) {
      int[] arr = {1, 5, 6, 3, 2, 8, 7, 4};
      MergeSort mergeSort = new MergeSort(arr.length);
      mergeSort.mergeSort(arr, 0, arr.length-1);
      for(int a : arr) {
         System.out.println(a);
      }
   }
}

class MergeSort {
   /**
    * 临时数组，用于合并时用于存放元素
    */
   int[] temp;


   public MergeSort() {
   }

   public MergeSort(int length) {
      temp = new int[length];
   }

   /**
    * 将分解的序列进行合并，合并的同时完成排序
    * @param arr 待合并的数组
    * @param left 数组左边界
    * @param right 数组右边界
    */
   private void merge(int[] arr, int left, int right) {
      //两个序列的分界点
      int mid = (left+right)/2;
      //temp数组中插入的位置
      int tempLeft = 0;
      int arrLeft = left;
      //第二个序列的首元素下标
      int arrRight = mid+1;

      while(arrLeft<=mid && arrRight<=right) {
         //如果第一个序列的元素小于第二序列的元素，就将其放入temp中
         if(arr[arrLeft] <= arr[arrRight]) {
            temp[tempLeft] = arr[arrLeft];
            arrLeft++;
         }else {
            temp[tempLeft] = arr[arrRight];
            arrRight++;
         }
         tempLeft++;
      }


      //将不为空的序列中的元素依次放入temp中
      while (arrLeft <= mid) {
         temp[tempLeft] = arr[arrLeft];
         tempLeft++;
         arrLeft++;
      }

      while (arrRight <= right) {
         temp[tempLeft] = arr[arrRight];
         tempLeft++;
         arrRight++;
      }

      //将临时数组中的元素放回数组arr中
      tempLeft = 0;
      arrLeft = left;
      while (arrLeft <= right) {
         arr[arrLeft] = temp[tempLeft];
         arrLeft++;
         tempLeft++;
      }
   }

   public void mergeSort(int[] arr, int left, int right) {
      int mid = (left+right)/2;
      if(left < right) {
         mergeSort(arr, left, mid);
         mergeSort(arr, mid+1, right);
         merge(arr, left, right);
      }
   }
}
```

#### 10、基数排序

- 将所有待比较数值（正整数）统一为同样的数位长度，**数位较短的数前面补零**
- 从最低位开始，依次进行一次排序
- 从最低位排序一直到最高位（个位->十位->百位->…->最高位）排序完成以后, 数列就变成一个有序序列
- 需要我们获得最大数的位数
  - **可以通过将最大数变为String类型，再求得它的长度即可**

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.5v4jtzysz380.png)

**代码**

```java
public class RadixSort {
    public static void main(String[] args) {
        int []arr = {8, 9 ,6, 5, 16, 98213, 2767, 266};
        Radix domo = new Radix();
        domo.sort(arr);
        for (int i : arr){
            System.out.println(i);
        }
    }
}

class Radix{
    /**
     * 进行基数排序  待排序的数组
     * @param  arr 待排序的数组
     */
    public void sort(int[] arr){
        //创建一个二维数组，用于表示桶
        //桶的个数固定为10个（个位是0~9），最大容量由数组的长度决定
        int [][] bucket = new int[10][arr.length];
        //用于记录每个桶中有多少个元素
        int []elements = new int[10];

        //获得最大位数
        int maxlength = getlength(arr);

        for (int time = 1, step=1; time<=maxlength;time++,step *=10){
            for (int i = 0; i < arr.length; i++) {
                //去除位数
                int digt = arr[i]/step%10;
                //放入桶中
                bucket[digt][elements[digt]] = arr[i];
                //桶中元素加一
                    elements[digt]++;
            }
            //将桶中的元素重新放回到数组中
            //用于记录应该放入原数组的哪个位置
            int index = 0;
            for (int i = 0; i < 10; i++) {
                //从桶中按放入顺序依次取出元素，放入原数组
                int position = 0;
                //桶中有元素才取出
                while (elements[i]>0){
                    arr[index] = bucket[i][position];
                    elements[i]--;
                    position ++;
                    index++;
                }

            }
        }
    }

    private int getlength(int arr[]){
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if(max<arr[i]){
                max = arr[i];
            }
        }

        int digits = (max + "").length();
        return digits;
    }
}
```

## 六、查找

#### 1、线性查找

线性查找是一种非常简单的查找方式。查找思路是：从数组的一个元素出发，一个个地和要查找的值进行比较，如果发现有相同的元素就返回该元素的下标。反之返回-1（未找到）

**代码**

```java
public class LineSearch {
    public static void main(String[] args) {
        int[] arr = {98,213,28,2,3,4};
        int index = line(4, arr);
        if (index == -1){
            System.out.println("没有找到");
        }else{
            System.out.println("元素下标为" + index);
        }
    }

    public static int line(int value, int[] arr){
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == value){
                return i;
            }
        }
        return -1;
    }
}
```

#### 2、二分查找

进行二分查找的数组必须为**有序数组**

- 设置一个指向中间元素下标的变量mid，mid=(left + right)/2
- 让要查找的元素和数组mid下标的元素进行比较
  - 如果查找的元素大于arr[mid]，则left变为mid后面一个元素的下标
  - 如果查找的元素小于arr[mid]，则right变为mid前一个元素的下标
  - 如果查找的元素等于arr[mid]，则mid就是要查找元素所在的位置
- 当left > rigth时，说明元素不在该数组中

**代码**

```java
public class BinarySearch {
    public static void main(String[] args) {

        int[] arr = {-1, 5, 7, 10, 18, 27};
        int num = binarySerach(arr, 5);
        System.out.println("-------------");
        if (num == -1){
            System.out.println("fault");
        }else {
            System.out.println("index is " + num);
        }
    }

    public static int binarySerach(int[] arr, int num){
        int left = 0;
        int right = arr.length -1;
        while (left <= right){
            int mid = (right - left )/2 + left;
            if (arr[mid] < num){
                left = mid+1;
            }else if (arr[mid] >num){
                right = mid -1;
            }else{
                return mid;
            }
        }
        return -1;
    }
}
```

**但是有可能要查找的元素有多个**。这时就需要在找到一个元素后，不要立即返回，而是**扫描其左边和右边的元素**，将所有相同元素的下标保存到一个数组中，然后一起返回

```java
public class BinarySearch {
    public static void main(String[] args) {

        int[] arr = {-1, 5, 7, 10, 10, 10, 10,18, 27};
        Arrays.sort(arr);
        List<Integer> result = binarySearch(arr, 10);
        result.sort(Comparator.naturalOrder());
        System.out.println("----a---------");
        if (result.size() == 0){
            System.out.println("fault");
        }else {
            for (Integer index: result){
                System.out.println(index);
            }
        }
    }

    public static List<Integer> binarySearch(int []arr, int num){
        int left = 0;
        int right = arr.length -1;
        List<Integer> binlist = new ArrayList<>();
        while(left <= right) {
            int mid = (left + right) / 2;
            //如果要查找的值大于中间位置的值，说明要查找的值在右边部分
            if (arr[mid] < num) {
                left = mid + 1;
            } else if (arr[mid] > num) {
                //如果要查找的值小于中间位置的值
                //说明要查找的值在左边部分
                right = mid - 1;
            } else {
                binlist.add(mid);
                int leftmid = mid - 1;
                while (leftmid > 0 && arr[leftmid] == num) {
                    binlist.add(leftmid);
                    leftmid--;
                }

                int rightmid = mid + 1;
                while (rightmid < right && arr[rightmid] == num) {
                    binlist.add(rightmid);
                    rightmid++;
                }
                return binlist;
            }
        }
        return binlist;
    }
```

#### 3、插值查找

在二分查找中，如果我们 要找的元素位于数组的最前端或者最后段，这时的查找效率是很低的。所以在二分查找至上，引入了插值查找，也是一种**基于有序数组**的查找方式

插值查找与二分查找的区别是：插值查找使用了一种**自适应**算法，用这种算法来计算mid。

mid的值在两种查找算法中的求法：

- 二分查找：mid = (left + right)/2

- 插值查找：

  mid = left + (right - left) * (num - arr[left]) / (arr[right] - arr[left])

  - 其中num为要查找的那个值

**代码**

```java
public class InsertSearch {
    public static void main(String[] args) {
        int[] arr = {-1, 8, 7, 10, 10, 10, 10, 29, 27};
        Arrays.sort(arr);
        List<Integer> result = binarySearch(arr, 10);
        result.sort(Comparator.naturalOrder());
        System.out.println("-------------");
        if (result.size() == 0){
            System.out.println("fault");
        }else {
            for (Integer index: result){
                System.out.println(index);
            }
        }
    }



    public static List<Integer> binarySearch(int []arr, int num){
        int left = 0;
        int right = arr.length -1;
        List<Integer> binlist = new ArrayList<>();
        while(left <= right) {
            // 和二分查找一样，只需要改一个公式；
            int mid = left + (right - left)*(num - arr[left])/(arr[right]-arr[left]);
            //如果要查找的值大于中间位置的值，说明要查找的值在右边部分
            if (arr[mid] < num) {
                left = mid + 1;
            } else if (arr[mid] > num) {
                //如果要查找的值小于中间位置的值
                //说明要查找的值在左边部分
                right = mid - 1;
            } else {
                binlist.add(mid);
                int leftmid = mid - 1;
                while (leftmid > 0 && arr[leftmid] == num) {
                    binlist.add(leftmid);
                    leftmid--;
                }

                int rightmid = mid + 1;
                while (rightmid < right && arr[rightmid] == num) {
                    binlist.add(rightmid);
                    rightmid++;
                }
                return binlist;
            }
        }
        return binlist;
    }
}
```

#### 4、斐波那契查找(黄金分割法)

**查找思路**

对于斐波那契数列：1、1、2、3、5、8、13、21、34、55、89……（也可以从0开始），**前后两个数字的比值**随着数列的增加，**越来越接近黄金比值0.618**。比如这里的89，把它想象成整个有序表的元素个数，而89是由前面的两个斐波那契数34和55相加之后的和，也就是说把元素个数为89的有序表分成由前55个数据元素组成的前半段和由后34个数据元素组成的后半段，那么前半段元素个数和整个有序表长度的比值就接近黄金比值0.618，假如要查找的元素在前半段，那么继续按照斐波那契数列来看，55 = 34 + 21，所以继续把前半段分成前34个数据元素的前半段和后21个元素的后半段，继续查找，如此反复，直到查找成功或失败，这样就把斐波那契数列应用到查找算法中了。如下图：

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.1s79eyt8rd5s.3gphczu1plo0.png)

**举例**

比如有一个数组arr={1，2，3，4，5，6，7，8，9，10，11，12}要对他进行斐波那契查找，查找的值是10

首先，你得创建一个斐波那契数列出来我们定义为f[k],长度暂且定义为10吧那f={1,1,2,3,5,8,13,21,34,55},创建好了之后，我们再看原数组长度arr.length=12，根据斐波那契查找原则我们发现他的长度不等于斐波那契数列的某一个数值，所以我们要将数组的长度补至最近的斐波那契数，好，我们最近的值是13，所以我们复制最后一个元素至arr数组末尾(当然，数组长度是不能改变的，我们只能创建一个新的数组来复制arr数组的值并复制最后一个元素添加到末尾)，好，新的数组元素就是{1，2，3，4，5，6，7，8，9，10，11，12，12}

接下来得找查找算法里的中间值了，斐波那契查找发就是讲原序列分为斐波那契数组里的连续的两个值，上面我们说了原序列长度为13，在斐波那契数列里找到f[k]=f[k-1]+f[k-2],即13=8+5,f[6]=f[5]+f[4],则中间值就是f[5]=8

找到中间值之后下来就是递归了，将目标值和中间值进行比较，如果目标值小于中间值，说明向左递归，将左边的部分继续分解为两个斐波那契数，以此类推直到找到目标数

```java
public class FibonaSearch {
    public static void main(String[] args) {
        int[] arr = {-1, 8, 7, 8, 9, 10, 29, 27};
        Arrays.sort(arr);
        for(int i: arr){
            System.out.print(i+ " ");
        }
        System.out.println();
        int num = fibSearch(arr, 0);
        if(num == -1) {
            System.out.println("num is't finding");
        }else {
            System.out.println("num is " + num);
        }
    }

    /**
     *
     * @param maxsize
     * @return f
     */
    public static int[] fib(int maxsize){
        int[] f = new int[maxsize];
        f[0] = 1;
        f[1] = 1;
        for (int i =2; i<maxsize;i++){
            f[i] = f[i-1] +f[i-2];
        }
        return f;
    }
    //编写斐波那契查找算法
    //使用非递归的方式编写算法
    public static int fibSearch(int[] arr, int key){
        int low = 0;
        int high = arr.length -1;
        int k = 0;
        int mid;
        int []f = fib(20);
        while (high > f[k] - 1){
            k++;
        }

        int[] temp = Arrays.copyOf(arr, f[k]);

        for (int i = high; i<temp.length;i++){
            temp[i] = temp[high];
        }

        while (low <= high){
            mid = low + f[k-1] - 1;
            if (key < arr[mid]){
                high = mid - 1;
                //为甚是 k--
                //说明
                //1. 全部元素 = 前面的元素 + 后边元素
                //2. f[k] = f[k-1] + f[k-2]
                //因为 前面有 f[k-1]个元素,所以可以继续拆分 f[k-1] = f[k-2] + f[k-3]
                //即 在 f[k-1] 的前面继续查找 k--
                //即下次循环 mid = f[k-1-1]-1
                k--;
            }else if (key > arr[mid]){
                low = mid +1;
                //为什么是k -=2
                //说明
                //1. 全部元素 = 前面的元素 + 后边元素
                //2. f[k] = f[k-1] + f[k-2]
                //3. 因为后面我们有f[k-2] 所以可以继续拆分 f[k-1] = f[k-3] + f[k-4]
                //4. 即在f[k-2] 的前面进行查找 k -=2
                //5. 即下次循环 mid = f[k - 1 - 2] - 1
                k -= 2;
                k -=2;
            }else{
                if (mid<=high){
                    return mid;
                }else {
                    return high;
                }
            }
        }
        return -1;
    }

}
```

## 七、哈希表

#### 1、基本介绍

散列表（Hash table，也叫哈希表），是根据**关键码值**(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.1vwohmy3sj8g.png" alt="image" style="zoom:80%;" />

所以由示意图可以看出哈希表是一种将**数组与链表相结合**的数据结构

**代码**

```java
public class Hash {
    public static void main(String[] args) {
        Stdudent student1 = new Stdudent(1, "Nyima");
        Stdudent student2 = new Stdudent(2, "Lulu");
        Stdudent student6 = new Stdudent(6, "WenWen");

        HashTable abc = new HashTable(5);
        abc.add(student1);
        abc.add(student2);
        abc.add(student6);
        abc.travel();
        abc.find(6);
    }

}


class Stdudent{
    int id;
    String name;
    Stdudent next;

    public Stdudent(int id, String name){
        this.name = name;
        this.id =id;
    }

    @Override
    public String toString() {
        return "stdudent{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", next=" + next +
                '}';
    }
}

class linkelist{
    private Stdudent head = new Stdudent(-1, "");

    /**
     *
     * @param node
     */
    public void addStudent(Stdudent node){
        if (head.next == null){
            head.next = node;
            return;
        }

        Stdudent temp = head;
        while (temp.next != null){
            temp = temp.next;
        }
        temp.next = node;
    }


    public void delate(Stdudent node){
        if (head.next == null){
            System.out.println("null link");
            return;
        }
        Stdudent temp = head;
        while (temp.next!=null && temp.next.id != node.id){
            temp = temp.next;
        }
        if(temp.next.id != node.id){
            System.out.println("dont find");
            return;
        }
        temp.next = temp.next.next;
    }

    public void travel(){
        if (head.next == null){
            System.out.println("null link");
            return;
        }
        Stdudent temp = head;
        while (temp.next != null){
            temp = temp.next;
            System.out.print(temp + "");
        }
        System.out.println();
    }
    // 查学生id
    public void find(int id){
        if (head.next == null){
            System.out.println("null link");
            return;
        }
        Stdudent temp = head;
        while (temp.next != null){
            temp = temp.next;
            if (temp.id == id){
                System.out.println("this is" + temp);
                return;
            }
        }
        System.out.println("dont find");
    }
}

class HashTable{
   private linkelist[] list;
   private int size;


   public HashTable(int size){
       this.size = size;
       list = new linkelist[size];
       for (int i = 0; i < this.size; i++) {
           list[i] = new linkelist();
       }
   }


   public void add(Stdudent node){
        int hashId = gethash(node.id);
        list[hashId].addStudent(node);
   }


   public void travel(){
       for (int i = 0; i < size; i++) {
           list[i].travel();
       }
   }


   public void find(int id){
       int hashId = gethash(id);
       list[hashId].find(id);
   }


   private int gethash(int id){
       return id%size;
   }

}
```

## 八、树结构

#### 1、二叉树

**为什么需要树**

- 数组的查找效率高，但是插入效率低。
- 链表的插入效率高，查找效率低。

需要一种数据结构来平衡查找与插入效率，使得查找速度和插入速度都能得到提升，因此有了树这种数据结构。

#### 2、树的基本概念

树是一种非线性的数据结构，它是由n（n>=0）个有限结点组成一个具有层次关系的集合。把它叫做树是因
为它看起来像一棵倒挂的树，也就是说它是根朝上，而叶朝下的。

根结点：根节点没有前驱结点。
除根节点外，其余结点被分成是一棵结构与树类似的子树。每棵子树的根结点有且只有一个前驱，可以有0个或多个后继。
因此，树是递归定义的

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.3ghwf2thmug0.png)

- 节点的度：一个节点含有的子树的个数称为该节点的度； 如上图：A的为2
- 叶节点：度为0的节点称为叶节点； 如上图：G、H、I节点为叶节点
- 非终端节点或分支节点：度不为0的节点； 如上图：B、D、C、E、F节点为分支节点
- 双亲节点或父节点：若一个节点含有子节点，则这个节点称为其子节点的父节点； 如上图：A是B的父节点
- 孩子节点或子节点：一个节点含有的子树的根节点称为该节点的子节点； 如上图：B是A的孩子节点
- 兄弟节点：具有相同父节点的节点互称为兄弟节点； 如上图：B、C是兄弟节点
- 树的度：一棵树中，最大的节点的度称为树的度； 如上图：树的度为2
- 节点的层次：从根开始定义起，根为第1层，根的子节点为第2层，以此类推；
- 树的高度或深度：树中节点的最大层次； 如上图：树的高度为4
- 堂兄弟节点：双亲在同一层的节点互为堂兄弟；如上图：H、I互为兄弟节点
- 节点的祖先：从根到该节点所经分支上的所有节点；如上图：A是所有节点的祖先
- 子孙：以某节点为根的子树中任一节点都称为该节点的子孙。如上图：所有节点都是A的子孙
- 森林：由m棵互不相交的树的集合称为森林；

#### 3、二叉树的基本概念

每个节点**最多只能有两个子节点**的一种形式称为二叉树

- 满二叉树：一个二叉树，如果每一个层的结点数都达到最大值，则这个二叉树就是满二叉树。也就是说，如果一个二叉树的层数为K，且结点总数是(2^k) -1 ，则它就是满二叉树。
- 完全二叉树：完全二叉树是效率很高的数据结构，完全二叉树是由满二叉树而引出来的。对于深度为K的，有n个结点的二叉树，当且仅当其每一个结点都与深度为K的满二叉树中编号从1至n的结点一一对应时称之为完全二叉树。 要注意的是满二叉树是一种特殊的完全二叉树。

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.nwvx939m2e8.png)

**二叉树的性质**

1. 若规定根节点的层数为1，则一棵非空二叉树的第i层上最多有2^(i-1) 个结点.
2. 若规定根节点的层数为1，则深度为h的二叉树的最大结点数是2^h- 1.
3. 对任何一棵二叉树, 如果度为0其叶结点个数为 n0, 度为2的分支结点个数为 n2,则有n0＝n2＋1
4. 若规定根节点的层数为1，具有n个结点的满二叉树的深度，h=Log2(n+1). (ps：Log2(n+1)是log以2为
   底，n+1为对数)
5. 对于具有n个结点的完全二叉树，如果按照从上至下从左至右的数组顺序对所有节点从0开始编号，则对
   于序号为i的结点有：

**代码**

```java
public class BinaryTree {
    public static void main(String[] args) {
        hero root = new hero(1, "songjiang");
        hero hero1 = new hero(2, "lujunyi");
        hero hero2 = new hero(3, "linchong");
        hero hero3 = new hero(4, "shijin");
        hero hero4 = new hero(4, "wangying");

        root.setLeft(hero1);
        hero1.setLeft(hero2);
        root.setRight(hero3);
        hero1.setRight(hero4);

        Binary tree = new Binary(root);
        System.out.println("-------qianxu---------");
        tree.preTraverse();
        System.out.println("-------zhongxu---------");
        tree.midTraverse();
        System.out.println("-------houxu---------");
        tree.lastTraverse();
    }
}

/**
 * 定义二叉树的节点
 */
class hero{
    private int  id;
    private String name;
    hero left;
    hero right;

    public hero(int id, String name) {
        this.id = id;
        this.name = name;
    }

    // get set 方法

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setLeft(hero left) {
        this.left = left;
    }

    public void setRight(hero right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "hero{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
    /**
     * 前序遍历，中序遍历，后序遍历
     */
    public void preTraverse(){
        // 根，左，右
        System.out.println(this);
        if (this.left != null){
            this.left.preTraverse();
        }
        if (this.right != null){
            right.preTraverse();
        }
    }

    public void midTraverse(){
        // 左，根，右
        if (this.left != null){
            this.left.midTraverse();
        }
        System.out.println(this);
        if (this.right != null){
            right.midTraverse();
        }
    }


    public void lastTraverse(){
        // 左，根，右
        if (this.left != null){
            this.left.lastTraverse();
        }
        if (this.right != null){
            right.lastTraverse();
        }
        System.out.println(this);
    }
}
class Binary{
    private hero root;
    public Binary(hero root){
        this.root = root;
    }

    public void preTraverse(){
        if (root != null){
            System.out.println("traverl");
            root.preTraverse();
        }else {
            System.out.println("null");
        }
    }

    public void midTraverse(){
        if (root != null){
            System.out.println("midtraverl");
            root.midTraverse();
        }else {
            System.out.println("null");
        }
    }

    public void lastTraverse(){
        if (root != null){
            System.out.println("midtraverl");
            root.lastTraverse();
        }else {
            System.out.println("null");
        }
    }
}
```

#### 4、二叉树的查找

前、中、后序查找的思路与遍历相似，当找到对应的元素时，直接返回即可。

**代码实现**

```java
public class Demo2 {
   public static void main(String[] args) {
      //创建根节点
      BinarySearchTree tree = new BinarySearchTree();

      //手动创建节点
      Student student1 = new Student(1, "A");
      Student student2 = new Student(2, "B");
      Student student3 = new Student(3, "C");
      Student student4 = new Student(4, "D");
      Student student5 = new Student(5, "E");
      student1.setLeft(student2);
      student1.setRight(student3);
      student2.setLeft(student4);
      student3.setRight(student5);

      //指定根节点
      tree.setRoot(student1);

      //查找
      tree.preSearch(3);
      tree.midSearch(4);
      tree.lastSearch(7);

   }
}

class BinarySearchTree {
   private Student root;

   public void setRoot(Student root) {
      this.root = root;
   }

   public void preSearch(int id) {
      System.out.println("前序查找");
      if(root == null) {
         System.out.println("树为空！");
         return;
      }
      Student result = root.preSearch(id);
      if(result == null) {
         System.out.println("未找到该元素");
         System.out.println();
         return;
      }
      System.out.println(result);
      System.out.println();
   }

   public void midSearch(int id) {
      System.out.println("中序查找");
      if(root == null) {
         System.out.println("树为空！");
         return;
      }
      Student result = root.midSearch(id);
      if(result == null) {
         System.out.println("未找到该元素");
         System.out.println();
         return;
      }
      System.out.println(result);
      System.out.println();
   }

   public void lastSearch(int id) {
      System.out.println("后序查找");
      if(root == null) {
         System.out.println("树为空！");
         return;
      }
      Student result = root.lastSearch(id);
      if(result == null) {
         System.out.println("未找到该元素");
         System.out.println();
         return;
      }
      System.out.println(result);
      System.out.println();
   }
}

class Student {
   int id;
   String name;
   Student left;
   Student right;

   public Student(int id, String name) {
      this.id = id;
      this.name = name;
   }

   public int getId() {
      return id;
   }

   public void setId(int id) {
      this.id = id;
   }

   public String getName() {
      return name;
   }

   public void setName(String name) {
      this.name = name;
   }

   public Student getLeft() {
      return left;
   }

   public void setLeft(Student left) {
      this.left = left;
   }

   public Student getRight() {
      return right;
   }

   public void setRight(Student right) {
      this.right = right;
   }

   @Override
   public String toString() {
      return "Student{" +
            "id=" + id +
            ", name='" + name + '\'' +
            '}';
   }

   /**
    * 前序查找
    * @param id 要查找的学生id
    * @return 查找到的学生
    */
   public Student preSearch(int id) {
      //如果找到了，就返回
      if(this.id == id) {
         return this;
      }

      //在左子树中查找，如果找到了就返回
      Student student = null;
      if(left != null) {
         student = left.preSearch(id);
      }
      if(student != null) {
         return student;
      }

      //在右子树中查找，无论是否找到，都需要返回
      if(right != null) {
         student = right.preSearch(id);
      }
      return student;
   }

   /**
    * 中序查找
    * @param id 要查找的学生id
    * @return 查找到的学生
    */
   public Student midSearch(int id) {
      Student student = null;
      if(left != null) {
         student = left.midSearch(id);
      }
      if(student != null) {
         return student;
      }

      //找到了就返回
      if(this.id == id) {
         return this;
      }

      if(right != null) {
         student = right.midSearch(id);
      }
      return student;
   }

   /**
    * 后序查找
    * @param id 要查找的学生id
    * @return 查找到的学生
    */
   public Student lastSearch(int id) {
      Student student = null;
      if(left != null) {
         student = left.lastSearch(id);
      }
      if(student !=null) {
         return student;
      }

      if(right != null) {
         student = right.lastSearch(id);
      }

      if(this.id == id) {
         return this;
      }

      return student;
   }
}
```

#### 5、二叉树的删除

**删除要求**

- 如果删除的是叶子节点，则直接删除即可
- 如果删除的是非叶子节点，则**删除该子树**

**删除思路**

- 因为我们的二叉树是**单向的**，所以我们是判断**当前结点的子结点**是否需要删除结点，而不能去判断当前这个结点是不是需要删除结点
- 如果当前结点的**左子结点不为空**，并且左子结点就是要删除结点，就将 **this.left = null**; 并且就返回 (结束递归删除)
- 如果当前结点的**右子结点不为空**，并且右子结点就是要删除结点，就将 **this.right= null** ;并且就返回 (结束递归删除)
- 如果第2和第3步没有删除结点，那么我们就需要向左子树进行递归删除
- 如果 4步也没有删除结点，则应当向右子树进行递归删除

**代码**

```java 
public class Demo3 {
   public static void main(String[] args) {
      //创建根节点
      BinaryDeleteTree deleteTree = new BinaryDeleteTree();

      //手动创建节点
      StudentNode student1 = new StudentNode(1, "A");
      StudentNode student2 = new StudentNode(2, "B");
      StudentNode student3 = new StudentNode(3, "C");
      StudentNode student4 = new StudentNode(4, "D");
      StudentNode student5 = new StudentNode(5, "E");
      student1.setLeft(student2);
      student1.setRight(student3);
      student2.setLeft(student4);
      student3.setRight(student5);

      //指定根节点
      deleteTree.setRoot(student1);

      //删除节点
      deleteTree.deleteNode(3);
   }
}


class BinaryDeleteTree {
   StudentNode root;

   public void setRoot(StudentNode root) {
      this.root = root;
   }

   /**
    * 删除节点
    * @param id 删除节点的id
    */
   public void deleteNode(int id) {
      System.out.println("删除节点");
      if(root.id == id) {
         root = null;
         System.out.println("根节点被删除");
         return;
      }
      //调用删除方法
      root.deleteNode(id);
   }
}

class StudentNode {
   int id;
   String name;
   StudentNode left;
   StudentNode right;

   public StudentNode(int id, String name) {
      this.id = id;
      this.name = name;
   }

   public int getId() {
      return id;
   }

   public void setId(int id) {
      this.id = id;
   }

   public String getName() {
      return name;
   }

   public void setName(String name) {
      this.name = name;
   }

   public StudentNode getLeft() {
      return left;
   }

   public void setLeft(StudentNode left) {
      this.left = left;
   }

   public StudentNode getRight() {
      return right;
   }

   public void setRight(StudentNode right) {
      this.right = right;
   }

   @Override
   public String toString() {
      return "StudentNode{" +
            "id=" + id +
            ", name='" + name + '\'' +
            '}';
   }

   /**
    * 删除节点
    * @param id 删除节点的id
    */
   public void deleteNode(int id) {
      //如果左子树不为空且是要查找的节点，就删除
      if(left != null && left.id == id) {
         left = null;
         System.out.println("删除成功");
         return;
      }

      //如果右子树不为空且是要查找的节点，就删除
      if(right != null && right.id == id) {
         right = null;
         System.out.println("删除成功");
         return;
      }

      //左递归，继续查找
      if(left != null) {
         left.deleteNode(id);
      }

      //右递归，继续查找
      if(right != null) {
         right.deleteNode(id);
      }
   }
}
```

#### 6、顺序存储二叉树

**基本说明**

从数据存储来看，数组存储方式和树的存储方式可以相互转换，即**数组可以转换成树**，**树也可以转换成数组**

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.47viwnzs11e0.png" alt="image" style="zoom:67%;" />

**特点**

- 顺序二叉树通常只考虑**完全二叉树**

- 第 n个元素的**左**子节点为 **2 × n + 1**

- 第 n个元素的**右**子节点为 **2 × n + 2**

- 第 n个元素的父节点为(n-1) ÷2
  - 其中**n** 表示二叉树中的第几个元素(从0开始编号)

遍历过程和二叉树的遍历类似，只不过递归的条件有所不同

**代码**

```java 
public class ArrayBinaryTree {
    public static void main(String[] args) {
        int[] arr = {1,2,3,4,5,6,7};
        ArrayBinary haha = new ArrayBinary(arr);

        haha.preOder();
    }
}

class ArrayBinary{
    private int[] arr;

    public ArrayBinary(int[] arr){
        this.arr = arr;
    }
    public void preOder(){
        this.preOder(0);
    }

    public void preOder(int index){
        if(arr == null || arr.length == 0){
            System.out.println("arr is empty");
            return;
        }
        System.out.println(arr[index]);
        if ((2*index + 1) < arr.length){
            preOder(2*index+1);
        }
        if ((2*index + 2) < arr.length){
            preOder(2*index+2);
        }
    }
}
```

 

#### 7、索化二叉树

**基本概念**

------

因为一般的二叉树，叶子节点的左右指针都为空，这样就会造成**空间的浪费**。为了减少浪费，便有了线索化二叉树

- n个结点的二叉链表中含有 n+1 【公式 2n-(n-1)=n+1】个空指针域。利用二叉链表中的空指针域，存放指向该结点在**某种遍历次序**下的前驱和后继结点的指针
- 这种加上了线索的二叉链表称为线索链表，相应的二叉树称为线索二叉树
- 根据线索性质的不同，线索二叉树可分为**前序**线索二叉树、**中序**线索二叉树和**后序**线索二叉树三种
- 如果一个节点已经有了左右孩子，那么该节点就不能被线索化了，所以线索化二叉树后，节点的left和right有如下两种情况
  - left可能指向的是左孩子，也可能指向的是前驱节点
  - right可能指向的是右孩子，也可能指向的是后继节点

**图解**

**中序线索化前**

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.516ri2bsdlc0.png" alt="image" style="zoom:80%;" />

**中序线索化后**

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.67k9lprecak0.png" alt="image" style="zoom:80%;" />

**实现代码**

**线索化思路**

- 每个节点需要用两个变量来表示**左右指针的类型**（保存左右子树，还是前驱后继）
- 需要用两个变量来表示**当前节点**和当前节点的**前驱节点**
- 通过将**当前节点的左指针**指向前驱节点，来实现前驱节点的绑定
- 通过将**前驱节点的右指针**指向当前节点，来实现后继节点的绑定

**遍历方式**

- 各个节点可以通过**线性**的方式遍历，无需使用递归的方式遍历
- 首先有一个外循环，代替递归操作，循环条件的暂存节点不为空
- 第一个内循环用于找到第一个元素，然后打印
- 第二个循环用于找到节点的后继元素
- 最后将暂存节点令为右孩子

```java
public class Demo1 {
	public static void main(String[] args) {
		//初始化节点
		Student student1 = new Student(1, "1");
		Student student2 = new Student(2, "3");
		Student student3 = new Student(3, "6");
		Student student4 = new Student(4, "8");
		Student student5 = new Student(5, "10");
		Student student6 = new Student(6, "14");

		//手动创建二叉树
		ThreadedBinaryTree tree = new ThreadedBinaryTree();
		student1.setLeft(student2);
		student1.setRight(student3);
		student2.setLeft(student4);
		student2.setRight(student5);
		student3.setLeft(student6);

		tree.setRoot(student1);

		tree.midThreaded();

		//得到第五个节点的前驱节点和后继节点
		System.out.println("第五个节点的前驱节点和后继节点");
		System.out.println(student5.getLeft());
		System.out.println(student5.getRight());

		//遍历线索化二叉树
		System.out.println("遍历线索化二叉树");
		tree.midThreadedTraverse();

	}
}

class ThreadedBinaryTree {
	private Student root;
	/**
	 * 指向当前节点的前一个节点
	 */
	private Student pre;

	public void setRoot(Student root) {
		this.root = root;
	}

	/**
	 * 中序线索化
	 * @param node 当前节点
	 */
	private void midThreaded(Student node) {
		if(node == null) {
			return;
		}
		//左线索化
		midThreaded(node.getLeft());
        
		//线索化当前节点
		//如果当前节点的左指针为空，就指向前驱节点，并改变左指针类型
		if(node.getLeft() == null) {
			node.setLeft(pre);
			node.setLeftType(1);
		}
		//通过前驱节点来将右指针的值令为后继节点
		if(pre != null && pre.getRight() == null) {
			pre.setRight(node);
			pre.setRightType(1);
		}

		//处理一个节点后，让当前节点变为下一个节点的前驱节点
		pre = node;

		//右线索化
		midThreaded(node.getRight());
	}

	public void midThreaded() {
		midThreaded(root);
	}

	/**
	 * 遍历线索化后的二叉树
	 */
	public void midThreadedTraverse() {
		//暂存遍历到的节点
		Student tempNode = root;
		//非递归的方法遍历，如果tempNode不为空就一直循环
		while(tempNode != null) {
			//一直访问二叉树的左子树，直到某个节点的左子树指向前驱节点
			while(tempNode.getLeftType() != 1) {
				tempNode = tempNode.getLeft();
			}
			//找到了第一个节点
			System.out.println(tempNode);
			//再访问该节点的右子树，看是否保存了后继节点
			//如果是，则打印该节点的后继节点信息
			while(tempNode.getRightType() == 1) {
				tempNode = tempNode.getRight();
				System.out.println(tempNode);
			}

			tempNode = tempNode.getRight();
		}

	}
}

class Student {
	private int id;
	private String name;
	private Student left;
	private Student right;
	/**
	 * 左、右指针的类型，0-->指向的是左右孩子，1-->指向的是前驱、后续节点
	 */
	private int leftType = 0;
	private int rightType = 0;

	public Student(int id, String name) {
		this.id = id;
		this.name = name;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Student getLeft() {
		return left;
	}

	public void setLeft(Student left) {
		this.left = left;
	}

	public Student getRight() {
		return right;
	}

	public void setRight(Student right) {
		this.right = right;
	}

	public int getLeftType() {
		return leftType;
	}

	public void setLeftType(int leftType) {
		this.leftType = leftType;
	}

	public int getRightType() {
		return rightType;
	}

	public void setRightType(int rightType) {
		this.rightType = rightType;
	}

	@Override
	public String toString() {
		return "Student{" +
				"id=" + id +
				", name='" + name + '\'' +
				'}';
	}
}
```

#### 8、树的应用

##### 堆排序

**基本介绍**

- 堆排序是利用堆这种数据结构而设计的一种排序算法，堆排序是一种选择排序，它的最坏，最好，平均时间复杂度均为 **O(nlog^n^)**，它也是不稳定排序

- 堆是具有以下性质的**完全二叉树**：

  - 每个结点的值都大于或等于其左右孩子结点的值，称为

    大顶堆

    - 注意 : **没有要求结点的左孩子的值和右孩子的值的大小关系**

  - 每个结点的值都小于或等于其左右孩子结点的值，称为**小顶堆**

- 一般升序排序采用大顶堆，降序排列使用小顶堆

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.7gizeo99rhg.png" alt="image" style="zoom:80%;" />

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.3yy1g53l4bo0.png" alt="image" style="zoom:80%;" />

**排序思路**

- 堆是一种树结构，但是排序中会将堆进行**顺序存储**（变为数组结构）
- 将无序序列构建成一个堆，根据升序降序需求选择大顶堆或小顶堆
- 将堆顶元素与末尾元素交换，将最大(最小)元素"沉"到数组末端
- 重新调整结构，使其满足堆定义，然后继续交换堆顶元素与当前末尾元素，反复执行调整+交换步骤，直到整个序列有序

**图解**

![20180908013007479](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/20180908013007479.1wyzdocy869s.gif)

```java
public class HeapSort {
    public static void main(String []args){
        int []arr = {8, 9 ,6, 5, 16, 0};
        System.out.println("before sort "+Arrays.toString(arr));
        sort(arr);
        System.out.println("after sort  "+Arrays.toString(arr));
    }

    public static void sort(int []arr){
        //1.构建大顶堆
        for(int i=arr.length/2-1;i>=0;i--){
            //从第一个非叶子结点从下至上，从右至左调整结构
            adjustHeap(arr,i,arr.length);
        }
        //2.调整堆结构+交换堆顶元素与末尾元素
        for(int j=arr.length-1;j>0;j--){
            swap(arr,0,j);//将堆顶元素与末尾元素进行交换
            adjustHeap(arr,0,j);//重新对堆进行调整
        }

    }

    /**
     * 调整大顶堆（仅是调整过程，建立在大顶堆已构建的基础上）
     * @param arr
     * @param i
     * @param length
     */
    public static void adjustHeap(int []arr,int i,int length){
        int temp = arr[i];//先取出当前元素i
        for(int k=i*2+1;k<length;k=k*2+1){//从i结点的左子结点开始，也就是2i+1处开始
            if(k+1<length && arr[k]<arr[k+1]){//如果左子结点小于右子结点，k指向右子结点
                k++;
            }
            if(arr[k] >temp){//如果子节点大于父节点，将子节点值赋给父节点（不用进行交换）
                arr[i] = arr[k];
                i = k;
            }else{
                break;
            }
        }
        arr[i] = temp;//将temp值放到最终的位置
    }

    /**
     * 交换元素
     * @param arr
     * @param a
     * @param b
     */
    public static void swap(int []arr,int a ,int b){
        int temp=arr[a];
        arr[a] = arr[b];
        arr[b] = temp;
    }
}
```

##### 哈夫曼树

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.6hbe0nnd2j00.png" alt="image" style="zoom:80%;" />

**基本介绍**

- 给定 n个权值作为 n个叶子结点，构造一棵二叉树，**若该树的带权路径长度(wpl)达到最小**，称这样的二叉树为最优二叉树，也称为哈夫曼树(Huffman Tree)
- 哈夫曼树是带权路径长度最短的树，**权值较大的结点离根较近**

**重要概念**

- **路径和路径长度**：在一棵树中，从一个结点往下可以达到的孩子或孙子结点之间的通路，称为路径。**通路中分支的数目称为路径长度**。若规定根结点的层数为1，则从根结点到第L层结点的路径长度为 **L-1**
- 结点的权及带权路径长度：若将树中结点赋给一个有着某种含义的数值，则这个数值称为该结点的权
  - **结点的带权路径长度为**：从根结点到该结点之间的路径长度与该结点的权的**乘积**（W×L）
- **树的带权路径长度**：树的带权路径长度规定为**所有叶子结点的带权路径长度之和**（W1×L1+W2×L2…），记为WPL(weighted pathlength) ,权值越大的结点离根结点越近的二叉树才是最优二叉树。
- WPL最小的就是哈夫曼树

**创建思路** 

1、从小到大进行排序, 将每一个数据，每个数据都是一个节点 ， 每个节点可以看成是一颗最简单的二叉树

2、取出**根节点权值最小**的两颗二叉树

3、组成一颗新的二叉树, 该新的二叉树的根节点的权值是前面两颗二叉树**根节点权值的和**

4、再将这颗新的二叉树，以**根节点的权值大小 再次排序**，不断重复 1-2-3-4 的步骤，直到数列中，所有的数据都被处理，就得到一颗赫夫曼树

以{5,6,7,8,15}为例，来构造一棵哈夫曼树。

<img src="https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.398obh4qi240.png" alt="image" style="zoom:80%;" />

**代码实现**

```java
public class HaffmanTree {
    public static void main(String[] args) {
        int[] arr = {3, 6, 7, 1, 8, 29, 13};
        Haffman haff = new Haffman();
        Node root = haff.creatHaffmanTree(arr);
        if (root != null) {
            root.preOder();
        }else {
            System.out.println("The tree is epmty");
        }
    }
}

class Haffman{
    public Node creatHaffmanTree(int []arr) {
        ArrayList<Node> nodes = new ArrayList<>(arr.length);
        for (int value: arr){
            nodes.add(new Node(value));
        }

        while (nodes.size() > 1) {
            Collections.sort(nodes);

            Node left = nodes.get(0);
            Node right = nodes.get(1);

            Node parent = new Node(left.value + right.value);
            parent.left = left;
            parent.rgiht = right;

            nodes.remove(left);
            nodes.remove(right);
            nodes.add(parent);
        }
        return nodes.get(0);
    }
}

class Node implements Comparable<Node> {
    int value;
    Node left;
    Node rgiht;

    public Node(int value){
        this.value = value;
    }

    public void preOder(){
        System.out.print(this.value + " ");

        if (this.left != null){
            this.left.preOder();
        }
        if (this.rgiht != null){
            this.rgiht.preOder();
        }
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }

    @Override
    public int compareTo(Node o) {
        return this.value - o.value;
    }
}
```

#####   **哈夫曼编码**

#### 原理及图解

**前缀编码**：任何一个字符的编码，都不会是其它的字符的前缀，哈夫曼编码是前缀编码。

- 统计需编码的字符串中，各个字符出现的次数：如helloworld
  - h:1 e:1 w:1 r:1 d:1 o:2 l:3
- 将字符出现的次数作为权值，构建哈弗曼树。如下图

![image](https://cdn.jsdelivr.net/gh/RbRookie/image-management@master/20210423/image.rg786c3h0e8.png)

- 根据哈弗曼树进行编码，向左的路径为0，向右的路径为1
  - 字符编码结果为：h:000 e:001 w:100 d:1010 r:1011 l:11 o:01
  - 字符串编码结果为：000001111101100011011111010

#### 实现代码

此处代码只实现了哈弗曼树的创建与每个字符的编码
