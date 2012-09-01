demo.ridinglight
================

## Intro

This post is a *How To*. We get a demo and we looks how to code it.
The demo we picked is *"Riding a Beam of Light"*. Looks at it 
[live](http://jeromeetienne.github.com/demo.ridinglight/).


The code is on [github](https://github.com/jeromeetienne/demo.ridinglight)
under [MIT License](http://jetienne.mit-license.org). Have fun and play with it :)

Here is a screencast. It explains how the demo has been done. 
I based this post on the content of this video, so expect similarities.

<iframe width="420" height="315" src="http://www.youtube.com/embed/X25Cn90RwhU" frameborder="0" allowfullscreen></iframe>

## Code WalkThru

	// init the world
	var world	= tQuery.createWorld().boilerplate().pageTitle('#info').start();


### Skymap



	// init skymap
	var urls	= tQuery.TextureCube.createUrls('skybox', '.jpg', 'vendor/tquery/assets/images/cube');
	var textureCube	= tQuery.createCubeTexture(urls);
	var skymap	= tQuery.createSkymap({
		textureCube	: textureCube
	}).addTo(world);

### Saber

	var saber	= tQuery.createLightSaber().addTo(world);
	saber.object3D().translateY(-0.35).rotateY(Math.PI/2)

### Minecraft character

	var character	= new tQuery.MinecraftChar({
		skinUrl	: 'vendor/tquery/plugins/minecraft/examples/images/3djesus.png'
	});
	character.model.addTo(saber.object3D())
		.rotateY(Math.PI/2)
		.translateY(0.6);


Now we set the character position. It is sit, got legs apart and suggest he is riding with his arms.
The code below setup this position. You need to access each subpart of your model and setup its proper 
position. To get *left leg* subpart, use ```character.parts.legL```. For light legs, use ```.legR```, for right arms ```.armR```. you got it :)

	character.parts.legL.rotation.x	= -Math.PI/2 + 10*Math.PI/180;
	character.parts.legL.rotation.z = +30*Math.PI/180;
	character.parts.legR.rotation.x	= -Math.PI/2 + 10*Math.PI/180;
	character.parts.legR.rotation.z = -30*Math.PI/180;

	character.parts.armR.rotation.x = 45*Math.PI/180;
	character.parts.armR.rotation.z = -30*Math.PI/180;
	character.parts.armL.rotation.x = -60*Math.PI/180;
	character.parts.armL.rotation.z = -15*Math.PI/180;

### change the material of the hilt

Now lets change the material of the hilt.

We use a reflexion cube texture.

Note that we use the same ```textureCube``` as we used for the skymap.
Thus the texture is shared between the 2 objects and pushed only once
on GPU.
It would be a clear waste of resource to recreating another THREE.TextureCube here with the same images.
The images would be pushed twice in GPU. see ["Performance: Caching Material"](http://learningthreejs.com/blog/2011/09/16/performance-caching-material/) previous posts for more information.

The ```color``` parameter give you the opportunity to
[tint](http://en.wikipedia.org/wiki/Tints_and_shades) 
your material. 
The ```envMap``` parameter is set to ```textureCube``` as said previously.

	// change the material of the hilt
	var material	= tQuery.createMeshBasicMaterial({
		color	: 0x44aaFF,
		envMap	: textureCube
	})
	// set the material on each part of the hilt
	saber.object3D('hiltIn').material(material)
	saber.object3D('hiltOut').material(material) 

### post effect



	// put some post effect
	var composer	= tQuery.createEffectComposer().renderPass();
	composer.bloom(0.1)
	composer.motionBlur(0.95);
	composer.screen();
	composer.vignette(1.2,1);
	composer.finish();

### Sound depending on user move

```
```