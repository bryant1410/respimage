#respimage
**respimage** is a fast, lightweight and robust [responsive images](http://picture.responsiveimages.org/) polyfill, that saves the users bandwidth by [utilizing smart resource selection algorithm](how-respimg-works.md). It implements the ``srcset``/``sizes`` attributes as also the ``picture`` element. Unlike other responsive images polyfills ``respimage`` plays nicely with your graceful degradation / progressive enhancement strategy.

##Download and Embed
Simply [download the respimage.min.js](respimage.min.js) script and add it to your website or bundle it in your normal JS.

```html
<script src="respimage.min.js" async=""></script>
```
**respimage** will automatically run and polyfill all images. So you can simply start writing responsive images.

In case you want to include **respimage** only if the browser doesn't support responsive images you can use a script loader or write the following at the top of your head (before any other stylesheets or blocking JS):

```html
<script>
if(!window.HTMLPictureElement){
	//load respimage polyfill
	document.write('<script src="respimage.min.js" async=""><\/script>');
}
</script>
```

In case you need to support IE8 and want to include the script at the bottom you need to use either the [html5shiv](https://github.com/aFarkas/html5shiv) or add at least the following script inside your ``head`` element:

```html
document.createElement('picture');
```

Also note, that only IE8 in strict mode is supported. In case you need to support IE8 compatibility view or IE7, please use the [oldie plugin](plugins/oldie).

###Mobile support

For mobile support it is crucial to set the view ``meta`` tag to ``device-width``

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

##Markup Examples
Responsive images can be technically differentiated between 2 types.

* ``srcset`` with source descriptors (let the browser choose the right image based on screen size/resolution, bandwidth…):
	* density descriptor (``x``) (for static image sizes, Retina vs. normal resolution)
	* width descriptor (``w``) and the corresponding ``sizes`` attribute (for flexible, responsive / adaptive images)
* and the ``picture`` element with its ``source[media]`` children (gives the author control about what ``srcset`` should be chosen by the browser depending on specific media queries)


###``srcset`` with the density ``x`` descriptor
The ``x`` descriptor is natively supported in [Chrome 34+ and Safari 7.1+](http://caniuse.com/#feat=srcset). All other browsers will be polyfilled. <br />Note: You must not mix the ``w`` and the ``x`` descriptor in one ``srcset`` attribute!

```html
<img
	srcset="http://placehold.it/700x300 2x"
	src="http://placehold.it/350x150"
    alt="Static content image" />
```
[load example](http://codepen.io/aFarkas/pen/qEBOEq)


###``srcset`` with the width ``w`` descriptor and the ``sizes`` attribute
The ``w`` descriptor is currently only supported in Chrome. All other browsers will be polyfilled. <br />Note: You must not mix the ``w`` and the ``x`` descriptor in one ``srcset`` attribute!

```html
<img
	srcset="http://placehold.it/466x200 466w,
		http://placehold.it/700x300 700w,
		http://placehold.it/1050x450 1050w,
		http://placehold.it/1400x600 1400w"
	sizes="(max-width: 1000px)  calc(100vw - 20px), 1000px"
	src="http://placehold.it/466x200"
	alt="flexible image" />
```
[load example](http://codepen.io/aFarkas/pen/KwKdpY)

###The ``picture`` element
The ``picture`` element is currently only supported in [Chrome 38+](http://caniuse.com/#search=picture). All other browsers will be polyfilled. To support IE9 all source elements have to be wrapped inside of an ``audio`` or hidden ``video`` element:

```html
<picture>
	<!--[if IE 9]><audio><![endif]-->
	<source
			srcset="http://placehold.it/500x600/11e87f/fff"
			media="(max-width: 450px)" />
	<source
			srcset="http://placehold.it/700x300"
			media="(max-width: 720px)" />
	<source
			srcset="http://placehold.it/1400x600/e8117f/fff"
			media="(max-width: 1100px)" />
	<!--[if IE 9]></audio><![endif]-->
	<img
			src="http://placehold.it/1800x900/117fe8/fff"
			alt="image with artdirection" />
</picture>
```
[load example](http://codepen.io/aFarkas/pen/yyLJWO)

The art direction approach of the picture element and the descriptor approach can also be mixed:

```html
<picture>
	<!--[if IE 9]><video style="display: none;"><![endif]-->
	<source
			srcset="http://placehold.it/525x225 1.5x,
        	http://placehold.it/350x150 1x"
			media="(max-width: 380px)" />
	<source
			srcset="http://placehold.it/1400x600/e8117f/fff 1.5x,
        	http://placehold.it/1024x439/e8117f/fff 1x"
			media="(max-width: 1050px)" />
	<!--[if IE 9]></video><![endif]-->
	<img
			src="http://placehold.it/2100x900/117fe8/fff"
			alt="image with artdirection" />
</picture>
```
[load example](http://codepen.io/aFarkas/pen/RNwRzq)

##API
###``respimage`` function
In case new responsive images are created and dynamically added to the DOM simply invoke the ``respimage`` method.

```js
window.respimage();
```

Here an extended example how this could look like in a jQuery environment.

```js
$("div.dynamic-context").load("path-to-content.html", function(){
	if( window.respimage ) {
    	respimage();
    }
});
```

In case you are not supporting IE8 we recommend to use the [Mutation plugin](plugins/mutation) instead of doing this.

## Browser Support
**respimage** supports a broad range of browsers and devices. It is actively tested in the following browsers and devices IE8+, Firefox (ESR and current), Safari 7.0+, Chrome, Opera, Android 4.1+ and IOS 7+, but should work in a lot more browsers/devices. IE6 and IE7 are *not* supported.

###Troubleshooting and bug reporting
In case of any problems include the **respimage.dev.js** into your project and open your JS console. In case you think you have found a bug, please create a testcase and then report your issue. Note: You should not use the dev build inside your production environment, because it is a lot slower.

**Note: It is highly recommended to test with the *.dev.js file, especially if you are using responsive images the first time or you start a new project setup.** The **respimage.dev.js** file can give you some useful hints in the console. About 80% of all tutorials suggest wrong markup examples! Also note: That our respimg debugger can't check every possible error.

##The [intrinsic sizes / dimensions - Plugin](plugins/intrinsic-dimension)
The intrinsic dimension plugin extends ``respimage`` to add the intrinsic dimension based on the descriptor (and the sizes attribute) and the density of the source candidate to the width content attribute of the image element.

##The [Mutation - Plugin](plugins/mutation)
This plugin automatically detects new responsive images and also changes to ``srcset``/``media`` and ``sizes`` attributes.

##The [typesupport - Plugin](plugins/typesupport)
The type support plugin adds type support detection for the following image file types: apng, JPEG 2000, JPEG XR, WEBP

##The [oldie - Plugin](plugins/oldie)
Respimage supports IE8+ (including) out of the box. In case you need to support IE6/7 or any IE in compatibility view or quirksmode use the oldie plugin.

##Known issues/caveats
* Browsers without picture and srcset support and disabled JS will either show the image specified with the ``src`` attribute or - if omitted - show only the ``alt`` text
* **respimage** is quite good at detecting not to download a source candidate, because an image with a good resolution was already downloaded. If a fallback src with a lower resolution or another art direction set is used, **respimage** however will start to download the better candidate, after the browser might have already started to download the worse fallback candidate. Possible solutions/workarounds:
    * omit the ``src`` attribute,
    * use a [lazyLoading](https://github.com/aFarkas/lazysizes) script (what you should do, if you are a performance aware developer anyway) or
    * simply live with it. (recommended, because **respimage** does not simply switch the image src, but implements the [low quality image placeholder (LQIP)](how-respimg-works.md) technique
* Media queries support in old IEs (IE8/IE9) are limited to ``min-width`` and ``max-width``. For IE9 it is possible to extend support by including a [``matchMedia`` polyfill](https://github.com/paulirish/matchMedia.js).

##Responsive images and lazy loading
Beside the fact, that lazy loading improves performance, there is an interesting side effect. Due to delayed image loading the sizes attribute can be dynamically calculated with JS and makes integrating responsive images in any environment therefore easy. We recommend [lazysizes](https://github.com/aFarkas/lazysizes).

##Building a production ready respimage.js version from the *.dev.js file

The respimage.js or the respimage.min.js files are production ready versions of respimage while the respimage.dev.js file includes some informativ extra checks (For example, it checks wether your markup or the content of your ``sizes`` is reasonable.). Therefore the dev version is not only bigger but also a lot slower. In case you want to use the dev version inside your dev enviroment and want to automatically build a production ready version, you can do so by using the dead code removal feature of uglify. Here is a simple grunt config example:

```js
/*
// simply add the following option to your uglify option task
// to remove respimage's debug code:
compress: {
    global_defs: {
        "RIDEBUG": false
    },
    dead_code: true
}
*/
grunt.initConfig({
    //uglify task
    uglify: {
        options: {
            compress: {
                global_defs: {
                    "RIDEBUG": false
                },
                dead_code: true
            }
        },
        //your task:
        my_target: {
            files: [{
                expand: true,
                cwd: 'src/js',
                src: '**/*.js',
                dest: 'dest/js'
            }]
        }
    }
});
```

##Authors
* Authors of the original work: Scott Jehl, Mat Marquis, Shawn Jansepar (2.0 refactor lead)
* Authors of the improved **respimage** script: Alexander Farkas
* and many more: see [Authors.txt](Authors.txt)

##Contributing
Fixes, PRs and issues are always welcome, make sure to create a new branch from the **dev** (not the stable branch), validate against JShint and test in all browsers. In case of an API/documentation change make sure to also document it here in the readme.md.
