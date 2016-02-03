# Handling resolutions and aspect ratios of common mobile devices

<!--
todo: Jewlines content
-->

Here is short list of resolutions of common mobile devices useful for web,
application and game developers and designers creating content for mobile devices.

## Aspect ratios and resolutions

<table>
	<thead>
	<tr>
		<th>Aspect ratio</th>
		<th>Resolutions</th>
		<th>Example Devices</th>
	</tr>
	</thead>
	<tbody>
	<tr valign="top">
		<td rowspan=" 3">4:3</td>
		<td>320×240</td>
		<td>Android devices</td>
	</tr>
	<tr valign="top">
		<td>1024×768</td>
		<td>iPad 1, iPad 2</td>
	</tr>
	<tr valign="top">
		<td>2048×1536</td>
		<td>iPad 3</td>
	</tr>
	<tr valign="top">
		<td rowspan=" 2">3:2</td>
		<td>480×320</td>
		<td>iPhone 3GS and lower, Android devices</td>
	</tr>
	<tr valign="top">
		<td>960×640</td>
		<td>iPhone 4, iPhone 4S</td>
	</tr>
	<tr valign="top">
		<td rowspan=" 2">16:10</td>
		<td>800×480</td>
		<td>Android devices, WindowsPhone7</td>
	</tr>
	<tr valign="top">
		<td>1280×800</td>
		<td>Android tablets like Google Nexus 7, Samsung Galaxy Tab 10.1, Motorola Xoom, Asus Eee Pad Transformer</td>
	</tr>
	<tr valign="top">
		<td rowspan=" 1">17:10</td>
		<td>1024×600</td>
		<td>Android tablets like Samsung Galaxy Tab 7</td>
	</tr>
	<tr valign="top">
		<td rowspan=" 3">16:9</td>
		<td>640×360</td>
		<td>Symbian3 devices like Nokia C7</td>
	</tr>
	<tr valign="top">
		<td>854×480</td>
		<td>Android devices, MeeGo N9</td>
	</tr>
	<tr valign="top">
		<td>1136×640</td>
		<td>iPhone 5</td>
	</tr>
	</tbody>
</table>

## Using assets with atlases for mobile devices

Texture atlas is a image file that contains sub-image (assets).
It allows to render textures by modifying the texture coordinates of the object’s
uvmap on the atlas, pointing to the part of the image that holds a texture.
Main advantages of using atlases are:

- rendering optimisation (CPU) as one image is already stored in memory,
- lowering number of connections which usually reduces the time of
downloading assets (web development).

In game development (especially tile-based games) texture atlases are very
commonly used to reduce draw calls  which improves overall performance of
the game allowing higher FPS (Frame Per Seconds) rate, providing better
experience for the player.

Depending on target devices, it is **good practice** to prepare couple
or more atlases storing references to the same assets in different sizes
and pointing application or game to selected atlas depending on the
dimensions of user device. For example, going back to game development,
depending on screen size user could store 3 atlases:

- SD – for ipods, low res iphones for small resolution devices up to 960×640 px
- HD – for iphone retina, ipad, android phones and devices with resolution up to 1024×768 px
- UD (ultra def) – for ipad retina and big screen devices, with higher resolution (ex.: 2048×1536 px )

Well known approach that is used by web developers and designers, and listed
as a good practice , is to design interface going from smallest target device
screen size to the biggest screen size. That is especially common with responsive
websites and applications. Now… as it works perfectly for interface design
and UI design it might seem quite intuitive to prepare assets and atlases
in the same way, it would be wrong to do so!

**While targeting devices with different screen definitions,  it is important
to start with assets for the highest definition device going downwards.**
Why? Once the hi-res asset is prepared it is very easy to scale it down,
without worrying about "pixelation".

And so following three definition from above example, we could create biggest
assets and then scale them down adjusting pixel size while replacing atlas
inside the game to keep the right size within the game.
