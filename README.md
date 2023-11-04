# ASSA VR Tutorial

## Introduction
Welcome to the tutorial of the 5th and last day of the Autumn School Series in Acoustics (ASSA) Acoustic Signal Processing school: Signal Processing for Acoustic Virtual Reality. 
In this tutorial, we will use Unity together with Steam Audio and explore its various capabilities.

This tutorial is based on a previously given workshop by Erik Frej Knudsen and Helmer Nuijens which can be found via: https://github.com/efrej/SpatialAudioWorkshop.

## 1. Download Unity and open the project
- Download Unity Hub https://unity.com/download
- Download this repository to your computer.
- Open the Unity Hub and select 'Open'.
- Navigate to the repository you just downloaded and select the root-folder (containing folders such as Assets, Packages, etc.)
- Unity Hub will probably warn you that you have a 'Missing Editor Version'. Allow Unity Hub to Install Version 2021.1.19f1. This is the version that this tutorial will use.
- Select install (you do not have to tick any boxes other than those that are ticked).
- Open the ASSA_VR_Tutorial project (this may take a few minutes).
- In the project window, navigate to Assets/Scenes and open DemoScene.

## 2. Set up Steam Audio
- Download the Steam Audio Unity package from: https://valvesoftware.github.io/steam-audio/ (documentation can be found here https://valvesoftware.github.io/steam-audio/doc/unity/index.html)
- Unzip the folder.
- Add the package to Unity: Assets → Import Package → Custom Package
- Navigate to the Steam Audio package you just downloaded and then to the following file: steamaudio_unity/unity/SteamAudio.unitypackage.
- Select Import when Unity asks you to.
- Update project settings to use Steam Audio: Edit → Project settings → Audio and select Steam Audio for the Spatializer Plugin and Ambisonics Decoder Plugin.
- For each Audio Source, enable “Spatialize” in the Audio Source Component:
    - In the Hierarchy, select Speaker1 / PhoneBootStand_02 / Speaker2.
    - In the Inspector, find the Audio Source component
    - Tick the Spatialize box. 
- For each audio source add a Steam Audio Source:
    - Select Speaker1 / PhoneBootStand_02 / Speaker2 in the Hierarchy.
    - At the bottom of the inspector select Add Component → Steam Audio → Steam Audio Source. 

## 3. Explore!
3.1. Play the scene and listen, each audio source should be spatialized using Steam Audio using HRTF decoding. You will be able to hear when sounds are in front, back up, and down. 

3.2. Add Directivity 
- Select Speaker1 / PhoneBootStand_02 / Speaker2 and find the Steam Audio Source in the inspector.
- Enable directivity. Choose "Simulation defined" and play around with the dipole weight and power. The weight enables you to blend between monopole or dipole and the power defines how focused the directivity is. 

Play the scene and listen, you should now hear a difference when you move around an object.

3.3 Add Steam Audio Geometry

Right now, the geometry does not do anything to the sound yet. We can change this by adding a Steam Audio Geometry object to the environment.
- Select the Environment Game Object in the Hierarchy.
- In the Inspector select Add Component → Steam Audio Geometry.
- Tick Export All Children (you should see that the geometry has 1770 vertices and 1210 triangles).
    - Note that only objects with a mesh filter component will be included (these are automatically added when adding a standard 3D object in Unity).
- From the menu, select Steam Audio → Export Active Scene (the .asset file can be saved in the Asset folder). This creates a .asset file that stores the information of the selected geometry.
- You might notice that the Default Steam Audio Material is selected. You can choose any other by clicking on the circle in the Material selection box.
    - Note that whenever you change a material of a geometry, you will have to repeat the Export Active Scene step as explained before.

3.4. Add Occlusion

Now that we have a Steam Audio Geometry, we can start adding occlusion.

- Enable Occlusion in a Steam Audio Source Component and choose Simulation Defined.
- Choose Occlusion Type
    - Raycast will use a single ray from the listener to the emitter, when this is occluded the source is considered occluded. This works alright for occlusion of rooms, but in order to get realistic occlusion of objects or person in the room choose volumetric.
    - Volumetric uses multiple rays based on the occlusion radius. When the radius is larger (larger sound sources), an object must be larger in order to fully occlude a sound source. Occlusion samples determine the amount of rays that are used. More rays means a smoother transition between no occlusion and full occlusion.
    
Play the scene and listen, sound sources should be occluded when an object is blocking the sound source.

3.5 Add Transmission (optional)

Transmission can also be added.
- Enable Transmission in a Steam Audio Source Component.
- If checked, a filter based on the Steam Audio Material of the occluding scene geometry will be applied to the Audio Source. For this, you can choose frequency independent or frequency dependent filtering. The way that the sound is filtered is determined by the Steam Audio Material which you gave to the geometry. 

3.6. Add Reflections
- Enable Reflections in a Steam Audio Source Component (preferably Speaker2 as it is surrounded by walls).
    - (optional) The reflections can be spatialised by selecting Apply HRTF To Reflecions enabling HRTFs, with the cost of (slightly) increased CPU load. 

Play the scene and listen, you should now be able to hear indirect sounds reflected in the scene. If it is not as apparent, try to change the amount of reverb using the Reflections Mix Level slider, or set the Direct Mix Level slider to 0.
