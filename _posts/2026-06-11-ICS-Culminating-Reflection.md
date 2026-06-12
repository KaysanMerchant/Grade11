---
layout: post
title: "Intro to Computer Science Culminating Review"
author: Kaysan Merchant
date: 2026-06-11
---

# Overview

In this review, I cover topics about a game made in processing code by Rayan Bashir and myself, called Mom's Backyard. The game is an RNG game where you mine underground, getting random ores that you can sell for money to buy upgrades. The ultimate goal of the game is to obtain a Tuntungtungsanite, which with base odds is a 1/17025 chance.

I will be going over variables & data tracking, selection structures, repitition structures, arrays & data structures and use of custom functions & error checking.

<a href="{{ "/files/MomsBasement.zip" | relative_url }}" download style="display:inline-block;padding:0.75rem 1rem;background:#007acc;color:#fff;text-decoration:none;border-radius:4px;">
  Download Mom's Backyard
</a>

# Variables & Data Tracking

To begin with, the variables used vary widely. The main variable types used are int, float and boolean, with some, but few strings. Of course, we include arrays of these types in various ways. Float variables were used in the RNG system(will be explained later) and strings were mainly used in arrays for image file switching and text displays in the inventory and shop. Boolean variables were usually only used for game state changes, like when the inventory is open, the shop is open, or the game starting. They were also used to begin processes like the ore animation.
 
## Integer Variables

Integer variables are used the most. They are used for upgrade levels, player data, ore weights, ore IDs, and couple of more functions. Integers were used for ore IDs because it was easily callable to be used in oreAnimations, invetories and for selling. For player data, it was used to be easily altered and used in actual modifiers for the game(e.g. making mineSpeed make the ore animation go faster as seen below). This way of using integers for tracking player data and ores was the easiest method we could think of, which is why we did not struggle with using it.

```java
if (oreAnim){
      if (pickY <= 240){
        pickY += 3;
        pickX += 1;
        delay(int(5/mineSpeed));
        
      }
      imageMode(CENTER);
      image(glow, width/2, oreY, 75, 75);
      image(oreImages[currentOreID], width/2, oreY, 75, 75);
      imageMode(CORNER);
      image(rock, 250, 200, 300, 200);
      oreY -= mineSpeed*3;
      if (oreY <= maxOreHeight) {
          delay((int)(2000/mineSpeed));
          oreY = height/2;
          pickY = 175;
          pickX = 145;
          oreAnim = false;
      }
}
```

# Selection Structure

Selection structures were used in the code in various ways. One of the many ways we use it is to check game states. Very simply, we use selection structures like if statements to check if the game has started, if an ore animation has started, if the shop is open and if the inventory is open. Below is how our draw function is structured to check for these.

```java
void draw(){
  if (start){ //if game not started(startmenu is true)
    tint(depthUpgradeColors[currentDepth]);
    image(background, 0, 0, 800, 600);
    tint(255);
    startMenu();
  } else{ // if game started
    if (!inventoryOpen && !shopOpen) { // if both shop and inventory is closed
      gameMenu();
    } else if (inventoryOpen) {
      drawInventory();
    } else if (shopOpen) {
      drawShop();
    }
    if (oreAnim){ // if ore animation is started
      if (pickY <= 240){
        pickY += 3;
        pickX += 1;
        delay(int(5/mineSpeed));
        
      }
      imageMode(CENTER);
      image(glow, width/2, oreY, 75, 75);
      image(oreImages[currentOreID], width/2, oreY, 75, 75);
      imageMode(CORNER);
      image(rock, 250, 200, 300, 200);
      oreY -= mineSpeed*3;
      if (oreY <= maxOreHeight) {
          delay((int)(2000/mineSpeed));
          oreY = height/2;
          pickY = 175;
          pickX = 145;
          oreAnim = false;
      }
    }
  }
}
```

Other examples of our selection structure usage if for conditional activations, like in out custom function, checkObjectPressed. In checkObjectPressed, we check if the mouse is within certain boundaries, and if it is, the object gets a white overlay on it. It also activates a process if clicked.

```java
boolean checkObjectPressed(int x, int y, int button_width, int button_height) {
  if (mouseX >= x && mouseX <= x + button_width && mouseY >= y && mouseY <= y + button_height) { //If in range
    fill(255, 126);
    rectMode(CORNER);
    noStroke();
    rect(x, y, button_width, button_height);
    noTint();
    if (mousePressed) { //if clicked
      return true;
    }
  }
  return false;
}
```

# Repitition Structure

Loop are used very often in our code. One of the best implementations of our code using loops is when we loop through all the ores in the inventory. For each ore, we draw a slot for the inventory with its own space. Every 5 ores, it moves on to the next line. The loop here simply goes through each ore in our ore table.

```java
for (int i = 0; i<25; i++) {
    if (i < 5) { // if there ore is part of the first row
      drawInventoryOreSlot(300+(i%5)*(480/5), 20, i);
    } else if (i < 10) { // if ore is part of second row
      drawInventoryOreSlot(300+(i%5)*(480/5), 20+(560/5), i);
    } else if (i < 15) { // and so on
      drawInventoryOreSlot(300+(i%5)*(480/5), 20+(2*560/5), i);
    } else if (i < 20) {
      drawInventoryOreSlot(300+(i%5)*(480/5), 20+(3*560/5), i);
    } else if (i < 25) {
      drawInventoryOreSlot(300+(i%5)*(480/5), 20+(4*560/5), i);
    }
}
```

Another similar usage is the reset button. When resetting, each ore in the player's ore inventory must be wiped, in a loop that goes through every row.

```java
 if(checkObjectPressed(300, 462, 200, 75)){
    for(int i = oreInv.getRowCount()-1; i >= 0; i--){
      oreInv.setInt(i, "quantity", 0);
      oreInv.setInt(i, "isFound", 0);
    }
 }
 ```

 None of the repitition structures posed an issue to us, due to the semester of experience we gained using structures like this.

# Arrays & Data Structure

Now here comes the good part. Arrays and data structures. In our code, arrays are used secondary to **tables**. Tables essentially act as 2d arrays where you can have columns and rows. Rows are given integer values while columns are given string names. The columns, in a pre-made table must be specified as header when loading the table, as seen below.

```java
oreTable = loadTable("oreTable.csv", "header");
oreInv = loadTable("oreInv.csv", "header");
playerData = loadTable("playerData.csv", "header");
```

Our code only uses 3 total tables to store: every ore, the ores the player has, and all player data. To access or change any value in any of the tables, two commands can be used: ```table.getInt(rowNum, column)``` and ```table.setInt(rowNum, column, value)```. Here they are in use:

```java
playerData.setInt(0, "value", cash);
playerData.setInt(1, "value", (int)mineSpeed);
playerData.setInt(2, "value", (int)luckMultiplier);
playerData.setInt(3, "value", currentDepth);
saveTable(oreInv, "data/oreInv.csv");
saveTable(playerData, "data/playerData.csv");
```
```java
for(int i = (currentDepth+1)*5-1; i >= 0; i--){
    weight -= oreTable.getInt(i, "weight");
    if (weight <= 0){
      oreInv.setInt(i, "quantity", oreInv.getInt(i, "quantity")+1);
      if (oreInv.getInt(i, "isFound") == 0){
        oreInv.setInt(i, "isFound", 1);
      }
      return i;
    }
}
```

The first code snippet is from the player save function. It uses ```table.setInt(rowNum, column, value)``` to set the unsaved player data into the table values. It then saves the tables into the game data folder, making it a permanent save.

The second code snippet utilizes the weight of ores in an RNG system to determine what ore you get when you mine. Every time a new ore is cycled through, it pulls its weight from the table using ```table.getInt(rowNum, column)```. It subtracts that from a randomly generated weight and one the randomly generated weight is less than or equal to 0, it takes the ore ID of the ore whose weight was subtracted last and uses both ```table.setInt(rowNum, column, value)``` and ```table.getInt(rowNum, column)``` to increase the quantity of that ore in the player invetory by one. It will also check if the ore has been found, using 1 as true and 0 as false.

There are other ways to use tables, like using a function to get or set a string or a float instead of an integer, but our uses only required integers. Due to this data structure being completely new to us, it required some time to get used to, as we only usually used arrays before this. The way I was able to wrap my head around using tables, was to think of it as a 2D array. You would have arrays for the columns, and arrays for each row, and they would cross over each other like a web. I believe using this made out game 10x easier to code, as without it, our code would abe a big jumbled up mess of arrays and text file decoding.

Arrays are also used in our code, for loading pickaxe images and choosing the pickaxe image needed at any point.

# Use of Custom Function & Error Checking/Restrictions

Moving on to the final section, our use of custom functions & error checking. One of the custom functions I wrote, that the whole game functions off of, is the function mineOre. This function esserntially is what makes our game a game. It randomly chooses an ore for the player to receive every time they mine.

```java
int mineOre(){
  for(int i = 0; i < (currentDepth+1)*5; i++){
    totalWeight += oreTable.getInt(i, "weight");
  }
  float weight = random(0, totalWeight+1)/1.00+(luckMultiplier*0.05);
  totalWeight = 0;
  for(int i = (currentDepth+1)*5-1; i >= 0; i--){
    weight -= oreTable.getInt(i, "weight");
    if (weight <= 0){
      oreInv.setInt(i, "quantity", oreInv.getInt(i, "quantity")+1);
      if (oreInv.getInt(i, "isFound") == 0){
        oreInv.setInt(i, "isFound", 1);
      }
      return i;
    }
  }
  return 0;
}
```

The first thing that happens in the code is that the functions calculates the total weight of the ores available to a person at their depth upgrade. What I mean by weight is that each ore has a number attached to it that describes how often it shows up. For example, our rarest weight has a weight of 1, while dirt, the most common, has a weight of 3000. The greater the number, the more common it is. Let me explain why.

At each depth upgrade, you can unlock 5 ores. For this example, lets say you have all 25 ores unlocked. The first thing the method does is add the weight of all the ores. This gives us a sum of 17025. The next step is generating a random number between 0 and 17025. This, in a way, already gives us the random selection. The next step is where it gets tricky. Starting from the rarest ore, it takes the weight of every ore and subtracts it from the randomly generated number(e.g. 17025-1-2-3-5-10-20 and so on and so forth). When a certain weight causes the the random number to drop to or below 0, it stops looping. The ore ID connected to that weight then gets returned by the function and 1 gets added to the player's inventory of that certain ore ID. If the ore has not been found yet, it sets the isFound variable to 1, or a true in integer form.

The reason we used this method to create the RRNG system is because it was one of 2 methods we found to implement RNG and this seemed much easier than the other method we found. It posed a challenge when trying to implement tables into this system, as changing up the equation to fit our code structure required some thought, but once the basics of tables were learned, it became a piece of cake.

Now the error checking present in this function is at the very bottom, where it says return 0. 0 is dirt in our ore ID. Essentially, in case the RNG system failed(which in testing it did, but was fixed), it would show a piece of dirt but would not update the inventory or the isFound status. It was essentially a placeholder that would only be used if the RNG system failed.

The next custom function I will show is one of our button. Each special button in our code(e.g. the shop button, inventory button, mine button, save button) ran a seperate function, and the button I will show you is the save button.

``` java
void saveButton(int x, int y, int button_width, int button_height) {
  imageMode(CORNER);
  image(saveButton, x, y, button_width, button_height);
  if (checkObjectPressed(x, y, button_width, button_height)) {
    playerSave();
  }
}
```

This button is pretty simple, but the logic it gets triggered by and it triggers is a bit more complex. Initially, it takes 4 inputs. The x position of the button, the y position, the button width and the button height. With this, it loads the specific image file using the inputted coordinates then it runs a selection statement, checking for another function.

```java
boolean checkObjectPressed(int x, int y, int button_width, int button_height) {
  if (mouseX >= x && mouseX <= x + button_width && mouseY >= y && mouseY <= y + button_height) {
    fill(255, 126);
    rectMode(CORNER);
    noStroke();
    rect(x, y, button_width, button_height);
    noTint();
    if (mousePressed) {
      return true;
    }
  }
  return false;
}
```

This one takes the inputs of the previous statement and checks in the mouse is within the boundaries of those inputs. If it is, then a translucent rectangle is placed over it to create a highlight effect. After that, it checks if a mosue button is clicked. It the mouse click happens in the boundaries of the button, the function returns a true.

Moving back to the save button, when a true is returned by the collision detection, it triggers the player save function.

```java
void playerSave(){
  playerData.setInt(0, "value", cash);
  playerData.setInt(1, "value", (int)mineSpeed);
  playerData.setInt(2, "value", (int)luckMultiplier);
  playerData.setInt(3, "value", currentDepth);
  saveTable(oreInv, "data/oreInv.csv");
  saveTable(playerData, "data/playerData.csv");
}
```

This was explained earlier, it uses the built-in table functions to take the player data variables and place them in the player data table, and then it saves both the ore inventory and the player data to the game files. The reason only player data must be placed in the table is because the ores, when mined, go straight to the table instead of staying as game variables, which disappear once the game ends. This took some time to figure out as trying to figure out why the player data was not automatically being place in the loaded table was a bit confusing. We were able to figure it out by just going over how we chanegd player data mid game. 

# Conclusion

Overall, this assignment was quite fun. Having the option to make whatever game we wanted made it a lot more enjoyable, but it also made it a great learning experience. Rayan and I were able to experiment with new things we have not tried in Processing, like using tables and had a great time doing it.

In the future, I would like to polish this game up, by fixing loose ends in the code, making the visuals a bit more custom, and fleshing out more features. I think these 3 goals could be done with just a bit more time, but hopefully I am able to make a similar game, or better in the future.