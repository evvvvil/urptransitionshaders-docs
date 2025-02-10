# **URP Transition shaders documentation**
 
## What does it do?

Extends URP shaders to allow users to transition / reveal / fade any object.

Retain all URP material features such a lighting, surface options, surface inputs, etc...
The idea isn't to rewrite URP shaders such as lit and unlit but to surgically add the transition feature while retaining all the URP features intact.

## How to use:

Import the URP-Transition-Shaders package. You should now have new shader options in any Unity material's "Shader" dropdown. In "Universal Render Pipeline" menu you now have new transition shaders such as "Lit-Transition" and "Unlit-Transition".:star_struck:

![New otpions available in material shader dropdown](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/Screenshot01.png?raw=true)

![New otpions available in material shader dropdown](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/Screenshot03.png?raw=true)


1. Create a new material in Unity or select an existing material which is using "Lit".

2. Switch the material's shader from "Lit" to "Lit-transition": click shader dropdown on the top of material and in  "Universal Render Pipeline" menu select "Lit-transition".

3. If you are editing an existing "Lit" material then all the maps and values should stay the same. Your material's look will stay intact. You should now see a new fold out "Transition Options" in your material inspector panel,

4. Move the "Fader" slider to see the transition in effect. Change the "Scale" and "Position Offset" transition options to make sure your object fully hidden at fader value 0 and fully shown at fader value 1. If you have transparent material then change the "Gradient Length" at yer leisure.

5. If you are casting shadows then you should also change "Shadow scale" and "Shadow offset" separately from "Scale" and "Offset" above. That's because the shadow values will most likely have to be a bit different depending on your "Shadow cut off", whether you have transparency and other factors. Start with the shadow scale and offset  same as the scale / offset values above and tinker with the values until you're happy the shadow transition animation matches your mesh transition animation. 

6. Add the provided script "URPTransitionGroup" as a component to your

## Transition Options:

**Fader** 

Animate from 0 to 1 to reveal the transition. This is your main transition 'fader'. Your object should be hidden at 0 and fully shown at 1. Change the 'Scale' and 'Position Offset' to achieve this, based on your 'Axis', 'Direction', etc.

**Mode**

Transition uses local or world axis. Use world axis to transition multiple meshes along in a sequence after each other.

**Axis**

Select which axis to do the transition along.

**Direction**

Select which direction the transition will progress along the axis.

**Scale**

Scale transition coverage to adjust amount of object revealed at the end of transition.

**Gradient Length**

This option is only available when surface  type is set to 'Transparent'. 
Length of transition gradient. Lower number will make the transition gradient shorter.

**Position Offset**

Offset start position of transition. Example: if object is centrally aligned (axis / center point at center) but you want to transition from the bottom then change this value down to shift transition start to bottom.

**Write Depth**

Forces depth write on this material. Tick this to resolve depth issues with transparent meshes, such as when multiple overlapping meshes share the same transition material.

**Shadow Scale**

Same as 'Scale' but for shadows. It lets you adjust the shadow transition scale separately to fine tune the shadow transition and align it with the mesh transition. Value should be close to 'Scale' above + 'Shadow Alpha Clipping' value.

**Shadow Offset**

Same as 'Position Offset' but for shadow. It lets you adjust the shadow position start offset separately to fine tune the shadow transition and align it with the mesh transition. Value should be close to 'Position Offset'.

**Shadow Alpha Clipping**
_This option is only available when surface render face is set to 'Front' or 'Back'._<br> 
Use shadow alpha clipping to fine tune the shadow's coverage. Same concept as 'Alpha Clipping' in 'Surface Options', however this only affect the shadow and doesn't require 'Alpha Clipping' to be on. Try a value of 0.5 to start off and tweak to get a thinner or wider shadow.

**Shadow Render face**

_This option is only available when surface render face is set to 'Front' or 'Back'._<br> 
Only available when 'Render Face' in 'Surface Options' is not set to 'Both'. When rendering just the front or back face, shadows can appear cut off during transitions. Use this to override the render face setting for shadows to improve coverage. Flip the shadow render face or select both to ensure the shadow aligns with the mesh transition.

**Mirror Transition**

Mirror transition will fade both axis directions. For example to reveal an object from its center outwards vertically and in both up and down direction.

**Mirror Offset**

_This option is only available when mirror transition is ON._<br> 
Offset start of mirror effect. Useful when 'Direction' is 'Negative' - should be close to 0 when 'Positive'. Change this to pull in or push out the start of mirror effect. Combine this value - to hide enough of object at the start of transition, and the scale value to reveal enough of the object at the end.

**Slant Transition**

Slant / angle transition with a diagonal cut along another axis.

**Slant Axis**

_This option is only available when slant transition is ON._<br> 
Which axis to do the slant along.

**Slant Amount**

_This option is only available when slant transition is ON._<br> 
Slant amount, and also direction. The slant angle direction will depend on this value being positive or negative.

**Backface colour**

_This option is only available when surface render face is set to 'Both'._<br> 
You can colour the back face of your double sided object during the transition to give it a more tidy / stencil look.

**Backface colouring**
_This option is only available when surface render face is set to 'Both'._<br>
How much to colour the back face with the 'Backface Colour' defined above.

**Silhouette Tint**

Tint final colour with a flat silhouette colour to create an unlit silhouette look.

**Silhouette Colour**

Which colour to use as the silhouette tint colour.

**Silhouette Amount**

How much to tint the final colour with the 'Silhouette Colour' defined above.