# 问题描述
定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

```
import java.util.Stack;

public class Solution {

    public void push(int node) {}
    
    public void pop() {}
    
    public int top() {}
    
    public int min() {}
}
```
# 问题分析
- 条件：定义栈结构；时间复杂度为O(1)
- 目标：min函数返回栈中最小值

# 求解思路1 -- 两个栈实现
由于栈是FILO，如果要得到时间复杂度为O(1)的min函数，说明，栈顶元素就应该是最小值，那么最后放进去的应该是最小值。如果只用一个栈的话，需要将元素加进去之后还要讲当前栈中最小值也加进去。先用两个栈实现：一个用来存原始的元素序列，一个存当前压栈数列中的最小值。

比如，stack中依次入栈：{5, 3, 4, 10, 2, 12, 1, 8} ，则temp依次入栈 {5, 3, 3，3, 2, 2, 1, 1}

# 代码实现1 -- 两个栈实现

```
private static Stack<Integer> stack = new Stack<>();//存原始数据的栈
private static Stack<Integer> tempStack = new Stack<>();//存当前最小值的栈

private static int minValue = Integer.MAX_VALUE;//保存当前最小值

public static void push(int node) {
	stack.push(node);
	if(node < minValue){
		minValue = node;
		tempStack.push(minValue);
	}else{
		tempStack.push(minValue);
	}
	
}
public static void pop() {
	if(!stack.isEmpty() && !tempStack.isEmpty()){			
		tempStack.pop();
		stack.pop();
	}
}

//注意调用top()或者min()方法时两栈元素个数不一致问题
public static int top() {
	int tempTop = -1;
	if(!stack.isEmpty() && !tempStack.isEmpty()){
	   tempTop = stack.pop();
	   tempStack.pop();//两个栈保持一致
		push(tempTop); 
	}
	
	return tempTop;
}

public static int min() {
	int tempValue = -1;
	if(!stack.isEmpty() && !tempStack.isEmpty()){
		tempValue = tempStack.pop();
		int tempNode = stack.pop();
		push(tempNode);
	}
	
	return tempValue;
}
```

# 求解思路2 -- 一个栈实现
上面方法用了两个栈来实现，一个存元素，一个存最小值。这种方法用到了两个栈，当需要push或者pop栈的时候需要保证两栈一致，不然在调用top和min方法的时候会出现非预期结果。现在用一个栈实现：在加入每个元素之后还需加入当前栈中的最小值，也就是说需要入栈两次，先是元素后是最小值。

# 代码实现2 -- 一个栈实现

```
private static Stack<Integer> stack = new Stack<>();

private static int minValue = Integer.MAX_VALUE;//保存当前最小值
    
public static void push(int node){
	if(node < minValue){
		minValue = node;
	}
	stack.push(node);//要先加node再加最小值
	stack.push(minValue);
}
public static void pop(){
	if(!stack.isEmpty()){		
		stack.pop();//第一次将元素出栈
		stack.pop();//第二次将当前最小值出栈
	}
}
public static int top(){
	int tempTopValue = -1;
	if(!stack.isEmpty()){
		stack.pop();
		tempTopValue = stack.pop();
		push(tempTopValue);
	}
	
	return tempTopValue;
}
public static int min(){
	int tempMinValue = -1;
	if(!stack.isEmpty()){
		tempMinValue = stack.pop();
		int node = stack.pop();
		push(node);
	}
	return tempMinValue;
}
```
这种方法每次push或pop都需要两次操作，才能达到本题要求
# 测试用例
两个栈测试用例：
- {"PUSH5","MIN","TOP" "PUSH3", "PUSH4", "PUSH10", "PUSH2", "PUSH12", "PUSH1", "PUSH8"}
- {"PUSH5", "PUSH3", "PUSH4", "PUSH10", "PUSH2", "PUSH12", "PUSH1", "PUSH8"}
一个栈测试用例
- {"PSH3","MIN","PSH4","MIN","PSH2","MIN","PSH3","MIN","POP","MIN","POP","MIN","POP","MIN","PSH0","MIN"}
- {"PSH3","MIN","PSH4","MIN","PSH2","MIN","PSH3","MIN","POP","POP","TOP"}
- {"PSH3","MIN","PSH4","MIN","PSH2","MIN","POP","POP","TOP"}
