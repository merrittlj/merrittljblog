---
title: "Reversing and Patching Minesweeper Part 0 -- Introduction"
date: 2022-08-02T00:00:00-00:00
---

Hello readers, and welcome to my first article I’ve wrote and the first in this series. In this series we will be **Reverse Engineering** a basic, old **Windows Minesweeper binary** and **creating custom patches** for our desired functionality.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/minesweep0title.png)
###### _This is going to be a fun series._

### **My goal for this series**

There are **many** different reasons why I’m starting this series. However, in general, besides **having fun**, this is meant to be a **well documented, easy** introduction to **Reverse Engineering binaries** as a whole; and **some** of this knowledge can even be **used outside** of this topic.

### **Needed Resources**

As **Reverse Engineering** is pretty complex in general, we will need a couple things to get started, such as:

### **Your target program**

For this series, we will use a simple **Windows Minesweeper** binary(found [**here**](http://www.minesweeper.info/downloads/WinmineXP.html)). We will be **looking through** and **investigating** the multiple functionalities of this program, and then we will **apply patches** to make it do what we desire.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/target.png)
###### _Our target program_

### **Your favorite Reverse Engineering Tool**

There is **many** awesome and incredibly useful programs out there for **Reverse Engineering**, but in this tutorial we will be using **_Ghidra_**, and **very useful** and popular tool developed by the **_NSA_**. It is **free**, and is seen to many as a **strong competitor to _IDA Pro_, another(paid) extensive tool**. You can download it [**here**](https://github.com/NationalSecurityAgency/ghidra/releases).

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidra.png)
###### _Made by the **NSA**! That’s how you know it will be good. Credit: [**ghidra-sre.org**](https://ghidra-sre.org/images/GHIDRA_1.png)_

### **Cheat Engine**

**Cheat Engine** is useful for many things, mainly some of the features that **_Ghidra_ doesn’t have**. With Cheat Engine, if you think some code is doing something, you can easily **test it using breakpoints** and see what certain **variables are** as well, or you could track certain **addresses** and their **value** throughout execution. You can find it [**here**](https://www.cheatengine.org/).

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ce.png)
###### _**Cheat Engine** with Windows dark mode._

### **A good understanding of how computers work at a low level(and how to operate one in general)**

This can apply to **many** things. Having a **solid understanding on how things are functioning** greatly **helps readability and flow** when working on a program.

For starters, you should have a **basic understanding of Assembly**, as well as **coding** in general. Keep note, that whilst the **more you have experience** with coding and Assembly **the better off you will be**, but **to get started all you should need is a basic understanding** of syntax and different operations(Such as **mov** or **jmp** in **Assembly**, or **C++ project structure**).

There are **many great places** to learn either coding or Assembly on the World Wide Web, but some good places to look further into are:

*   Learning Assembly: [**The Art of Assembly Language(2nd Edition)**](http://www.staroceans.org/kernel-and-driver/The.Art.of.Assembly.Language.2nd.Edition.pdf)
*   Learning C++: [**C++ Basics/Reference Guide By W3 Schools**](https://www.w3schools.com/cpp/default.asp)
*   Combining your knowledge with both: [**Godbolt Compiler Explorer**](https://godbolt.org/)
*   Going further with your knowledge: [**Crackme Practice**](https://crackmes.one/)

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/peek.png)
###### _A peek into some interesting code in **Ghidra**._

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/godbolt.png)
###### _The default [**godbolt.org**](https://godbolt.org/) webpage._

### **Summary**

**Reverse Engineering is a exciting and challenging topic**, with **many possibilities**. In this series, we are just **touching the surface** and patching a simple binary, but there is **much more for you to learn and do**.

**In the next article**, we will be installing **_Ghidra_**, and getting it up and running to start our project.

Thanks for reading, [**_merrittlj_**](https://github.com/merrittlj)