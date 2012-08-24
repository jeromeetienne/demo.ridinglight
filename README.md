demo.ridinglight
================

demo "Riding a beam of light.

See screencast on [youtube](http://www.youtube.com/watch?v=X25Cn90RwhU)

<iframe width="420" height="315" src="http://www.youtube.com/embed/X25Cn90RwhU" frameborder="0" allowfullscreen></iframe>


## Code WalkThru

```
	// init the world
	var world	= tQuery.createWorld().boilerplate().pageTitle('#info').start();
```


### Skymap

```
	// init skymap
	var urls	= tQuery.TextureCube.createUrls('skybox', '.jpg', 'vendor/tquery/assets/images/cube');
	var textureCube	= tQuery.createCubeTexture(urls);
	var skymap	= tQuery.createSkymap({
		textureCube	: textureCube
	}).addTo(world);
```

### Saber

```
	var saber	= tQuery.createLightSaber().addTo(world);
	saber.object3D().translateY(-0.35).rotateY(Math.PI/2)
```

### Minecraft character

```
	var character	= new tQuery.MinecraftChar({
		skinUrl	: 'vendor/tquery/plugins/minecraft/examples/images/3djesus.png'
	});
	character.model.addTo(saber.object3D())
		.rotateY(Math.PI/2)
		.translateY(0.6);
```

```
	character.parts.legL.rotation.x	= -Math.PI/2 + 10*Math.PI/180;
	character.parts.legL.rotation.z = +30*Math.PI/180;
	character.parts.legR.rotation.x	= -Math.PI/2 + 10*Math.PI/180;
	character.parts.legR.rotation.z = -30*Math.PI/180;

	character.parts.armR.rotation.x = 45*Math.PI/180;
	character.parts.armR.rotation.z = -30*Math.PI/180;
	character.parts.armL.rotation.x = -60*Math.PI/180;
	character.parts.armL.rotation.z = -15*Math.PI/180;
```

### change the material of the hilt

```
	// change the material of the hilt
	var material	= tQuery.createMeshBasicMaterial({
		color	: 0x44aaFF,
		envMap	: textureCube
	})
	saber.object3D('hiltIn').material(material)
	saber.object3D('hiltOut').material(material) 
```

### post effect

```
	// put some post effect
	var composer	= tQuery.createEffectComposer().renderPass();
	composer.bloom(0.1)
	composer.motionBlur(0.95);
	composer.screen();
	composer.vignette(1.2,1);
	composer.finish();
```

### Sound depending on user move

```
```