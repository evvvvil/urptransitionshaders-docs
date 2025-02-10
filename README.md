# **URP Transition Shaders Documentation**

![Glorious URP transition](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/urp-transition-shaders-02.jpg?raw=true)
 
## What does it do?

Extends URP shaders to add a transiton feature to reveal / fade any object or group of objects.
Retains all URP material features intact such as: lighting, surface options, surface inputs.
Surgically adds transition feature into URP shaders without disturbing "that tasty URP sauce".

Please see the [demo video on youtube](https://www.youtube.com/watch?v=O7nvYtbpcAo)

![Glorious URP transition](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/urp-transition-shaders-01.jpg?raw=true)

# Features:
- Transparent smooth alpha transition fade
- Opaque transition with 'inside' colouring
- Seamless shader switch: material maps and values carry over, so you dont have to redesign your materials
- Transition along local or world axis
- Transitions shadows and depth as well
- Mirrored, slanted and other transition effects
- Use the provided URPTransitionGroup script to build your own logic
- Or create sequences to fade on start without writing any code
- Reveal huge part of a level in a sequence or just individual objects 
- "Silhouette" shader option to flash object to unlit plain colour

![Glorious URP transition](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/urp-transition-shaders-03.jpg?raw=true)

## How to use:

1. Import the URP-Transition-Shaders package. You should now have new shader options in any Unity material's `Shader` dropdown. In `Universal Render Pipeline` category you now have new transition shaders such as `Lit-Transition` and `Unlit-Transition`. :star_struck:

![New options available in material shader dropdown](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img01.png?raw=true)

![New options available in material shader dropdown](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img02.png?raw=true)

2. Create a new material in Unity or select an existing material which is using `Lit`.

3. Switch the material's shader from `Lit` to `Lit-transition`: click shader dropdown on the top of material and in `Universal Render Pipeline` category select `Lit-transition`.

4. If you are editing an existing `Lit` material then all the maps and values should stay the same. :rowing_woman: Your material's look will stay intact. You should now see a new fold out `Transition Options` in your material inspector panel.

5. Move the `Fader` slider to see the transition in effect. Change the `Scale` and `Position Offset` transition options to make sure your object fully hidden at fader value 0 and fully shown at fader value 1. If you have transparent material then change the `Gradient Length` at yer leisure.

6. If you are casting shadows then you should also change `Shadow scale` and `Shadow offset` separately from `Scale` and `Offset` above. That's because the shadow values will most likely have to be a bit different depending on your `Shadow cut off`, whether you have transparency and other factors. Start with the shadow scale and offset same as the scale / position offset values above and tinker with the values until you're happy the shadow transition animation matches your mesh transition animation. 

7. Add the provided script `URPTransitionGroup` as a component to the mesh that has your new transition material. Alternatively you can also add the script to a parent of multiple meshes which have transition materials. This will create a `URPTransitionGroup`, ready to be faded in / out.<br>NOTE: You can visually check / debug multiple material transitions by adding `URPTransitionGroup` to a parent of meshes and using the script's `Transition Fader` under "DEBUG IN EDITOR". This is handy to visualise all your children material transition in one push of a fader.

![URPTransitionGroup component](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img04.png?raw=true)

8. Once you have created a `URPTransitonGroup` you can either:
    - Write your own logic to transition/fade in or out your meshes by calling `URPTransitonGroup`'s transition functions `FadeIn(float duration)` or `FadeOut(float duration)`
    - Use `URPTransitonGroup` inspector UI options to create transition from start without having to write any code.

9. See the `ExampleTransition` scene in URPTransitionShaders/scenes/ExampleTransition for an example on how to use this package.

## Transition Easing:

You can set which type of animation easing for your material transition in the `URPTransitionGroup` component. Look for the `Transition Ease` drop down that sets which easing will be used with `Transition on start` as well as when you call `FadeIn(float duration)` or `FadeOut(float duration)`. Know yer easing: https://easings.net/

## Final Notes:

To achieve smooth transparency without a hard edge use Surface type `Transparent` with `Alpha` blend mode and UNTICK `Preserve specular lighting`.

In UNLIT and deferred, if you see black areas which should be transparent: turn on `Alpha clipping` and drop `Base Color` alpha channel to a value below the `Alpha Clipping` threshold.

## Transition Options:

![Transition options](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img03.png?raw=true)

**Fader**<br>
Animate from 0 to 1 to reveal the transition. This is your main transition 'fader'. Your object should be hidden at 0 and fully shown at 1. Change the `Scale` and `Position Offset` to achieve this, based on your `Axis`, `Direction`, etc.

**Mode**<br>
Transition uses local or world axis. Use world axis to transition multiple meshes along in a sequence after each other.

**Axis**<br>
Select which axis to do the transition along.

**Direction**<br>
Select which direction the transition will progress along the axis.

**Scale**<br>
Scale transition coverage to adjust amount of object revealed at the end of transition.

**Gradient Length**<br>
_This option is only available when surface  type is set to `Transparent`._<br>
Length of transition gradient. Lower number will make the transition gradient shorter.

**Position Offset**<br>
Offset start position of transition. Example: if object is centrally aligned (axis / center point at center) but you want to transition from the bottom then change this value down to shift transition start to bottom.

**Write Depth**<br>
Forces depth write on this material. Tick this to resolve depth issues with transparent meshes, such as when multiple overlapping meshes share the same transition material.

**Shadow Scale**<br>
Same as `Scale` but for shadows. It lets you adjust the shadow transition scale separately to fine tune the shadow transition and align it with the mesh transition. Value should be close to `Scale` above however it depends on your `Shadow Alpha Clipping` value.

**Shadow Offset**<br>
Same as `Position Offset` but for shadow. It lets you adjust the shadow position start offset separately to fine tune the shadow transition and align it with the mesh transition. Value should be close to `Position Offset`.

**Shadow Alpha Clipping**<br>
_This option is only available when surface render face is set to `Front` or `Back`._<br> 
Use shadow alpha clipping to fine tune the shadow's coverage. Same concept as `Alpha Clipping` in 'Surface Options', however this only affect the shadow and doesn't require `Alpha Clipping` to be on. Try a value of 0.5 to start off and tweak to get a thinner or wider shadow.

**Shadow Render face**<br>
_This option is only available when surface render face is set to `Front` or `Back`._<br> 
When rendering just the front or back face, shadows can appear cut off during transitions. Use this to override the render face setting for shadows to improve coverage. Flip the shadow render face or select both to ensure the shadow aligns with the mesh transition.

**Mirror Transition**<br>
Mirror transition will fade both axis directions. For example to reveal an object from its center outwards vertically and in both up and down direction.

**Mirror Offset**<br>
_This option is only available when `Mirror Transition` is ON._<br> 
Offset start of mirror effect. Useful when `Direction` is `Negative` - should be close to 0 when `Positive`. Change this to pull in or push out the start of mirror effect. Combine this value - to hide enough of object at the start of transition, and the scale value to reveal enough of the object at the end.

**Slant Transition**<br>
Slant / angle transition with a diagonal cut along another axis.

**Slant Axis**<br>
_This option is only available when `Slant transition` is ON._<br> 
Which axis to do the slant along.

**Slant Amount**<br>
_This option is only available when `Slant transition` is ON._<br> 
Slant amount, and also direction. The slant angle direction will depend on this value being positive or negative.

**Backface colour**<br>
_This option is only available when surface render face is set to `Both`._<br> 
You can colour the back face of your double sided object during the transition to give it a more tidy / stencil look.

**Backface colouring**<br>
_This option is only available when surface render face is set to `Both`._<br>
How much to colour the back face with the 'Backface Colour' defined above.

**Silhouette Tint**<br>
Tint final colour with a flat silhouette colour to create an unlit silhouette look.

**Silhouette Colour**<br>
_This option is only available when `Silhouette tint` is ON._<br> 
Which colour to use as the silhouette tint colour.

**Silhouette Amount**<br>
_This option is only available when `Silhouette tint` is ON._<br> 
How much to tint the final colour with the `Silhouette Colour` defined above.

## URPTransitionGroup Options:

![URPTransitionGroup options](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img04.png?raw=true)

**Action on start**<br>
What happens when the game starts:
-`Nothing` does nothing. 
-`Hide on start` hides all transition materials on start.
-`Transition on start` fades in all transition materials on start. Transition animation will have a duration of `Transition on start duration` and be delayed by `Transition on start delay`.

**Transition on start duration**<br>
Duration of the start transition (in seconds).

**Transition on start delay**<br>
Delay the start transition by some time before it starts (in seconds).

**Transition ease**<br>
Choose a type of easing for the transition animation.

**Transition children**<br>
Tick to also transition all children which have a transition material.

**DEBUG IN EDITOR - Transition fader**<br>
Use this slider to visually debug the transition animation.

**Update material list**<br>
If you change the object hierachy, such as changing children materials or adding new children, then please click this `Update material list` button to refresh the list of transition materials.<br>NOTE: You only need to worry about updating list of materials when in editor mode. When the game starts the list of material will be automatically updated.