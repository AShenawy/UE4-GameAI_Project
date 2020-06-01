# UE4: Game AI Project

This project implements AI Behaviour Tree and Blackboard included in Unreal Engine 4. This is made for the 'Artificial Intelligence for Games' course in Tallinn University.

### Prerequisites

* UE4 editor version 4.23
* Github Desktop (optional)

## Running the tests

There are passive and active tests included in the project. Each of these tests is meant to check that all AI conditions exist and have been accurately implemented.
To run them, go to Window -> Developer Tools -> Session Frontend, then click on the 'Automation' tab and click 'Run Level Test'. The included tests will then run automatically with each test ending in 'success' or 'failed'. If the test fails, some information will be shown in the lower Automation Test Results message window.
With the current build, all tests were passed with success by the AI implementation.

## Built With

* All code was built using UE4's visual scripting blueprints.
* Specific AI behaviour was built using UE4's Behaviour Tree, Blackboard, Environment Query System (EQS) and blueprints.

AI blueprints and Behaviour Tree are commented within the project as well.

## AI Behaviour

In this project, there are 5 AI-controlled characters. The level goes through 5 different stages as follows:
1- Running along a specific set path
2- Collecting objects (represented as coins) randomly generated in the level area
3- Running up to behind each other to try landing a sneak/back attack
4- Grouping with teammates at safety points
5- End of level and final score calculation

### Stage 1 - Run, Forrest, Run!

In this stage the AI characters are given a target point along a set spline, and are expected to run towards that point. While that point is the active target and they're running towards it, they are also looking up the next point to move to after reaching the current one. This 'parallel' calculation ensures the characters are continuously running without stuttering; which can be caused by the character running to a point, stopping, finding the next point then running again - it's most obvious when the target points are very close to each other, like in our level.

### Stage 2 - Collect Coins

Here, 9 coins are spawned randomly on the ground area every 6 seconds. The AI characters are required to use their site perception (included in UE4) to locate all coin objects. If they cannot see any coins within their sight radius (180 degrees), they will turn around to look behind them. Once they sight 2 or more coins, the EQS for this step kicks in: It measures the distances to all perceived coins, then scores each coin based on how far it is from the character. The nearer a coin is to the character, the higher the score it receives. The AI then chooses the coin with the highest score - i.e. nearest to the character and runs towards it to pick it up. It waits a very short time (simulating a human player looking around) and repeats the process again.
It is possible for a character to not run to any coin. This case happens when all spawned coins are too far away from the character and beyond their sight range (currently at 1000 units).

### Stage 3 - Sneak Attack

At this stage, the AI characters are expected to move towards each other but only if the other character they sight has their back to them - simulating a sneak attack from the back. The main functionality is similar to the coin collection, with a few exceptions:
* The AI character cannot sneak behind the same 'enemy' twice; this is to avoid having it stuck behind one enemy and not moving away so long as the stage is active
* Although the character can turn around (like in the coin collection stage) if it doesn't see an enemy in front of it, this action is randomised here and when the AI isn't allowed to perform it, it instead picks a random location point anywhere in the level and moves to that point before trying again to find an enemy. This is to simulate a wandering around behaviour, more typical to a human player.
* The AI character now needs to do two checks in the EQS, instead of the one in the coin collection. First, it needs to be able to see an enemy in front of it. Second, the enemy must be looking the opposite way and have their back facing the AI character. Without this rule, the attack will be head on, and can't be considered sneaky anymore.

### Stage 4 - Retreat with your Ally

In this final stage, each 2 characters (including the player) are assigned a team and matching colours. The AI is expected to choose one of the safety/end point spots and stand there with their teammate. To do this, each character first finds their teammate, then they find the nearest End Point to both of them and run to that point. Once they get there, they wait until the final stage.

### Stage 5 - Finish

No AI behaviour takes place here. The scores of all 4 previous stages is summed and displayed and the game (and tests if running) end.


## Team Members

* Anastasiia Tecepeleva
* Daniel Gumnikow
* Ahmed Mohamed Said Anwar ElShenawy

## Acknowledgments

* AI for Games video playlist by Roman Gorislavski