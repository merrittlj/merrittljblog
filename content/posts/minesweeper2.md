---
title: "Reversing and Patching Minesweeper Part 2 -- Advanced Modifications"
date: 2022-08-14T00:00:00-00:00
---

Hello readers, and welcome to the **third** article in this series. In this article, we will be using our **knowledge** of Minesweeper’s code to make more **advanced** modifications, such as making **all** of the tiles mines.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/minesweep2title.png)
###### _We do a little trolling._

### **Going back to the Part 1**

As a little **refresher**, in the **second** article we searched through Minesweeper’s **code**, and found where mines are **randomly** created, and where the timer started **ticking**. Unfortunately, the spot where mines are created **wasn’t** very useful at the time, but now this helps us greatly as we **don’t** have to find where we need to patch, we only need to find **what** to patch.

### **Looking further into the code**

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler4.png)
###### _The **decompiled** code where mines are randomly created._

### **Using what we know**

As we can see, when we were looking for when the **sound** was played, we found where mines are **created**. The function that the **selected** variable is **initialized from** returns a **random** variable, so we can assume **iVar1** and **iVar2** are **both** random numbers. In the **next condition**, we can see that both are used, most likely to randomly choose if a square should be a mine or not. This is further **confirmed** through the code **below**, as it **subtracts** a number(most likely how many more mines need to be randomly placed).

### **Organization and readability**

To make sure we can keep **track** of things throughout the complex **process**, we can set a **pre-comment**(one that shows **before** the code) to quickly write things down. These **save** to the project, so you can have them even the next time you start the project up. We can either **right click** on a line in the Assembly code, or do the **same** in the decompiled code. From there we go to **_Comments -> Set Pre Comment…_** and then type in what we want to write down. For the code we found, we can simply write **“Decreases mine count.”**.

To further **improve** readability, we can(**cosmetically**) change the variable name through **right clicking** the variable in either code, then pressing **_Rename Global_**, or by **highlighting** the variable then pressing **L**. Our final line of code that decreases the variable will look similar to below.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler5.png)
###### _That looks much better!_

### **Testing the variables**

Setting a **breakpoint** under the code that **decreases** the mine count left, we can see that once we start a **beginner** game and **count** every time it hits the breakpoint, it amounts to **10**. If we **track** the address that is being decreased whilst clicking on the breakpoints, we can see it going down from 10–9, 9–8, etc. If we **disable** the breakpoint and still track the address, we can see it at the **current** mine count for the board, so we can safely assume that the amount left of mines to be placed gets **reset** once **all** of them have been placed.

### **Our testing results**

Now that we have found the mine count left **variable**, we can find where it gets set, and change that to **enough** mines to **fill** up the entire board. If we find the **write** references to the mine count, we can change it when we find the code that we think most likely sets it.

Ah, if we looked **further** into the function code, we would have realized that **all** of the references are all in it! **First**, there is a line of code that **sets** the mine count to a variable(most likely the starting mine count, that doesn’t change), then there is the line that **decreases**, and then there is the same line of code that sets the mine count yet **again**. Earlier, we saw the mine count decreasing when the breakpoint was hit, yet it **appeared** to stay **consistent** the whole time **without** the breakpoints. We can see that mine count left variable is set to the **non-changing** mine count, it is **iterated** through the ensure that **all** mines are placed, then it is **reset** back to the original mine count. We can do some **formatting** to the chunk of code in the function to make it look much **better**, making the end result look something like below.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler6.png)
###### _Now it’s getting somewhat readable._

### **Testing the starting mine count**

The only downside to improving the appearance is that now instead of easily tracking variables in **Cheat Engine** by adding the address using the name, now we have to **double click** on the variable, and **copy** the address from there. Anyways, we can easily head back to the code to continue through clicking the back arrow at the top, or pressing **_ALT+LEFT_**. We can copy the address of our starting mine count variable, appropriately named **starting\_mine\_count**, then put that address in **Cheat Engine** and re-enable the breakpoints.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ce3.png)
###### _I changed the descriptions for clarity(**CTRL+ENTER).**_

Now, we have the **starting** mine count variable. While this could be changed to set the mine count to what we desire, due to the **uncertainty** of the code we will have to take a **different** approach.

### **Patching the mine count**

Onto the patching! Even though patching the starting mine count **might** be useful, it would be easier to just **directly** change the mine count when it is set, as we don’t know how the starting mine count might be changed or taken into account **after** we change it.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidraassembly1.png)
###### _The Assembly setting the mine count._

### **Going through the code**

As you can see above, the **Assembly** sets **EAX** to the **starting** mine count, and then sets the **mine** count to **EAX**. For **simplicity**(and not to waste any **space**) we can change **EAX**, rather then setting the mine count to a **different** value.

### **What do we patch it to?**

Since the board size of a **Beginner** board is a total of **9x9**, for now(not worrying about other sizes) we can simply **patch** the instruction to set the starting mine count to **81**, or **80** if you want to **guarantee** a first click not a mine, but the second is(due to Minesweeper moving mines to make your first click not a mine).

As you can’t patch the decompiled code **directly**, you have to do it through the **Assembly**. Even though the decompiled code looks complex, in Assembly all we have to do is modify **EAX**. In hex, **81** is represented as **51**. To **prove** our theory correct, we don’t have to **calculate** how much mines we need, for now we will **only** worry about the beginner board.

Now armed with the knowledge of what to do, we can simply now replace **EAX** in the instruction with **0x51**(0x indicates that it is in hex). If we visit the **Bytes** window, we can see that as **0x51** is less bytes then the before **EAX**, so we have some free space.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidrahex.png)
###### _Not much, but maybe useful later._

Now that we have patched it, it should be working perfectly fine, but on run, you should see something very different.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/minesweepblank.png)
###### _All of our clicks are blank!_

Doing more **research** into this, and we come up with a very important rule that we were forgetting **earlier**. We can look further into this behavior in [**this**](https://web.archive.org/web/20180618103640/http://www.techuser.net/mineclick.html) article. In the end, when the **first** click is a mine, it **moves** it to somewhere **without** a mine. However, since there is **no** spots **without** mines, we have a weird state where **all** tiles are mines, but **aren’t** at the same time. Hopefully there is code somewhere that we can easily **find**, and then **disable** this functionality to **guarantee** a mine click every time.

### **Looking for the first click mine prevention**

As **Minesweeper** has to **eventually** let you click on a mine, there has to be a variable **somewhere** where it tracks when you can click on a mine, and when you can’t(such as on the first click). As we want this variable to work **more** then **once**, we need to **reset** it when a new game gets **started**. We already have found where a new game gets started, we can start looking there.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler7.png)
###### _Interesting…_

The variable **DAT\_010057a4** looks interesting, so lets see if we can track it in **Cheat Engine**. If we go to the **Beginner** board and test it their, we don’t see it increase. However, as you may realize, this is **misleading**. The variable most likely gets set if you **successfully** click on a square, instead of clicking on a square and then the the mine getting **moved**, to the same square(with no free squares). While it may sound confusing right now, we can confirm this theory through starting a **Custom** board, and then tracking the variable after a successful click. This number jumps up fast, and from this we can infer that the variable is how many squares are cleared.

We can look into the **references** for this, as it it most likely used to **check** for the first click. Since we haven’t cleared any mines **yet**, the value is **0**, being easy to track in the code.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler8.png)
###### _That is exactly what we are looking for!_

To finally confirm this further, we can start a new **Beginner Game**, set the value to **1**(or any other value **above** 0), then click.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/minesweepbeginner.png)
###### _Yes!_

We got it! When we set the variable to a value above **0**, it bypasses the first click check! Are only problem now, is we have to scale the mines according to board size. We don’t want to focus on **Custom** boards for now, as it seems like a bit of a waste of time, and they most likely use different variables anyways.

### **Patching the variable**

Before we should start experimenting with board **size** and **dynamic** mine counts, we should first **set** this variable so we don’t have to use **Cheat Engine** every time to test it. We can simply change the following:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler9.png)
###### _I changed the variable name for appearance._

From **EDI**, to **1**(or anything **higher** then 0). We have some **free** space after this, that we can patch with **NOP**s to prevent any weird behavior. Exporting and running this, we can see that everything has went well, so now we can **start** working on changing the mine count **depending** on the board.

### **Finding height and width variables**

To start off, we need to look for a variable that controls board size. Since it is most likely used in calculations, it would make sense to have two variables to track width and height of the board. We can easily find these using **Cheat Engine**, and scanning for the height and width while changing the board size. We can do the first scan for 9, then change it to a **Intermediate** Board and simply scroll through the list to see that changed.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ce4.png)
###### _Only 4 changed._

We can sort out which is width and height through making **Expert** boards, as they are 16 tall and 30 wide.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ce5.png)
###### _Changed descriptions._

### **Making the mine count dynamic**

Conveniently, if you look into the function code that randomly places mines(the one we **changed**) you can see the following code towards the **top**:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler10.png)
###### _Those addresses look familiar._

Even better, in **Assembly**, the **MUL** instruction multiplies **EAX** and whatever you supply, and stores it in **EAX**; you can find the reference [**here**](https://c9x.me/x86/html/file_module_x86_id_210.html). As we know from earlier, **EAX** is the mine count, which we hard coded to 81 for testing purposes. As we can simply use the instruction **MUL ECX** to set **EAX** to our desired mine count, we can replace all of space used by this instruction by pressing **C**, or right clicking and pressing **Clear Code Bytes**, and then changing all of the empty space to NOPs.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler11.png)
###### _That’s a good chunk of space!_

Now that we have some **space**, we can now replace the first **instruction** with **MUL ECX**, and then we should be done! However, we have one small(and fixable) problem. The code is pretty sloppy, so how the original mine count would be set is through **directly** setting it to something, rather then **referencing** it. It should be fine as **EAX** gets set earlier anyways, but another reason why it was set is because of the function above it.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler12.png)
###### _Why is it doing that?_

While this may look confusing at first, we need to think about **variables** such as **ECX**, **EAX**, etc. They are used in many functions to store data, and it just happens that in the function above the mine count getting set, it **changes** around some of the variables.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler13.png)
###### _There’s our problem._

In addition, it doesn’t reset the variables in the function, as normally after the function **EAX** gets overwritten anyways, but in this case we are **multiplying** it. Thankfully, this doesn’t appear to matter when it is executed, so we can move it behind the mine count getting set. Moving around a bit more to make sure everything **works** and **NOP**ing the **free** space, we end up with something like below:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler14.png)
###### _Note: Make sure when copying the call instruction that the **correct** hex is set._

This should all work, so now if we go ahead and export and run…

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/minesweepmines.png)
###### _Looks like we made the easiest game of Minesweeper, just flag all of the mines._

We got it! Now, **every** click on every board is a **mine**, even **Custom** boards!

### **Summary**

This was a long, and complicated article. We did a lot here, and I’m not sure if there is much more to modify with Minesweeper at least, but I’m planning to make many other articles on Reverse Engineering related things. Feel free to recommend any ideas going forth for the series, or any other thing I should write on.

Thanks for reading, [**_merrittlj_**](https://github.com/merrittlj)
