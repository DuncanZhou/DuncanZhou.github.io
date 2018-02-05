---
title: 四则运算表达式求值
tags: Stack
categories: Learning
---
### 表达式求值
> 对于表达式的求值,一般使用中缀表达式转后缀表达式后,对后缀表达式求值,因为对于后缀或者前缀表达式计算,计算的顺序都是唯一的.

#### 中缀表达式转后缀表达式的方法：
* 1.遇到操作数：直接输出（添加到后缀表达式中）
* 2.栈为空时，遇到运算符，直接入栈
* 3.遇到左括号：将其入栈
* 4.遇到右括号：执行出栈操作，并将出栈的元素输出，直到弹出栈的是左括号，左括号不输出。
* 5.遇到其他运算符：加减乘除：弹出所有优先级大于或者等于该运算符的栈顶元素，然后将该运算符入栈
* 6.最终将栈中的元素依次出栈，输出。
```
public ArrayList<String> SuffixExpression(ArrayList<String> string){
        ArrayList<String> suffix = new ArrayList<>();
        Stack<String> stack = new Stack<>();
        for(int i = 0; i < string.size(); i++){
            if(string.get(i).equals("*") || string.get(i).equals("/") || string.get(i).equals("(")){
                //push
                stack.push(string.get(i));
            }
            else if(string.get(i).equals("+") || string.get(i).equals("-")){
                while(!stack.isEmpty() && (stack.peek().equals("*") || stack.peek().equals("/")))
                {
                        suffix.add(stack.pop());
                }
                //push
                stack.push(string.get(i));
            }
            else if(string.get(i).equals(")"))
            {
                while(!stack.isEmpty() && !stack.peek().equals("("))
                    suffix.add(stack.pop());
                //pop (
                stack.pop();
            }else{
                suffix.add(string.get(i));
            }
        }
        //push all elements in stack
        while(!stack.isEmpty())
            suffix.add(stack.pop());
        return suffix;
    }
```
### 计算后缀表达式
遇到操作数压入栈,否则弹出两个操作数进行操作后再压入栈中
```
public int CalaculateSuffix(ArrayList<String> suffix){
        //when meet operator, pop two elements to calaculate
        Stack<String> stack = new Stack<>();
        for(int i = 0; i < suffix.size(); i++){
            if(!(suffix.get(i).equals("+") || suffix.get(i).equals("-") || suffix.get(i).equals("*") || suffix.get(i).equals("/")))
                //push into stack
                stack.push(suffix.get(i));
            else{
                //calucate
                String operator = suffix.get(i);
                String b = stack.pop();
                String a = stack.pop();
                String temp = String.valueOf(operate(a,b,operator));
                stack.push(temp);
            }
        }
        return Integer.valueOf(stack.pop());
    }
```

