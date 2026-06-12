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

Integer variables are used the most. They are used for upgrade levels, player data, ore weights, ore IDs, and couple of more functions. Integers were used for ore IDs because it was easily callable to be used in oreAnimations, invetories and for selling. For player data, it was used to be easily altered and used in actual modifiers for the game(e.g. making mineSpeed make the ore animation go faster as seen below).

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

Selection structures were used in the code in various ways. One of the many ways we use it is to check game states. Very simply, we use selection structures like if statements to check if the game has started, if an ore animation has started, if thew shop is open and if the inventory is open. Below is how our sraw function is structured to check for these.

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
    if (i < 5) {
      drawInventoryOreSlot(300+(i%5)*(480/5), 20, i);
    } else if (i < 10) {
      drawInventoryOreSlot(300+(i%5)*(480/5), 20+(560/5), i);
    } else if (i < 15) {
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

There are other ways to use tables, like using a function to get or set a string or a float instead of an integer, but our uses only required integers.

Arrays are also used in our code, for loading pickaxe images and choosing the pickaxe image needed at any point.

# Use of Custom Function & Error Checking/Restrictions

