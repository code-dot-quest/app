# Architecture (V1)

## Purpose

We are building an HTML game running on browsers called Code.quest that teaches children and adults of all ages coding in a gamified way. The game is divided into quests where each quest is divided into chapters and then into challenges. The player needs to complete every challenge by using blocks to solve for a certain coding goal.

In every challenge, there would be a gamified simulation showing characters in some theme, like medival - similar to the stage in Microsoft MakeCode Arcade. The blocks that the player combines will allow the characters in the simulation to do various actions, for example pick up a sword or fight.

To code, the player will always use blocks and will never type directly. To teach multiple languages, the player will be able to select the flavor of blocks: (1) Natural language (code in English blocks). (2) JavaScript blocks. (3) Python blocks.

## Dependency Stack

* TypeScript - Most code will be written in TypeScript but I want to be able to have some files in JavaScript since I plan to import some files that are not necessarily fully TypeScript compliant.

* Vite - As the build tool, I would like to use Vite to build the website. It should be configured with hot reloading to allow for a good developer experience.

* Phaser - The simulation looks like a game with sprites and animations. The core of the simulation should use a game engine. Phaser is a modern HTML game engine.

* Blockly - Since coding in the game happens by selecting and combining blocks and not by typing code, the heart of the blocks engine will be the Blockly library by Google.

* JS-Interpreter - When a block-based program runs as part of the simulation, it will run inside a JavaScript interpreter. This will allow to stop endless loops and support single step debugging.

* React - I don't want to rely exessively on React as a UI framework in the attempt of making the game as simple as possible. I'm thinking about a shallow React shell where the simulation and block panel could be components.

* Zustand - For simple state management of the game I'd like to rely on Zustand and try to make this reliance as decoupled from React as possible.

* Vitest - I would like to be able to cover some specs with tests and rely on Vitest library for these tests.

## Implementation Goals

* Stage - A TypeScript class that abstracts the simulation. It receives JavaScript code in its constructor and allows to run this code (either all at once or single-step). It also allows to listen to events like the completion of a puzzle.

    * Phaser is an implementation detail of this class, I want to be able to replace Phaser in the future and the public interface of this class would not change.

    * JS-Interpreter is an implementation detail of this class, I want to be able to replace to a different interpreter and the public interface of this class would not change.

* Block library - A collection of blocks available for usage in challenges. Blocks are divided to natural language blocks, JavaScript blocks and Python blocks.

    * Every block has a Blockly definition of how it acts and two generators - one for interacting with the simulation (Stage) in JavaScript and one for printing the generated code in a pretty way. 
    
    * A Python block, for example, would have a JavaScript generator that interacts with the Stage and a pretty print generator that shows Python syntax to the player.

* Tile and Game Object library - A collection of game objects that make up the simulation (Stage). The library includes the graphics for each game object and the behavior in code (JavaScript classes).

    * The background of the Stage is made of tiles. Each tile specifies characteristics in code for example which characters can walk over this tile.

    * The Stage can hold game objects like houses, swords, knights. A game object has graphics and animations and coded behaviors (JavaScript implementation).

## Design Goals

* Testability - I want to be able to test the Stage and allow running of a challenge, supplying a viable solution for this challenge and checking that the code solution indeed solves the challenge in a test.