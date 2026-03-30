---
title: JavaSE迷你项目练习-坦克大战
date: 2026-03-03 14:42:59
tags: JavaSE
categories: 项目练习
---

# 项目-坦克大战

## 坦克大战1.0

### Tank.java

```java
package Chapter16.tankgame2;

public class Tank {
    private int x;//坦克的横坐标
    private int y;//坦克的纵坐标
    private int direct;//坦克方向
    private int speed = 1;

    public Tank(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    public int getY() {
        return y;
    }

    public void setY(int y) {
        this.y = y;
    }

    public int getDirect() {
        return direct;
    }

    public void setDirect(int direct) {
        this.direct = direct;
    }

    public int getSpeed() {
        return speed;
    }

    public void setSpeed(int speed) {
        this.speed = speed;
    }

    //上下左右移动方法
    public void moveUp(){
        y -= speed;
    }
    public void moveRight(){
        x += speed;
    }
    public void moveDown(){
        y += speed;
    }
    public void moveLeft(){
        x -= speed;
    }
}
```

### MyTank.java

```java
package Chapter16.tankgame2;

//自己的坦克

public class MyTank extends Tank {
    public MyTank(int x, int y) {
        super(x, y);
    }
}
```

### Enemy.java

```java
package Chapter16.tankgame2;

public class Enemy extends Tank{
    public Enemy(int x, int y) {
        super(x, y);
    }
}
```

### MyPanel.java

```java
package Chapter16.tankgame2;

//坦克大战的绘图区域

import javax.swing.*;
import java.awt.*;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.util.Vector;

//为了监听键盘事件，实现KeyListener
public class MyPanel extends JPanel implements KeyListener {
    //定义我的坦克
    MyTank myTank = null;
    //定义敌人的坦克，放入Vector
    Vector<Enemy> enemy = new Vector<>();
    int enemyTankSize = 3;

    public MyPanel() {
        myTank = new MyTank(100,100);//初始化自己的坦克
        myTank.setSpeed(2);
        //初始化敌人的坦克
        for(int i = 0;i < enemyTankSize;i++){
            Enemy enemyTank = new Enemy(100 * (i + 1),0);
            //设置方向
            enemyTank.setDirect(2);
            //加入
            enemy.add(enemyTank);
        }
    }

    @Override
    public void paint(Graphics g) {
        super.paint(g);
        g.fillRect(0,0,100,750);//填充矩形，默认黑色

        //画出玩家的坦克
        drawTank(myTank.getX(),myTank.getY(),g,myTank.getDirect(),1);
        //画出敌人的坦克，遍历Vector
        for(int i = 0;i < enemy.size();i++){
            //取出坦克
            Enemy enemyTank = enemy.get(i);
            drawTank(enemyTank.getX(),enemyTank.getY(),g,enemyTank.getDirect(),0);
        }

    }

    //画出坦克-封装方法
    /**
     * @param x 坦克左上角x坐标
     * @param y 坦克左上角y坐标
     * @param g 画笔
     * @param direct 坦克方向（上下左右） 0表示向上
     * @param type 坦克的类型
     **/
    public void drawTank(int x,int y,Graphics g,int direct,int type){
        switch (type){
            case 0://敌方的坦克
                g.setColor(Color.cyan);
                break;
            case 1://玩家的坦克
                g.setColor(Color.yellow);
                break;
        }

        //根据坦克方向，来绘制坦克
        switch (direct){
            case 0:
                g.fill3DRect(x,y,10,60,false);//画出坦克左边的轮子
                g.fill3DRect(x + 30,y,10,60,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,20,40,false);//画出坦克盖子
                g.fillOval(x + 10,y + 20,20,20);//画出圆形盖子
                g.drawLine(x + 20,y,x +20,y + 30);//画出炮筒
                break;
            case 1:
                g.fill3DRect(x,y,60,10,false);//画出坦克左边的轮子
                g.fill3DRect(x,y + 30,60,10,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,40,20,false);//画出坦克盖子
                g.fillOval(x + 20,y + 10,20,20);//画出圆形盖子
                g.drawLine(x + 30,y + 20,x + 60,y + 20);//画出炮筒
                break;
            case 2:
                g.fill3DRect(x,y,10,60,false);//画出坦克左边的轮子
                g.fill3DRect(x + 30,y,10,60,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,20,40,false);//画出坦克盖子
                g.fillOval(x + 10,y + 20,20,20);//画出圆形盖子
                g.drawLine(x + 20,y + 60,x + 20,y + 30);//画出炮筒
                break;
            case 3:
                g.fill3DRect(x,y,60,10,false);//画出坦克左边的轮子
                g.fill3DRect(x,y + 30,60,10,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,40,20,false);//画出坦克盖子
                g.fillOval(x + 20,y + 10,20,20);//画出圆形盖子
                g.drawLine(x + 30,y + 20,x,y + 20);//画出炮筒
                break;
            default:
                System.out.println("暂时没有处理");
        }
    }

    @Override
    public void keyTyped(KeyEvent e) {

    }

    //处理WSAD键按下的情况
    @Override
    public void keyPressed(KeyEvent e) {
        if(e.getKeyCode() == KeyEvent.VK_W){
            //改变坦克方向
            myTank.setDirect(0);
            myTank.moveUp();
        }else if(e.getKeyCode() == KeyEvent.VK_D){
            myTank.setDirect(1);
            myTank.moveRight();
        }else if(e.getKeyCode() == KeyEvent.VK_S){
            myTank.setDirect(2);
            myTank.moveDown();
        }else if(e.getKeyCode() == KeyEvent.VK_A){
            myTank.setDirect(3);
            myTank.moveLeft();
        }
        //让面板重绘
        this.repaint();
    }

    @Override
    public void keyReleased(KeyEvent e) {

    }
}
```

### TankGame02.java

```java
package Chapter16.tankgame2;

import javax.swing.*;
import java.awt.event.KeyListener;

public class TankGame02 extends JFrame {
    //定义一个Panel
    MyPanel mp = null;
    public static void main(String[] args) {
        TankGame02 tankGame02 = new TankGame02();
    }

    public TankGame02(){
        mp = new MyPanel();
        this.add(mp);//把面板（游戏的绘图区域）加进去
        this.setSize(1000,750);
        this.addKeyListener(mp);//让JFrame监听mp的键盘事件
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisible(true);
    }
}
```

## 坦克大战2.0

### 增加功能

1.当玩家按下`J`键，就射出一颗子弹。
2.让敌人的坦克也能够发射子弹(可以有多颗子弹)。
3.当我方坦克击中敌人坦克时，敌人的坦克就消失。
4.让敌人的坦克也可以自由随机的上下左右移动。
5.控制我方的坦克和敌人的坦克在规定的范围移动。
6.我方坦克在发射的子弹消亡后，才能发射新的子弹。=>扩展（发多颗子弹怎么办）
7.让敌人坦克发射的子弹消亡后，可以再发射子弹。
8.当敌人的坦克击中我方坦克时，我方坦克消失。

### 代码实现

#### Enemy.java

```java
package Chapter18;

import java.util.Vector;

public class Enemy extends Tank implements Runnable{
    Vector<Shoot> shoots = new Vector<>();
    boolean isLive = true;
    public Enemy(int x, int y) {
        super(x, y);
    }

    @Override
    public void run() {
        while(true){
            //这里我们判断如果shoots.size() == 0
            if(isLive && shoots.size() < 5){
                Shoot s = null;
                //判断坦克的方向，创建对应的子弹
                switch (getDirect()){
                    case 0:
                        s = new Shoot(getX() + 20,getY(),0);
                        break;
                    case 1:
                        s = new Shoot(getX() + 60,getY() + 20,1);
                        break;
                    case 2:
                        s = new Shoot(getX() + 20,getY() + 60,2);
                        break;
                    case 3:
                        s = new Shoot(getX(),getY() + 20,0);
                        break;
                }
                shoots.add(s);
                //启动
                new Thread(s).start();
            }
            //根据坦克当前的方向来继续移动
            switch (getDirect()){
                case 0:
                    //让坦克保持一个方向走30步
                    for(int i = 0;i < 30;i++) {
                        if(getY() > 0) {
                            moveUp();
                        }
                        //休眠50ms
                        try {
                            Thread.sleep(50);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    break;
                case 1:
                    for(int i = 0;i < 30;i++) {
                        if(getX() + 60 < 1000) {
                            moveRight();
                        }
                        //休眠50ms
                        try {
                            Thread.sleep(50);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    break;
                case 2:
                    for(int i = 0;i < 30;i++) {
                        if(getY() + 60 < 1000) {
                            moveDown();
                        }
                        //休眠50ms
                        try {
                            Thread.sleep(50);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    break;
                case 3:
                    for(int i = 0;i < 30;i++) {
                        if(getX() > 0) {
                            moveLeft();
                        }
                        //休眠50ms
                        try {
                            Thread.sleep(50);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                    break;
            }
            //然后随机改变方向
            setDirect((int)(Math.random() * 4));
            //写并发程序一定要考虑清楚，该线程什么时候退出
            if(!isLive){
                break;
            }
        }
    }
}
```

#### MyPanel.java

```java
package Chapter18;

//坦克大战的绘图区域

import javax.swing.*;
import java.awt.*;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.util.Vector;

//为了监听键盘事件，实现KeyListener
//为了让Panel不停地重绘子弹，需要将MyPanel实现Runnable，当作一个线程使用
public class MyPanel extends JPanel implements KeyListener,Runnable {
    //定义我的坦克
    MyTank myTank = null;
    //定义敌人的坦克，放入Vector
    Vector<Enemy> enemy = new Vector<>();
    int enemyTankSize = 3;

    public MyPanel() {
        myTank = new MyTank(100,100);//初始化自己的坦克
        myTank.setSpeed(2);
        //初始化敌人的坦克
        for(int i = 0;i < enemyTankSize;i++){
            Enemy enemyTank = new Enemy(100 * (i + 1),0);
            //设置方向
            enemyTank.setDirect(2);
            //启动敌人坦克线程
            new Thread(enemyTank).start();
            //给该enemyTank加入一颗子弹
            Shoot shoot = new Shoot(enemyTank.getX() + 20,enemyTank.getY() + 60,enemyTank.getDirect());
            //加入enemyTank的Vector成员
            enemyTank.shoots.add(shoot);
            //启动shoot
            new Thread(shoot).start();
            //加入
            enemy.add(enemyTank);
        }
    }

    @Override
    public void paint(Graphics g) {
        super.paint(g);
        g.fillRect(0,0,100,750);//填充矩形，默认黑色

        if(myTank != null && myTank.isLive) {
            //画出玩家的坦克
            drawTank(myTank.getX(), myTank.getY(), g, myTank.getDirect(), 1);
        }
//        //画出myTank射击的子弹
//        if(myTank.shoot != null && myTank.shoot.isLive != false){
//            g.draw3DRect(myTank.shoot.x,myTank.shoot.y,1,1,false);
//        }
        //将myTank的子弹集合shoots，遍历取出绘制
        for(int i = 0;i < myTank.shoots.size();i++){
            Shoot shoot = myTank.shoots.get(i);
            if(shoot != null && shoot.isLive == true){
                g.draw3DRect(shoot.x,myTank.shoot.y,1,1,false);
            }else{//如果该shoot对象已经无效，就从shoots集合中拿掉
                myTank.shoots.remove(shoot);
            }
        }
        //画出敌人的坦克，遍历Vector
        for(int i = 0;i < enemy.size();i++){
            //取出坦克
            Enemy enemyTank = enemy.get(i);
            //判断当前坦克是否存活
            if(enemyTank.isLive) {//当敌人坦克是存活的，才画出该坦克
                drawTank(enemyTank.getX(), enemyTank.getY(), g, enemyTank.getDirect(), 0);
                //画出enemyTank所有子弹
                for (int j = 0; j < enemyTank.shoots.size(); j++) {
                    //取出子弹准备绘制
                    Shoot shoot = enemyTank.shoots.get(j);
                    //绘制
                    if (shoot.isLive) {//isLive == true
                        g.draw3DRect(shoot.x, shoot.y, 1, 1, false);
                    } else {
                        //从Vector移除
                        enemyTank.shoots.remove(shoot);
                    }
                }
            }else{
                enemy.remove(enemyTank);
            }
        }

    }

    //画出坦克-封装方法
    /**
     * @param x 坦克左上角x坐标
     * @param y 坦克左上角y坐标
     * @param g 画笔
     * @param direct 坦克方向（上下左右） 0表示向上
     * @param type 坦克的类型
     **/
    public void drawTank(int x,int y,Graphics g,int direct,int type){
        switch (type){
            case 0://敌方的坦克
                g.setColor(Color.cyan);
                break;
            case 1://玩家的坦克
                g.setColor(Color.yellow);
                break;
        }

        //根据坦克方向，来绘制坦克
        switch (direct){
            case 0:
                g.fill3DRect(x,y,10,60,false);//画出坦克左边的轮子
                g.fill3DRect(x + 30,y,10,60,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,20,40,false);//画出坦克盖子
                g.fillOval(x + 10,y + 20,20,20);//画出圆形盖子
                g.drawLine(x + 20,y,x +20,y + 30);//画出炮筒
                break;
            case 1:
                g.fill3DRect(x,y,60,10,false);//画出坦克左边的轮子
                g.fill3DRect(x,y + 30,60,10,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,40,20,false);//画出坦克盖子
                g.fillOval(x + 20,y + 10,20,20);//画出圆形盖子
                g.drawLine(x + 30,y + 20,x + 60,y + 20);//画出炮筒
                break;
            case 2:
                g.fill3DRect(x,y,10,60,false);//画出坦克左边的轮子
                g.fill3DRect(x + 30,y,10,60,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,20,40,false);//画出坦克盖子
                g.fillOval(x + 10,y + 20,20,20);//画出圆形盖子
                g.drawLine(x + 20,y + 60,x + 20,y + 30);//画出炮筒
                break;
            case 3:
                g.fill3DRect(x,y,60,10,false);//画出坦克左边的轮子
                g.fill3DRect(x,y + 30,60,10,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,40,20,false);//画出坦克盖子
                g.fillOval(x + 20,y + 10,20,20);//画出圆形盖子
                g.drawLine(x + 30,y + 20,x,y + 20);//画出炮筒
                break;
            default:
                System.out.println("暂时没有处理");
        }
    }

    //如果我们的坦克可以发射多颗子弹。在判断我方子弹是否击中敌人的坦克时，就需要把我们的子弹集合中
    //所有的子弹，都取出和敌人的所有坦克进行判断
    public void hitEnemyTank(){
        //遍历我们的子弹
        //判断是否击中了敌人坦克
        for(int j = 0;j < myTank.shoots.size();j++) {
            Shoot shoot = myTank.shoots.get(j);
            if (shoot != null && myTank.shoot.isLive) {//当我的子弹还存活
                //遍历敌人所有的坦克
                for (int i = 0; i < enemy.size(); i++) {
                    Enemy enemyTank = enemy.get(i);
                    hitTank(myTank.shoot, enemyTank);
                }
            }
        }
    }
    //编写方法，判断敌人坦克是否击中我的坦克
    public void hitMyTank(){
        //遍历所有的敌人坦克
        for(int i = 0;i < enemy.size();i++){
            //取出敌人坦克
            Enemy enemyTank = enemy.get(i);
            //遍历enemyTank对象的多有子弹
            for(int j = 0;j < enemyTank.shoots.size();j++){
                //取出子弹
                Shoot shoot = enemyTank.shoots.get(j);
                //判断shoot是否击中我的坦克
                if(myTank.isLive && shoot.isLive){
                    hitTank(shoot,myTank);
                }
            }
        }
    }
    //编写方法，判断我方的子弹是否击中敌人坦克
    public static void hitTank(Shoot s,Tank enemy){
        //判断s击中坦克
        switch (enemy.getDirect()){
            case 0://坦克向上
            case 2://坦克向下
                if(s.x > enemy.getX() && s.x < enemy.getX() + 40
                && s.y > enemy.getY() && s.y < enemy.getY() + 60){
                    s.isLive = false;
                    enemy.isLive = false;
                }
            case 1://坦克向右
            case 3://坦克向左
                if(s.x > enemy.getX() && s.x < enemy.getX() + 60
                        && s.y > enemy.getY() && s.y < enemy.getY() + 40){
                    s.isLive = false;
                    enemy.isLive = false;
                }
        }
    }

    @Override
    public void keyTyped(KeyEvent e) {

    }

    //处理WSAD键按下的情况
    @Override
    public void keyPressed(KeyEvent e) {
        if(e.getKeyCode() == KeyEvent.VK_W){
            //改变坦克方向
            myTank.setDirect(0);
            if(myTank.getY() > 0) {
                myTank.moveUp();
            }
        }else if(e.getKeyCode() == KeyEvent.VK_D){
            myTank.setDirect(1);
            if(myTank.getX() + 60 < 1000) {
                myTank.moveRight();
            }
        }else if(e.getKeyCode() == KeyEvent.VK_S){
            myTank.setDirect(2);
            if(myTank.getY() + 60 < 750) {
                myTank.moveDown();
            }
        }else if(e.getKeyCode() == KeyEvent.VK_A){
            myTank.setDirect(3);
            if(myTank.getX() > 0) {
                myTank.moveLeft();
            }
        }
        //如果用户按下J，就发射
        if(e.getKeyCode() == KeyEvent.VK_J){
            //判断myTank的子弹是否销毁，发射一颗子弹的情况
//            if(myTank.shoot == null || !myTank.shoot.isLive) {
//                myTank.shootEnemyTank();
//            }
            //发射多颗子弹
            myTank.shootEnemyTank();
        }
        //让面板重绘
        this.repaint();
    }

    @Override
    public void keyReleased(KeyEvent e) {

    }

    @Override
    public void run() {//每隔100ms，重绘区域，刷新绘图区域，子弹就会移动
        while(true) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            hitEnemyTank();
            //判断敌人坦克是否击中我们
            hitMyTank();
            this.repaint();
        }
    }
}
```

#### MyTank.java

```java
package Chapter18;

//坦克大战的绘图区域

import javax.swing.*;
import java.awt.*;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.util.Vector;

//为了监听键盘事件，实现KeyListener
//为了让Panel不停地重绘子弹，需要将MyPanel实现Runnable，当作一个线程使用
public class MyPanel extends JPanel implements KeyListener,Runnable {
    //定义我的坦克
    MyTank myTank = null;
    //定义敌人的坦克，放入Vector
    Vector<Enemy> enemy = new Vector<>();
    int enemyTankSize = 3;

    public MyPanel() {
        myTank = new MyTank(100,100);//初始化自己的坦克
        myTank.setSpeed(2);
        //初始化敌人的坦克
        for(int i = 0;i < enemyTankSize;i++){
            Enemy enemyTank = new Enemy(100 * (i + 1),0);
            //设置方向
            enemyTank.setDirect(2);
            //启动敌人坦克线程
            new Thread(enemyTank).start();
            //给该enemyTank加入一颗子弹
            Shoot shoot = new Shoot(enemyTank.getX() + 20,enemyTank.getY() + 60,enemyTank.getDirect());
            //加入enemyTank的Vector成员
            enemyTank.shoots.add(shoot);
            //启动shoot
            new Thread(shoot).start();
            //加入
            enemy.add(enemyTank);
        }
    }

    @Override
    public void paint(Graphics g) {
        super.paint(g);
        g.fillRect(0,0,100,750);//填充矩形，默认黑色

        if(myTank != null && myTank.isLive) {
            //画出玩家的坦克
            drawTank(myTank.getX(), myTank.getY(), g, myTank.getDirect(), 1);
        }
//        //画出myTank射击的子弹
//        if(myTank.shoot != null && myTank.shoot.isLive != false){
//            g.draw3DRect(myTank.shoot.x,myTank.shoot.y,1,1,false);
//        }
        //将myTank的子弹集合shoots，遍历取出绘制
        for(int i = 0;i < myTank.shoots.size();i++){
            Shoot shoot = myTank.shoots.get(i);
            if(shoot != null && shoot.isLive == true){
                g.draw3DRect(shoot.x,myTank.shoot.y,1,1,false);
            }else{//如果该shoot对象已经无效，就从shoots集合中拿掉
                myTank.shoots.remove(shoot);
            }
        }
        //画出敌人的坦克，遍历Vector
        for(int i = 0;i < enemy.size();i++){
            //取出坦克
            Enemy enemyTank = enemy.get(i);
            //判断当前坦克是否存活
            if(enemyTank.isLive) {//当敌人坦克是存活的，才画出该坦克
                drawTank(enemyTank.getX(), enemyTank.getY(), g, enemyTank.getDirect(), 0);
                //画出enemyTank所有子弹
                for (int j = 0; j < enemyTank.shoots.size(); j++) {
                    //取出子弹准备绘制
                    Shoot shoot = enemyTank.shoots.get(j);
                    //绘制
                    if (shoot.isLive) {//isLive == true
                        g.draw3DRect(shoot.x, shoot.y, 1, 1, false);
                    } else {
                        //从Vector移除
                        enemyTank.shoots.remove(shoot);
                    }
                }
            }else{
                enemy.remove(enemyTank);
            }
        }

    }

    //画出坦克-封装方法
    /**
     * @param x 坦克左上角x坐标
     * @param y 坦克左上角y坐标
     * @param g 画笔
     * @param direct 坦克方向（上下左右） 0表示向上
     * @param type 坦克的类型
     **/
    public void drawTank(int x,int y,Graphics g,int direct,int type){
        switch (type){
            case 0://敌方的坦克
                g.setColor(Color.cyan);
                break;
            case 1://玩家的坦克
                g.setColor(Color.yellow);
                break;
        }

        //根据坦克方向，来绘制坦克
        switch (direct){
            case 0:
                g.fill3DRect(x,y,10,60,false);//画出坦克左边的轮子
                g.fill3DRect(x + 30,y,10,60,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,20,40,false);//画出坦克盖子
                g.fillOval(x + 10,y + 20,20,20);//画出圆形盖子
                g.drawLine(x + 20,y,x +20,y + 30);//画出炮筒
                break;
            case 1:
                g.fill3DRect(x,y,60,10,false);//画出坦克左边的轮子
                g.fill3DRect(x,y + 30,60,10,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,40,20,false);//画出坦克盖子
                g.fillOval(x + 20,y + 10,20,20);//画出圆形盖子
                g.drawLine(x + 30,y + 20,x + 60,y + 20);//画出炮筒
                break;
            case 2:
                g.fill3DRect(x,y,10,60,false);//画出坦克左边的轮子
                g.fill3DRect(x + 30,y,10,60,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,20,40,false);//画出坦克盖子
                g.fillOval(x + 10,y + 20,20,20);//画出圆形盖子
                g.drawLine(x + 20,y + 60,x + 20,y + 30);//画出炮筒
                break;
            case 3:
                g.fill3DRect(x,y,60,10,false);//画出坦克左边的轮子
                g.fill3DRect(x,y + 30,60,10,false);//画出坦克右边的轮子
                g.fill3DRect(x + 10,y + 10,40,20,false);//画出坦克盖子
                g.fillOval(x + 20,y + 10,20,20);//画出圆形盖子
                g.drawLine(x + 30,y + 20,x,y + 20);//画出炮筒
                break;
            default:
                System.out.println("暂时没有处理");
        }
    }

    //如果我们的坦克可以发射多颗子弹。在判断我方子弹是否击中敌人的坦克时，就需要把我们的子弹集合中
    //所有的子弹，都取出和敌人的所有坦克进行判断
    public void hitEnemyTank(){
        //遍历我们的子弹
        //判断是否击中了敌人坦克
        for(int j = 0;j < myTank.shoots.size();j++) {
            Shoot shoot = myTank.shoots.get(j);
            if (shoot != null && myTank.shoot.isLive) {//当我的子弹还存活
                //遍历敌人所有的坦克
                for (int i = 0; i < enemy.size(); i++) {
                    Enemy enemyTank = enemy.get(i);
                    hitTank(myTank.shoot, enemyTank);
                }
            }
        }
    }
    //编写方法，判断敌人坦克是否击中我的坦克
    public void hitMyTank(){
        //遍历所有的敌人坦克
        for(int i = 0;i < enemy.size();i++){
            //取出敌人坦克
            Enemy enemyTank = enemy.get(i);
            //遍历enemyTank对象的多有子弹
            for(int j = 0;j < enemyTank.shoots.size();j++){
                //取出子弹
                Shoot shoot = enemyTank.shoots.get(j);
                //判断shoot是否击中我的坦克
                if(myTank.isLive && shoot.isLive){
                    hitTank(shoot,myTank);
                }
            }
        }
    }
    //编写方法，判断我方的子弹是否击中敌人坦克
    public static void hitTank(Shoot s,Tank enemy){
        //判断s击中坦克
        switch (enemy.getDirect()){
            case 0://坦克向上
            case 2://坦克向下
                if(s.x > enemy.getX() && s.x < enemy.getX() + 40
                && s.y > enemy.getY() && s.y < enemy.getY() + 60){
                    s.isLive = false;
                    enemy.isLive = false;
                }
            case 1://坦克向右
            case 3://坦克向左
                if(s.x > enemy.getX() && s.x < enemy.getX() + 60
                        && s.y > enemy.getY() && s.y < enemy.getY() + 40){
                    s.isLive = false;
                    enemy.isLive = false;
                }
        }
    }

    @Override
    public void keyTyped(KeyEvent e) {

    }

    //处理WSAD键按下的情况
    @Override
    public void keyPressed(KeyEvent e) {
        if(e.getKeyCode() == KeyEvent.VK_W){
            //改变坦克方向
            myTank.setDirect(0);
            if(myTank.getY() > 0) {
                myTank.moveUp();
            }
        }else if(e.getKeyCode() == KeyEvent.VK_D){
            myTank.setDirect(1);
            if(myTank.getX() + 60 < 1000) {
                myTank.moveRight();
            }
        }else if(e.getKeyCode() == KeyEvent.VK_S){
            myTank.setDirect(2);
            if(myTank.getY() + 60 < 750) {
                myTank.moveDown();
            }
        }else if(e.getKeyCode() == KeyEvent.VK_A){
            myTank.setDirect(3);
            if(myTank.getX() > 0) {
                myTank.moveLeft();
            }
        }
        //如果用户按下J，就发射
        if(e.getKeyCode() == KeyEvent.VK_J){
            //判断myTank的子弹是否销毁，发射一颗子弹的情况
//            if(myTank.shoot == null || !myTank.shoot.isLive) {
//                myTank.shootEnemyTank();
//            }
            //发射多颗子弹
            myTank.shootEnemyTank();
        }
        //让面板重绘
        this.repaint();
    }

    @Override
    public void keyReleased(KeyEvent e) {

    }

    @Override
    public void run() {//每隔100ms，重绘区域，刷新绘图区域，子弹就会移动
        while(true) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            hitEnemyTank();
            //判断敌人坦克是否击中我们
            hitMyTank();
            this.repaint();
        }
    }
}
```

#### Shoot.java

```java
package Chapter18;

public class Shoot implements Runnable{
    int x;//子弹的x坐标
    int y;//子弹的y坐标
    int direct = 0;//子弹方向
    int speed = 2;//子弹的速度
    boolean isLive = true;//子弹是否存活

    public Shoot(int x, int y, int direct) {
        this.x = x;
        this.y = y;
        this.direct = direct;
    }

    @Override
    public void run() {//射击
        while(true){
            //休眠50ms
            try {
                Thread.sleep(50);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            //根据方向来改变x,y坐标
            switch(direct){
                case 0://上
                    y -= speed;
                    break;
                case 1://右
                    x += speed;
                    break;
                case 2://下
                    y += speed;
                    break;
                case 3://左
                    x -= speed;
                    break;
            }
            //当子弹移动到面板的边界时，就应该销毁（把启动的子弹线程销毁）
            //当子弹碰到敌人坦克时，也应该结束线程
            if(!(x >= 0 && x <= 1000 && y >= 0 && y <= 750 && isLive)){
                isLive = false;
                break;
            }
        }
    }
}
```

#### Tank.java

```java
package Chapter18;

public class Tank {
    private int x;//坦克的横坐标
    private int y;//坦克的纵坐标
    private int direct;//坦克方向
    private int speed = 1;
    boolean isLive = true;

    public Tank(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    public int getY() {
        return y;
    }

    public void setY(int y) {
        this.y = y;
    }

    public int getDirect() {
        return direct;
    }

    public void setDirect(int direct) {
        this.direct = direct;
    }

    public int getSpeed() {
        return speed;
    }

    public void setSpeed(int speed) {
        this.speed = speed;
    }

    //上下左右移动方法
    public void moveUp(){
        y -= speed;
    }
    public void moveRight(){
        x += speed;
    }
    public void moveDown(){
        y += speed;
    }
    public void moveLeft(){
        x -= speed;
    }
}
```

#### TankGame03

```java
package Chapter18;

import javax.swing.*;

public class TankGame03 extends JFrame {
    //定义一个Panel
    MyPanel mp = null;
    public static void main(String[] args) {
        TankGame03 tankGame03 = new TankGame03();
    }

    public TankGame03(){
        mp = new MyPanel();
        //将mp放入Thread，并启动
        Thread thread = new Thread(mp);
        thread.start();
        this.add(mp);//把面板（游戏的绘图区域）加进去
        this.setSize(1000,750);
        this.addKeyListener(mp);//让JFrame监听mp的键盘事件
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setVisible(true);
    }
}
```