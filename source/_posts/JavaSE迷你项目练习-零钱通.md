---
title: JavaSE 迷你项目练习-零钱通
date: 2025-05-07 15:33:34
tags: JavaSE
categories: 项目练习 
---

# 项目 - 零钱通

## 介绍

- 项目需求说明：使用Java开发零钱通项目，可以完成收益入账、消费、查看明细、退出系统等功能。

## 面向过程的代码

```java
package com.smallchange;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Scanner;

public class smallChangeSys {
    /**
     * 化繁为简
     * 1.先完成显示菜单，并可以选择菜单，给出对应提示
     * 2.完成零钱通明细
     * 3.完成收益入账
     * 4.完成消费
     * 5.退出
     * 6.用户输入4退出时，给出提示“你确定要退出吗？y/n”。必须输入正确的y/n，
     *   否则循环输入指令，知道输入y/n
     * 7.在收益入账和消费时，判断金额是否合理，并给出相应的提示
     */

    public static void main(String[] args) {
        boolean loop = true;
        Scanner scanner = new Scanner(System.in);
        String key = "";

        /**
         * 完成零钱通明细
         *  思路：
         *   （1）可以把收益入账和消费保存到数组
         *   （2）可以使用对象
         *   （3）简单的话可以使用String拼接
         */
        String details = "========零钱通明细========";

        /**
         * 完成收益入账
         *  思路：定义新的变量，
         */
        double money = 0;
        double balance = 0;
        Date date = null;   //date是java.util.Date类型，表示日期
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm");    //可以用于日期格式化的对象

        /**
         * 完成消费：
         *  思路：定义新变量，保存消费的原因
         */
        String note = "";
        do{
            System.out.println("========零钱通菜单========");
            System.out.println("\t\t1.零钱通明细");
            System.out.println("\t\t2.收益入账");
            System.out.println("\t\t3.消费");
            System.out.println("\t\t4.退出");

            System.out.print("请选择(1-4)：");
            key = scanner.next();

            //使用switch分支控制
            switch(key){
                case "1":
                    System.out.println(details);
                    break;
                case "2":
                    System.out.print("收益入账金额：");
                    money = scanner.nextDouble();
                    /**
                     * 在收益入账和消费时，判断金额是否合理，并给出相应的提示
                     * 思路：找出不正确金额的条件，给出提示后直接break
                     */
                    if(money <= 0){
                        System.out.println("收益入账金额需要大于0");
                        break;
                    }
                    balance += money;
                    //拼接收益入账信息到details
                    date = new Date();  //获取当前日期
                    details += "\n收益入账\t+" + money + "\t" + sdf.format(date) + "\t" + balance;
                    break;
                case "3":
                    System.out.print("消费金额：");
                    money = scanner.nextDouble();
                    /**
                     * 在收益入账和消费时，判断金额是否合理，并给出相应的提示
                     * 思路：找出不正确金额的条件，给出提示后直接break
                     */
                    if(money <= 0 || money > balance){
                        System.out.println("您的消费金额应该在 0 - " + balance + " 范围内");
                        break;
                    }
                    System.out.print("消费说明：");
                    note = scanner.next();
                    balance -= money;
                    //拼接消费信息到details
                    date = new Date();  //获取当前日期
                    details += "\n\t" + note + "\t-" + money + "\t" + sdf.format(date) + "\t" + balance;
                    break;
                case "4":
                    /**
                     * 用户输入4退出时，给出提示“你确定要退出吗？y/n”。必须输入正确的y/n，
                     * 否则循环输入指令，直到输入y/n
                     *  思路:
                     *  (1)定义变量choice，接收用户的输入
                     *  (2)使用while + break，来处理接收到的y/n
                     *  (3)当用户退出循环后，再判断choice是y还是n
                     *  (4)建议一段代码完成一个小功能，尽量不要混在一起
                     */
                    String choice = "";
                    while(true){    //要求用户必须输入y/n，否则就必须循环
                        System.out.println("你确定要退出吗？y/n");
                        choice = scanner.next();
                        if("y".equals(choice) || "n".equals(choice)){
                            break;
                        }
                    }
                    //当用户退出循环后，再判断
                    if("y".equals(choice)){
                        loop = false;
                    }
                    break;
                default:
                    System.out.println("菜单选择有误，请重新输入");
            }
        }while(loop);

        System.out.println("========退出了零钱通========");



    }
}
```

## 面向对象的代码

- 编写`SmallChangeSysOOP.java`类，并使用`SmallChangeSysApp.java`完成测试。

### SmallChangeSysOOP.java

```java
package com.smallchange.oop;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Scanner;

/**
 * 这类时完成零钱通各个功能的类
 * 使用OOP（面向对象编程)
 * 将各个功能对应一个方法
 */
public class SmallChangeSysOOP {
    //属性
    boolean loop = true;
    Scanner scanner = new Scanner(System.in);
    String key = "";

    /**
     * 完成零钱通明细
     *  思路：
     *   （1）可以把收益入账和消费保存到数组
     *   （2）可以使用对象
     *   （3）简单的话可以使用String拼接
     */
    String details = "========零钱通明细========";

    /**
     * 完成收益入账
     *  思路：定义新的变量，
     */
    double money = 0;
    double balance = 0;
    Date date = null;   //date是java.util.Date类型，表示日期
    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm");    //可以用于日期格式化的对象

    /**
     * 完成消费：
     *  思路：定义新变量，保存消费的原因
     */
    String note = "";

    //先显示菜单，并可以选择
    public void mainMenu(){
        do{
            System.out.println("========OOP零钱通菜单========");
            System.out.println("\t\t1.零钱通明细");
            System.out.println("\t\t2.收益入账");
            System.out.println("\t\t3.消费");
            System.out.println("\t\t4.退出");

            System.out.print("请选择(1-4)：");
            key = scanner.next();

            //使用switch分支控制
            switch(key){
                case "1":
                    this.detail();
                    break;
                case "2":
                    this.income();
                    break;
                case "3":
                    this.pay();
                    break;
                case "4":
                    this.exit();
                    break;
                default:
                    System.out.println("菜单选择有误，请重新输入");
            }
        }while(loop);
    }

    //完成零钱通明细
    public void detail(){
        System.out.println(details);
    }

    //完成收益入账
    public void income(){
        System.out.print("收益入账金额：");
        money = scanner.nextDouble();
        /**
         * 在收益入账和消费时，判断金额是否合理，并给出相应的提示
         * 思路：找出不正确金额的条件，给出提示后直接return
         */
        if(money <= 0){
            System.out.println("收益入账金额需要大于0");
            return; //退出方法，不再执行后面的代码
        }
        balance += money;
        //拼接收益入账信息到details
        date = new Date();  //获取当前日期
        details += "\n收益入账\t+" + money + "\t" + sdf.format(date) + "\t" + balance;
    }

    //完成消费
    public void pay(){
        System.out.print("消费金额：");
        money = scanner.nextDouble();
        /**
         * 在收益入账和消费时，判断金额是否合理，并给出相应的提示
         * 思路：找出不正确金额的条件，给出提示后直接return
         */
        if(money <= 0 || money > balance){
            System.out.println("您的消费金额应该在 0 - " + balance + " 范围内");
            return;
        }
        System.out.print("消费说明：");
        note = scanner.next();
        balance -= money;
        //拼接消费信息到details
        date = new Date();  //获取当前日期
        details += "\n\t" + note + "\t-" + money + "\t" + sdf.format(date) + "\t" + balance;
    }

    public void exit(){
        /**
         * 用户输入4退出时，给出提示“你确定要退出吗？y/n”。必须输入正确的y/n，
         * 否则循环输入指令，直到输入y/n
         *  思路:
         *  (1)定义变量choice，接收用户的输入
         *  (2)使用while + break，来处理接收到的y/n
         *  (3)当用户退出循环后，再判断choice是y还是n
         *  (4)建议一段代码完成一个小功能，尽量不要混在一起
         */
        String choice = "";
        while(true){    //要求用户必须输入y/n，否则就必须循环
            System.out.println("你确定要退出吗？y/n");
            choice = scanner.next();
            if("y".equals(choice) || "n".equals(choice)){
                break;
            }
        }
        //当用户退出循环后，再判断
        if("y".equals(choice)){
            loop = false;
        }
    }
}
```

### SmallChangeSysApp.java

```java
package com.smallchange.oop;

/**
 * 这里我们直接调用SmallChangeSysOOP对象，显示主菜单即可
 */
public class SmallChangeSysApp {
    public static void main(String[] args) {
        new SmallChangeSysOOP().mainMenu();
    }
}
```

