---
title: "Reversing and Patching Minesweeper Part 1 -- Installation and Basic Modification"
date: 2022-08-04T00:00:00-00:00
---

Hello readers, and welcome to the **second** article in this series. In this article, we will be installing **_Ghidra_** and setting up our project, then we will make some **basic modifications** to our target program. Grab a cup of coffee and be prepared to down it, because this is a long one.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/minesweep1title.png)
###### Time for our **first modification**!

### Ghidra Installation

Once you have navigated to the link from the **first article**(you can also find it [**here**](https://github.com/NationalSecurityAgency/ghidra/releases)), you will see the following page:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/latestghidrarelease.png)
###### The latest release of **Ghidra**.

After finding the latest release on the **GitHub** page, click on the **first** zip under **Assets**. Once downloaded, extract it using your preferred program. Keep in mind, you will need at least 1gb of space for extraction, and most likely more for other parts(projects, programs).

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidralocation.png)
###### It is recommended to extract **Ghidra** to a folder similar to this.

Now that **_Ghidra_** has been installed, you can run it using **_ghidraRun.bat_**, or similar on different operating systems.

**Note:** if any problems arise when installing, refer to the **Ghidra’s Troubleshooting Section**, found [**here**](https://ghidra-sre.org/InstallationGuide.html#Troubleshooting).

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidrashortcut.png)
###### I made a shortcut on my **Desktop** for easy access.

### Setting up our first project

We’re almost ready to start **Reverse Engineering** Minesweeper! Once **_Ghidra_** is open, it should look like this:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidraprojectwindow.png)
###### Now we need to create a project.

Conveniently, **_Ghidra_**  uses a **project-based system**, rather than individual files, as **many** times you may be working with multiple files for your needs.

To create a project, click on **_File -> New Project…_**, or press **_CTRL+N_**(or any similar shortcuts depending on the operating system). First you will be asked if you want to create a **Non-Shared Project** or a **Shared Project.** For this series, as it is only us, we will be using a **Non-Shared Project**. However keep in mind, that **Shared Projects** can be handy for certain situations. If we click through, we will be asked where we want to put our project. For this series, I will use the folder I created for projects as seem in a image earlier, **C:\\Ghidra\\Projects**. Give it a name, and hit **Finish**!

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidraprojectlocation.png)
###### The final step in our project creation process.

Now that we have created the project, we need to **add our files**. For now, we will **only** be using one file, **Winmine\_\_XP.exe** downloaded in the **first** article. We can import that through clicking **_File -> Import File…_** or pressing **_I_**_._ From there, navigate to where you downloaded Minesweeper, then click **Select File To Import**. You will get the following popup, and ensure everything looks correct, as seen below.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidraimport.png)
###### The import dialogue.

Once loaded in, you will see a **file summary** of the file you just imported. You can **ignore** this for now, as it can be opened up **later** again if needed.

Time to open up the file! We can open it simply by double clicking the file under our current project. Once opened, the window will look something like this:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/analyzeimage.png)
###### Low image quality, it’s asking if you want to analyze the file.

You can select **Yes**, then click **Analyze**. We will be using **default** settings for now. You can see the progress in the **bottom right**, but it should be relatively **fast** because of how small the file is. You’ve now successfully setup your project! Next, we will be making a **basic modification**, through **changing some of the sounds** in the program, **but first** we need to find **where the sounds are**, and try and **understand a bit more** about the program.

### Learning more about our target program

First, we need to think about what we are looking for. As we are **changing the sounds**, we need to **know what sounds play**, and understand **how they happened**.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/target.png)
###### Now we get to play Minesweeper!

After turning on sound through the **Game** tab, we can hear the following sounds when playing the game:

*   A **timer tick** sound when the timer increases, **which only happens once the user has clicked their first tile**.
*   A **bomb explosion** sound when… you lose through **clicking on a mine**.
*   A funky **win** sound when you clear the board **without touching any mines**.

**Now that we have established what we are looking for and how we get it**, let’s do some brainstorming and then jump into the code.

If you **think** about the game, you will notice that for it **not to be repetitive**, it needs some sort of **randomness** inside the **mine placement**, so you can’t memorize where they are. They most likely didn’t make their own random number engine, as using a widespread library is much easier.

We don’t know it yet, but once you go into the code you will see that this program is written in C++. Upon searching up “**_c++ random_**” in a search engine, we can see that the first result is a page talking about the **rand** library, found [**here**](https://cplusplus.com/reference/cstdlib/rand/). It looks a bit old, but we can see that the library named **rand** can be used for random numbers, and upon **looking into other code examples**, it seems to be pretty **widespread**.

Since it looks like mines are **generated randomly when a new game is made or when the first tile is clicked**, and the **timer starts ticking** when the game starts, **searching for this library** in **_Ghidra_** could help us find a **good area** where we might find some sound **related** code.

Upon opening up **_Ghidra_** again, we can see many things, but the place we are interested in mainly right now, is **Symbol Tree**, as this can show us **functions**, imported and exported **DLLs**, **Labels**, and **more**. Thankfully, instead of searching through each, we can use the **Filter** area below. Upon searching up **rand**, we can see two things under the **MSVCRT** DLL, as seen in the below image.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidrasymboltree.png)
###### Yes! They are using the library we searched up.

If we right click on **rand** we can see the following option **_Show references to_**. If we click on it, we will see the following window:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidrarandreferences.png)
###### You can also press **CTRL+SHIFT+F**.

If we select the first reference, we can see the following code in the **Decompiler** window:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler.png)
###### Hmm, what could it all mean?

If we go back to the earlier **rand** reference website, we can see the following example code:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/randexample.png)
###### Looks similar.

As we can see, a random number is generated through using “**rand() % num**”. As we can see in the decompiled code, **iVar1** is set to **rand()** and then it is divided by the provided parameter. As we can see, this function creates a random number using a provided one. We can look for times this function was referenced to by **right clicking** the function name, then clicking **References**, then clicking **References to \[function name\]**, or simply clicking on the function and then pressing **_CRTL+SHIFT+F_**.

When looking under the **references**, there is **only two**, and they both seem to be coming from relatively the **same area**. Visiting said area will bring us to some weird code, that is hard to figure out exactly what it does.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler1.png)
###### The decompiled **C++** code.

But not all hope is lost yet. We can whip out another tool downloaded earlier, **Cheat Engine.** Whilst it’s mainly used for game hacking, we can repurpose it for our needs, which is settings **breakpoints** in code. First, launch your **Minesweeper executable**, then open **Cheat Engine**. You can attach it to your Minesweeper **process** through clicking on the **search icon** in the **top left**, then searching through the **process lis**t to find one named **similarly** to our executable.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/minesweepprocess.png)
###### Our **Minesweeper** process

Once you click **Open**, you are attached to the process. To add our address that we want to add a breakpoint to in the code, we need to add it under **Add Address Manually**. Find the address of the code we want to investigate in **_Ghidra_**, then click **OK**. If we right click on the address in the **Address List**, we can see an option to **disassemble** the **memory region** near this address.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/functionaddress.png)
###### Our **address** that we want to look into.

By doing this, we can see the Assembly instructions around our address, which match up with the ones from **_Ghidra_**. Once we get to the correct code, we can set a breakpoint through right clicking the line, then clicking **Set Breakpoint**. If a popup happens asking to attach debugger, click **Yes**. Now, if we play Minesweeper and start a new game, we will see it freeze, indicating a breakpoint was hit. If we navigate back to **Cheat Engine**, we can see options at the top of where we set the breakpoint. If we click run, we can see the breakpoint was hit again. If we click **Run** multiple times, it will eventually stop, and we can play our game. Through this, we can conclude that on game startup, it **generates the mines** then **rather then on first click**. No luck here.

But **don’t** be discouraged! There is **many other ways** we can sift through the code. Anyways, this is still very **helpful**, as we now know **where** we have to modify if we want to **change** the mine count, the random placement, etc. Thinking **back** to the sound, the timer tick sound **only** starts when you **first** click on a tile. If we find when the timer tick sound **starts**, we might be able to change it **then**, or we could also try and find when the timer **increases**, as a sound is played then too. **Cheat Engine** can track variables, and even change them. If we go back to **Cheat Engine**, we can start our first search for the value of **0**, which is the timer value when we start a **new** game.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ce1.png)
###### That’s a lot of results.

Since it would take **forever** to sort through all of the results, we can click on our first mine, increasing the timer. Since it would be **too difficult** to correctly time our search for when the timer is a specific value, we can change the **scan type** to **Increased Value**. **Every time** the timer increases, we can do **another** scan through clicking **Next Scan**. Soon the results will get low enough to just scroll through the addresses, and find the one that increases correctly with the timer.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ce2.png)
###### We found it!

If we now add this to our **address list** through double clicking on the address, in the **address list** we can change it through right clicking the address, **_Change Record -> Value_**. If we change it back to something like **10**, we can see the timer starts counting up from **10**. Now that we have correctly identified our timer variable, let’s search for it in our code.

If we press **G** in our project or go to **_Navigation -> Go To…_** we can input the address of our timer variable in it, and click **OK**.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidravariable.png)
###### The variable inside the code.

As usual, we can search for **references** to it, through using the **right click menu** or shortcuts.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradatreferences.png)
###### Where to go first?

Since to modify a variable you have to **write** to it, we can select the different locations that **write** to it.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler2.png)
###### The first reference.

When looking through it seems that the first **write** checks if a variable isn’t 0(which is most likely a variable checking if we started), then it checks if the current time is below **999**, because as for Minesweeper’s **time display**, it stops ticking at **999**, as that is the max amount of time it can display. We see that it increases our variable by **1**, then calls two functions. **Double clicking** on the first one returns a function with a bunch of hard to read code, but this is most likely **not** where it plays sound. If the developers were **efficient**, they would have **one function**, where **multiple** sounds could be played through an **argument**. The second function has a argument when it get’s called, so lets investigate it.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidradecompiler3.png)
###### Yes! PlaySound!

We can see that it first checks if a variable is equal to **3**, which we don’t know. But most likely, this is just a variable checking if the game started or not. If we look further, we can see that it plays sound! If the inputted argument is **1,** it plays a specific sound, same for **2** and **3**! Now, all we have to do is to set a breakpoint in the earlier code to test the timer counting function. If we press **_ALT+LEFT_**, or press the **back arrow** in the top menu of buttons, we can go back to that spot, and mark down the address of where the function is **called**.

We found the function, but we can’t see an address of the line of code in the **Decompiler,** so how do we find it?

If we look through the function code in the main **Listing** window that shows **Assembly Code**, we can see the following code:

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidraassembly.png)
###### The function call.

In **Assembly**, the instruction **CALL** basically just “calls” a function. If we want to supply an argument with the call, we will **PUSH** it first, as you can see above the **CALL**. We can see the address on the side, which we can then use in **Cheat Engine** to set a breakpoint, as explained earlier.

Once we set the breakpoint at the address, we can see that whenever the timer increases, the breakpoint is hit, then once the breakpoint is stepped through, the sound is played. So now, we have figured out that when was pass **1** into the sound function, it is the timer tick. As we could see when looking at the sound function, **2** and **3** also played different sounds. To keep it **simple**, for this first modification, we will **change** the timer tick sound to a **different** one. As we can see, all we need to do is change **one** line of code, and we have an **entirely different** sound!

Thankfully, as it pushes **1** instead of a variable, we have some **available** space to change this to whatever number we desire. However, if you **don’t have enough space** to add new code or change old pieces, you will have to make more space somewhere, jump to it, and jump back. You **don’t** have to **worry** about that now however, as we will **learn** that **later**, and it is **not** needed for our purposes.

To patch a instruction, we can simply **right click** on the line of code, then press **Patch Instruction**, or hit **_CTRL+SHIFT+G_**. From there, we can change the hex value of **1**(0x1), to the hex value of **2**(0x2).

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidrapatching.png)
###### Patching the instruction.

From there, you can press **enter** to apply the changes. Now that we have changed the instruction, we need to create a new executable. As it won’t change automatically, we have to make it ourselves in **_Ghidra_** through clicking **_File -> Export Program_**, or pressing **O**. Make sure the format is **PE** so we can easily run it and that the output file is in the right location, then press **OK**.

![](https://github.com/merrittlj/merrittljblog/raw/main/assets/ghidraexport.png)
###### Finally, we finished!

Now if we run our patched executable, we can hear the **win** sound as the timer tick! It might get annoying fast, but we successfully patched our first program!

### Summary

Even though the thought process might seem **difficult at first** for patching a binary, with **experience** you can quickly recognize what functions do, and what you need to change.

In the **next article**, we will be using our knowledge of the program and **Reverse Engineering** to rig the mine count to make **every** tile a mine.

Thanks for reading, [**_merrittlj_**](https://github.com/merrittlj)
