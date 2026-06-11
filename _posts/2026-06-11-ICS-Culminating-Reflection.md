---
layout: post
title: "Intro to Computer Science Culminating Review"
author: Kaysan Merchant
date: 2026-06-11
---

# Overview

In this review, I cover topics about a game made in processing code by Rayan Bashir and myself, called Mom's Backyard. The game is an RNG game where you mine underground, getting random ores that you can sell for money to buy upgrades. The ultimate goal of the game is to obtain a Tuntungtungsanite, which with base odds is a 1/17025 chance.

I will be going over variables & data tracking, selection structures, repitition structures, arrays & data structures and use of custom functions & error checking.

# Variables & Data Tracking

To begin with, the variables used vary widely. The main variable types used are int, float and boolean, with some, but few strings. Of course, we include arrays of these types in various ways.

## Integer Variables

Integer variables are used the most. They are used for upgrade levels, player data, ore weights, ore IDs, and couple of more functions. Integers were used for ore IDs because it was easily callable to be used in oreAnimations, invetories and for selling. For player data, it was used to be easily altered and used in actual modifiers for the game(e.g. making mineSpeed make the ore animation go faster as seen below).

```python
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