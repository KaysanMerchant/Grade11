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
```

# Selection Structure

Selection structures were used in the code in various ways. One of the many ways we use it is to check game states. Very simply, we use selection structures like if statements to check if the game has started, if an ore animation has started, if thew shop is open and if the inventory is open. Below is how our sraw function is structured to check for these.

```java
void draw(){
  if (start){
    tint(depthUpgradeColors[currentDepth]);
    image(background, 0, 0, 800, 600);
    tint(255);
    startMenu();
  } else{
    if (!inventoryOpen && !shopOpen) {
      gameMenu();
    } else if (inventoryOpen) {
      drawInventory();
    } else if (shopOpen) {
      drawShop();
    }
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
  }
}
```

Other examples of our selection structure usage if for conditional activations, like in out custom function, checkObjectPressed. In checkObjectPressed, we check if the mouse is within certain boundaries, and if it is, the object gets a white overlay on it. It also activates a process if clicked.

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