---
title: Assignment 3
published_at: 2024-10-07
snippet: Finalising and references
disable_html_sanitization: true
allow_math: true
---

# Finalising Assignment 3

One of the technical issues that players tended to run into in my environment was that the mouse sensitivity was too high. I found out that there was actually a variable for this on the player character, so I changed this until the mouse sensitivity was more acceptable, which I found to be 1.

![mouse](/a3/mouse.png)

In the playtesting feedback, one respondant had remarked that "The many hills and having to go up and down, did feel a bit weird", which wasn't the feeling I intended to elicit. Furthermore, another had wrote that some trees were "easy to ignore" which I felt may be due to the hilly terrain covering some of the trees. I felt like the idea of growth may be clearer if the terrain was lower before the final hill, to juxtapose the player's height against them being towered over by the fences at the entrance. I evened out the terrain using the Raise and Lower terrain tool, holding Shift to lower the terrain. 

![terrain](/a3/terrain.png)

One of the playtesters had noted in the feedback that it was possible for the player to fall off of the edge of the terrain. I resolved this by creating 4 planes with an invisible material on each, and applying box colliders on them so the player couldn't walk through. I placed the four planes vertically around the 4 sides of the terrain.

![barriers](/a3/barriers.png)

Some of the playtesters didn't notice that they were meant to pick up the watering can and water the trees, so I added in some text by adding two TextMeshPro objects, parented to the Canvas object. The first messages says 'Press "E" to pickup the watering can' (I changed the picking up script to 'Input.GetKeyDown(E)' from 'MouseButtonDown(0)' to reflect this) and the second says 'Click on the trees to water them'.

![tmp1](/a3/tmp1.png)

I followed [this tutorial](https://www.youtube.com/watch?v=IqpgJlhtmoo&ab_channel=PixelbugStudio) to make the text 'type' on screen with an animated effect, to make it appear more natural and less sudden. While it used the TextMeshPro component, rather than having the text typed into the the TextMeshPro component like I had before, I had to type it into the script component instead as an element in an array. I had to set the variables of time between characters and time between words to dictate how fast the text would type: I eventually decided that 0.025 between characters and 0.25 between words was around as fast as the player would read the text. 

![tmp2](/a3/tmp2.png)

I made both TextMeshPro objects to inactive so that they wouldn't appear when the game starts. I created a cube infront of the watering can (to which I eventually applied an invisible material). I added a box collider component set to "Is Trigger" and a Rigidbody component, so that it could detect collisions and trigger things in the script. I made a script called "runtext", attached to the cube, which would set the 'Press "E"...' TextMeshPro object to active when a collision is detected with an object with the "Player" tag. 

![tmp3](/a3/tmp3.png)

I added a line to the picking up script that would make the 'Press "E"...' text inactive once the player picks up the watering can, and then make the 'Click on the trees...' TextMeshPro object active. I also added a line to the 'ChangeSizeScript' (which is what changes the size of the trees and calls the method to change the prefab), to make the 'Click on the trees..." TextMeshPro object inactive once the player grows a tree.

![pickup](/a3/pickup.png)

I was bothered by how the terrain textures were less stylised than the other objects in the environment, so I looked online for some free handpainted textures which I replaced them with. The tiling was quite obvious on the grass and the path, so I made some duplicates of those textures and changed the size and offsett coordinates of each, painting variations in the pattern in the 'Paint Terrain' tool to make them less predictable and obviously tiled. Particularly for the rock path, it was important to set the opacity to 100% so that the rocks didn't look like they were fading over one another.

![textures](/a3/textures.png)

Many of the playtesters did not identify that the environment was about becoming an adult. To make it more clear that the environment was about growing up, I needed to refine the 'black and white view' from the archway. From my research, I discovered a way to do this using a camera. First, I created a camera which looked out from the view of the archway. I could make an object which displayed the view of this camera by creating a Render Texture, and setting the "Output Texture" on the camera to that Render Texture. I made this Render Texture the albedo of a new material, and I applied the material to a plane which I placed inside the archway. To get the black and white effect, I used the same Global Volume I made before in [Week 9 Session 1 (Part 2)](https://jackreed050-dms1-blog-55.deno.dev/w9s1(2)) (this holds the post processing effect, and must be on the same layer as the camera). I kept the new camera on the Default Layer as with the Global Volume, turned on Post Processing in the new camera so the black and white effect would be applied, and turned off post processing on the Main Camera so that the effect wouldn't be applied to it. I reused the cube which triggered the original black and white effect, but changed the script attached to it so that it would set the plane in the archway to active when the player is colliding with the cube. This meant that the effect would not be seen from the front of the archway, and the view would only be black and white when the player was looking back. 

![vista](/a3/vista.png)
![vista1](/a3/vista1.png)

To further reinforce the idea that the meaning of the environment was becoming an adult, I created a final tree on top of the hill. I wanted this to be the 'adult' tree, so to display its maturity, in Blender I added apples (from a sketchfab asset) and flowers (which I created myself with sphere meshes) to the original 'grown tree' model. I then imported this model into Unity and set it to the reference for 'GrownTree' in the 'Replace Prefab' script ([from Week 9 Session 1 (Part 2)](https://jackreed050-dms1-blog-55.deno.dev/w9s1(2))) on the final tree.

![finaltree](/a3/finaltree.png)

It is important to use sound to create a 'sense of place', by materialising the things in the environment. I felt like it would be important to materialise the actions, picking up the watering can and watering the tree, by adding sounds to these. I found some audio in the YouTube Audio Library that represented the sound of these actions, and added Audio Source components to the watering can and all of the trees with these sounds set as the 'AudioClip' on these components. On the Picking up script and the Change size script, I used 'GetComponent.AudioSource().Play(0)' to get the audio source from each object (which I referenced in their script and components) and play the clip when the action took place. I set the Spatial Blend to 2D for each of these as they were not used for navigation, turned off loop as they were to play one shot, and turned off Play on Awake so they wouldnt play until triggered.

![materialise](/a3/finaltree.png)

While I wanted to convey a nostalgic, or meloncholic mood, this didn't come across to my playtesters. To help convey nostalgic and meloncholic emotions, I noted that the Minecraft soundtrack is successful at this for me. I considered using similar instrumentation to that used in the Minecraft soundtrack, such as lofi pianos and arpeggiated synths. I found an old beat I had made which was inspired by the Minecraft soundtrack and featured many of these sounds, so I exported loops with different sets of instruments (piano loop, synth loop, drums and bass and synth loop, drums and bass and synth and mellotron loop) and replaced the current clips (which were arranged on a [plane](https://jackreed050-dms1-blog-55.deno.dev/w8s2) so the soundtrack could evolve depending on the player's position) with these ones. As one of the technical issues one of the playtester's noted was the constant panning of the soundtrack due to this method, I made sure there was one loop playing the whole time (the piano), which would be playing from an Audio Source on the player controller, with the spatial blend set to 2D. I deleted some of the planes, as the ones before and right after the entrance were no longer needed due to the player controller being used as the main Audio source for this part. I arranged the audio clips so that an arpeggiated synth would be introduced when the player moved towards the watering can, and drums and bass would be introduced as the player walks along the dirt path to water the trees. Then, once the player gets to the top of the mountain, hi hats are introduced as well as a mellow sounding mellotron flute, to add a musical climax to this section. I also remade the 'pinging' sound coming from the trees with a synth in Ableton, to be more harmonious with the soundtrack, but made sure it looped at a different pace so it wouldn't be indistinguishable from the soundtrack, helping it stand out and draw the player to the trees.

![planes](/a3/planes.png)

After this, the sound became distorted in Unity Play (it sounded like it was clipping). I switched to a Mac Build, and it worked fine. Below is a video of me playing my environment:

<iframe width="560" height="315" src="https://www.youtube.com/embed/W1etzT0DvQw?si=XJBq_jw8jrQJcalM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# Resources Used

### Audio

* Soundtrack by me
* Tree 'pinging' sound by me
* Watering can sound: 'Water Can Fall' from YouTube Audio Library
* Watering trees sound: 'Water Leak' from YouTube Audio Library

### Textures
* Dirt path from [Handpainted Grass & Ground Textures](https://assetstore.unity.com/packages/2d/textures-materials/nature/handpainted-grass-ground-textures-187634?srsltid=AfmBOorKtQDVsjcnOQ_jgxHRYqu6hNy9QumHbZQHJxkDm2myZQF1Tnx) by Chromisu from Unity Asset Store.
* Grass and stone path from ['FREE handpainted game textures (toony) from KORDEX'](https://25games.itch.io/25games-textures) by 25games on itch.io.

### Models
* Trees from [Toon Tree 02](https://sketchfab.com/3d-models/toon-tree-02-d631da94d5c14b85b44d9a737fef883f) by Sergio Sotomayor on Sketchfab.
* Watering can from [A Soviet Watering Can](https://sketchfab.com/3d-models/a-soviet-watering-can-3b6560e03f02453288764b894c319098)
by Valger on Sketchfab.
* Fences from [Low-Poly Fence Asset Pack](https://sketchfab.com/3d-models/low-poly-fence-asset-pack-d242ecdbb1bc418d95058aeb79b69112) by Andrew Walsh on Sketchfab
* Apples from [Low-Poly Red Apple](https://sketchfab.com/3d-models/low-poly-red-apple-365819e9c1de43e6ab24147ee25d4833) by Jawlex on Sketchfab.
* Rocks, logs and tree 'bushes' from [Stylized forest - Scene "May holiday"](https://sketchfab.com/3d-models/stylized-forest-scene-may-holiday-c4baeda6e099461ab507b48245609eae) by Bársh on Sketchfab.
* Archway from [Stylised Low Poly Portals](https://sketchfab.com/3d-models/stylized-low-poly-portals-400f812a70f34292a928fa3d556c538a) by Dandushik on Sketchfab.
* Pedestal from [Stylised Pedestal](https://sketchfab.com/3d-models/stylized-pedestal-b9b93e2767594b3ab6dc7e46d87a3128) by Dablue on Sketchfab.







