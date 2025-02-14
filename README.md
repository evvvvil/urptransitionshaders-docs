# **URP Transition Shaders Documentation**

![Glorious URP transition](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/urp-transition-shaders-main.jpg?raw=true)
 
## What does it do?

Users can transition, reveal and fade meshes along an axis. Extends all major URP shaders, such as `Lit`, while maintaining key shader features: lighting, surface inputs, etc. Works with shadows!<br>
TL;DR: Surgically adds wicked transitions into URP shaders without disturbing "that tasty URP sauce".

Please CTRL / CMD click to [see demo video in a new tab](https://www.youtube.com/watch?v=O7nvYtbpcAo).

![Glorious URP transition](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/urp-transition-shaders-01.jpg?raw=true)

## Features:
- Transition, reveal, fade in or out any mesh or group of meshes
- Keeps all URP shader features intact, such as lighting and material options
- Transitions shadows and depth as well
- Transition along local or world axis
- Reveal huge parts of a level in a sequence or individual objects
- Transparent smooth alpha transition fade
- Opaque sliced transition with 'inside' colouring
- Seamless shader switch: I.E: existing "Lit" shader material maps & values carry over when switching to "LIt-Transition" shader
- Mirrored, slanted and other transition effects
- C# class `URPTransitionGroup` provided to help you build your own logic
- Or create sequences to fade on start without writing any code
- "Silhouette" shader option to flash & highlight a mesh with a plain colour (extra)
- Written in pure HLSL shader code - no overheads
- SRP batcher compatible

![Glorious URP transition](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/urp-transition-shaders-03.jpg?raw=true)

## How to use:

1. Import the "URP Transition Shaders" package. You will then have new shader options available for any Unity material. In the `Shader` dropdown of any material, in the `Universal Render Pipeline` category, new shaders are: `Lit-Transition`, `SimpleLit-Transition`, `ComplexLit-Transition`, `BakedLit-Transition`  and `Unlit-Transition`. :star_struck:

![New options available in material shader dropdown](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img-01.jpg?raw=true)

![New options available in material shader dropdown](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img-02.jpg?raw=true)

2. Create a new material in Unity or select an existing material which is using `Lit` / `Unlit` / `SimpleLit` / `ComplexLit` / `BakedLit`. (For the purpose of this step by step guide, we will assume we are using `Lit`)

3. Switch the material's shader from `Lit` to `Lit-Transition`: click shader dropdown on the top of material and in the `Universal Render Pipeline` category, select `Lit-Transition`.

4. If you are editing an existing `Lit` material then all the maps and values should stay the same. :rowing_woman: Your material's look will stay intact. You should now see a new fold out `Transition Options` in your material inspector panel.

![URPTransitionGroup component](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img-transition-options.jpg?raw=true)

5. Move the `Fader` slider to see the transition in effect. Change the `Scale` and `Position Offset` transition options to make sure your object is fully hidden at fader value 0 and fully shown at fader value 1. If you have transparent material then change the `Gradient Length` at yer leisure.

6. If you are casting shadows then you should also change `Shadow scale` and `Shadow offset` separately from `Scale` and `Offset` above. That's because the shadow values will most likely have to be a bit different depending on your `Shadow cut off`, whether you have transparency and other factors. Start with the shadow scale and offset same as the scale / position offset values above and tinker with the values until you're happy the shadow transition animation matches your mesh transition animation. 

7. Add the provided script `URPTransitionGroup` as a component to the mesh that has your new transition material. Alternatively you can also add the script to a parent of multiple meshes which have transition materials. This will create a `URPTransitionGroup`, ready to be faded in / out.<br>NOTE: You can visually check / debug multiple material transitions by adding `URPTransitionGroup` to a parent of meshes and using the script's `Transition Fader` under "DEBUG IN EDITOR". This is handy to visualise all your children material transition in one push of a fader.

![URPTransitionGroup component](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img04.png?raw=true)

8. Once you have created a `URPTransitonGroup` you can either:
    - Write your own logic to transition / fade in or out your meshes by calling `URPTransitonGroup`'s transition functions `FadeIn(float duration)` or `FadeOut(float duration)`
    - Use `URPTransitonGroup` inspector UI options to create transition from start without having to write any code.

9. See the `ExampleTransition` scene in URPTransitionShaders/scenes/ExampleTransition for an example on how to use this package.

## Transition Easing:

You can set which type of animation easing for your material transition in the `URPTransitionGroup` component. Look for the `Transition Ease` drop down that sets which easing will be used with `Transition on start` as well as when you call `FadeIn(float duration)` or `FadeOut(float duration)`. [Know yer easing](https://easings.net/), yeah? 

## Smooth transparency and depth:

To achieve smooth transparency without a hard edge use surface type `Transparent` with `Alpha` blend mode and UNTICK `Preserve specular lighting`.
Depending on your scene, you might get depth issues due to transparency, such as when multiple overlapping meshes share the same `Transparent` surface materials. Tick `Write depth` to force depth write on your transition materials to ressolve meshes overlapping.

## Shadow Render face

When rendering just the front or back face of a mesh, it's possible for shadows to appear cut off / half missing during the transitions. See image below as an example:

![URPTransitionGroup component](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img-shadow-01.jpg?raw=true)

Use `Shadow render face` to override the render face setting for shadows. Flip to `Back` or select `Both` to fix the shadow coverage. See image example below with `Shadow render face` set to `Back` and `Render face` set to `Front`:

![URPTransitionGroup component](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img-shadow-02.jpg?raw=true)

##  Unlit and deferred rendering quirk:

When using `Unlit-Transition` in DEFERRED rendering mode, if you see dark areas which should be transparent: turn on `Alpha clipping` and drop `Base Color` alpha channel to a value below the `Alpha Clipping` threshold. For example with `Alpha clipping` threshold of 0.5, drop the `Base Color` alpha channel to just below 50% and the dark area will become transparent.

## Transition Options:

![Transition options](https://github.com/evvvvil/urptranstionshaders-docs/blob/main/images/Img03w.png?raw=true)

**Fader**<br>
Animate from 0 to 1 to reveal the transition. This is your main transition 'fader'. Your object should be hidden at 0 and fully shown at 1. Change the `Scale` and `Position Offset` to achieve this, based on your `Axis`, `Direction`, etc.

**Mode**<br>
Transition uses local or world axis. Use world axis to transition multiple meshes along in a sequence after each other.

**Axis**<br>
Select which axis to do the transition along.

**Direction**<br>
Select which direction the transition will progress along the axis.

**Scale**<br>
Scale transition coverage to adjust amount of object revealed at the end of transition. This value influences how much to show when `Fader` value is 1.

**Position Offset**<br>
Offset start position of transition. This value influences how little to show when `Fader` value is 0. Example: if object is centrally aligned (axis / center point at center) but you want to transition from the bottom then change this value down to shift transition start to bottom.

**Gradient Length**<br>
_This option is only available when surface  type is set to `Transparent`._<br>
Length of transition gradient. Lower number will make the transition gradient shorter.

**Write Depth**<br>
Forces depth write on this material. Tick this to resolve depth issues with transparent meshes, such as when multiple overlapping meshes share the same transition material.

**Mirror Transition**<br>
Mirror transition will fade both axis directions. For example to reveal an object from its center outwards vertically and in both up and down direction.

**Mirror Offset**<br>
_This option is only available when `Mirror Transition` is ON._<br> 
Offset start of mirror effect. Useful when `Direction` is `Negative` - should be close to 0 when `Positive`. Change this to pull in or push out the start of mirror effect. Combine this value - to hide enough of object at the start of transition, and the scale value to reveal enough of the object at the end.

**Shadow Scale**<br>
Same as `Scale` but for shadows. It lets you adjust the shadow transition scale separately to fine tune the shadow transition and align it with the mesh transition. Value should be close to `Scale` however it depends on your `Shadow Alpha Clipping` value.

**Shadow Offset**<br>
Same as `Position Offset` but for shadow. It lets you adjust the shadow position start offset separately to fine tune the shadow transition and align it with the mesh transition. Start at 0 and tinker the value, which depends on your `Shadow Alpha Clipping` value.

**Shadow Alpha Clipping**<br>
Use shadow alpha clipping to fine tune the shadow's coverage. Same concept as `Alpha Clipping` in 'Surface Options', however this only affect the shadow and doesn't require `Alpha Clipping` to be on. Try a value of 0.5 to start off and tweak to get a thinner or wider shadow.

**Shadow Render face**<br>
_This option is only available when surface render face is set to `Front` or `Back`._<br> 
When rendering just the front or back face of a mesh, it's possible for shadows to appear cut off / half missing during the transitions.<br> Use `Shadow render face` to override the render face setting for shadows. Flip to `Back` or select `Both` to fix the shadow coverage.

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
- `Nothing` does nothing. 
- `Hide on start` hides all transition materials on start.
- `Transition on start` fades in all transition materials on start. Transition animation will have a duration of `Transition on start duration` and be delayed by `Transition on start delay`.

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

## URPTransitionGroup Public Methods:

**FadeIn(float duration)**
Start the transition fade from 0 to 1, animated over a duration of `duration` (in seconds) AND using the `Transition Ease` set above.

**FadeOut(float duration)**
Start the transition fade from 1 to 0, animated over a duration of `duration` (in seconds) AND using the `Transition Ease` set above.