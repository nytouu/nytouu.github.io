# Intradiegetic menu

For the in-game pause menu in DERAYAH, our first idea was a minimal 2D pause menu, we ended up scrapping this idea as it was too simple, not original and limited our creativity to make something that would satisfy the team. For that reason I proposed an idea : a 3D intradiegetic menu, with cloud-like buttons and text formed by particles of sand.

We kept that idea as it was original and it also made so that the first person view would not be interrupted, which was nice since that is also the case for the puzzles.

## Conception

The first step for creating the menu was sketching that idea, I did a few sketches on GIMP to show the menu in its different stages and how it should animate.

![PauseMenuSketch.gif](https://raw.githubusercontent.com/nytouu/nytouu.github.io/refs/heads/master/Showcases/TechArt/Images/PauseMenuSketch.gif)

After confirming what would end up in the final menu, I started actually creating the VFX in Unity using the VFX Graph. The goal was to create first the visual aspect of the VFX and then make it interactible in game.

<video src="https://github.com/user-attachments/assets/a04cc680-3525-45a0-ac0c-6428b5f348f6" controls="controls" style="max-width: 500px;">
</video>

The menu animation has different stages :
- First, the physics are stopped and the world fades to grayscale
- Sand particles spawn from the bottom with upwards velocity
- These particles agglomerate to form text representing the buttons
- A small cloud of sand appear underneath the text

## Visual aspect of the effect

I created this effect using Unity's VFX Graph, the graph is separated into 3 systems :
- The sand particles spawning
- The sand particles forming the text
- The cloud particles underneath the text

For the first part of this effect, I mostly follow this [tutorial](https://www.youtube.com/watch?v=ZytOQ4NSciU) which demonstrates how to make a similar effect where particles agglomerate towards a predefined shape (in this case a Signed Distance Field - or SDF for short). In this case, Unity provides a convenient tool to convert a 3D mesh into a .asset file containing the SDF, we use that to create 3 SDFs with ou text shapes (Resume, Options and Quit).

When we spawn our particles, we give them upward force as well as turbulence for a more organic movement. We add an additional force towards the SDF so that the particles agglomerate to form the shape of the text.

These particles will then fade out and another system wiil appear, forming the text. The reason why I chose to fade out the first system is because some particles would not properly get attracted to the SDF. This way we can get a more consistent look on the text, making it more readable.

Then we spawn a small, darker cloud underneath the text, that way the text can contrast more with a small background.

## Making the buttons interactive

This was very simple, I gave each button a Box Collider (since the buttons are World Space) and the hit detection is done with a Raycast. Whenever the ray hits the button, we lerp text color to the selected color.

![WorldButtonInspector.png](https://github.com/nytouu/nytouu.github.io/blob/master/Showcases/TechArt/Images/WorldButtonInspector.png?raw=true)

Then to make everything work, I made a prefab that spawns the 3 buttons whenever you hit Escape.
