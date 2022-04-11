# Unity Sorting Visualisation
a visualization tool of sorting algorithms made by Unity.

a personal project.

## How to Open
- go to https://leiqiaozhi.github.io/UnitySortingVisualisation/ to experience the online build

---

## Features
### Algorithms
This visualization tool covers 2 main type of sorting algorithms

1. **Comparison Sort**
- Insertion Sort
- Selection Sort
- Bubble Sort
- Quick Sort

2. **Linear Sort**
- Counting Sort
- Radix Sort

### List/Array Generation
This visualization tool is able to generate 3 types of random lists/arrays for comparison sorts

1. Completely Random
2. Mostly Sorted
- the randomness can be adjusted in settings
3. Reversed

### Adjustable Speed
Speed of animation can be adjusted in the settings menu
- Duration of animations of swapping and color changes are adjustable.
- Set the durations of animations to 0 to have minimal animation. 
- *Note that the speed of animation does not accurately reflect the true speed of the algorithms*

## How I Made It
As implied by the title, I made it entirely using the Game Engine Unity. I then deployed the web build on github pages.

All the 'white bars'(I call them 'items') are gameobjects that have a `SpriteRenderer` component attached to it. 
They have no scripts attached, since there might be thousands of them and that would be inefficient.
Instead, I have a `ItemManager` component that handles all the items. 
- the `ItemManager` component has a list of item gameobjects.
- it provides APIs that process and animate the items, such as swapping the items and changing their colors, so that sorting scripts do not need to worry about the items. 

Similarly, there is a `UIManager` component that handles the UI side of things, such as the debug texts that can be displayed at the bottom of the screen. 

There is a script for each sorting algorithm, and each of them inherits from an abstract class `ISort` (it used to be an interface thus the name).
The abstract class has abstract methods:
- assigning references to `ItemManager` and `UIManager`, so that sorting methods can call their APIs. 
	*e.g. calling `uiManager.displayText(...)` to display a debug text*
- a `Sort()` method that takes in a list of floats and returns nothing since sorting is in-place.
- I call a **Coroutine** for every sorting algorithm, since it allows me to chain actions and decouple animation code easily.
	- I can call `yield return {AnimationCoroutine}` to wait for an animation to finish.

Lastly, I have a `SortManager` to manage all the sorting alogrithms. It creates an instance of each sorting algorithm class at `Start` and then calls `Sort()` on the right one depending on user selection.
 
