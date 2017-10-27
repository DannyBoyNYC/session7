# Foundations Session 7 

## Homework

1. Install [GIT](https://git-scm.com) on your laptop
2. Carefully follow the tutorial on [Gituhub](https://try.github.io/levels/1/challenges/1)
3. Create an account on [Github](https://github.com)

1. Review the process used to set up this project and continue to add JavaScript for the video player.

![image](wide.png)
![image](mobile.png)




## Tooling

```sh
$ cd <session7>
$ npm init -y
$ npm install browser-sync node-sass concurrently --save-dev
```

Package.json scripts (similar to last week's):

```sh
  "scripts": {
    "sassy": "node-sass --watch sass --output app/css --source-map true",
    "start": "browser-sync start --server 'app' --files 'app'",
    "boom!": "concurrently \"npm run start\" \"npm run sassy\" "
  },
```

`$ npm run boom!`


## Header

In a style block in the html document:

```css
header {
	max-width: 940px;
}
header h1 {
	font-size: 3rem;
	margin: 0;
}
header p {
	font-size: 1.5rem;
	text-transform: uppercase;
	line-height: 1.1;
	margin-bottom: 1rem;
}
header h1 + p {
	padding-top: 1rem;
	border-top: 3px double #dbd1b5;
}
header p + p {
	font-size: 1rem;
	line-height: 1.1;
	color: #666;
}
```

## Aside - Using an App for SASS Preprocessing

[Syntactically Awesome Style Sheets](https://sass-lang.com) - takes sass files and converts (compiles) them into css. sass [adds features](http://sass-lang.com/guide) to css.

(Note - on OSX you may need to right click and choose open rather than double click in order to run these.)

[Koala](http://koala-app.coms)
[Scout app](https://github.com/scout-app/scout-app/)

For Scout the setup includes creating and input folder for sass and an output folder for css.


### Set up and nesting

Save reset.css to `/sass/imports/_reset.scss`
Save base.css to `/sass/imports/_base.scss`
Remove the header css from the html and add it to a new file and save it into the iports folder `/sass/imports/_base.scss`

Note the underscores in use here as well as the `.scss` extensions.

Note - you will need to remove or comment out the @import statement at the top of this document. Always check for errors by looking at the processes running in the terminal.

Create a new file, styles.scss, in `/sass` and import both the above.

The final `styles.scss` file should look like:

```css
@import 'imports/reset';
@import 'imports/base';
@import 'imports/header';
```

Since we are using SASS includes we can delete the base.css and reset.css files from the css directory.

Refactor the header in `_header.scss` file.

```css
header {
	max-width: 940px;
	h1 {
		font-size: 3rem;
		margin: 0;
	}
	p {
		font-size: 1.5rem;
		text-transform: uppercase;
		line-height: 1.1;
		margin-bottom: 1rem;
	}
	h1 + p {
		padding-top: 1rem;
		border-top: 3px double #dbd1b5;
	}
	p + p {
		font-size: 1rem;
		line-height: 1.1;
		color: #666;
	}
}
```

Inspect the header in the developer tools and note the *mapping*.

- maps the css line numbers to the scss line numbers
- note the line numbers in the elements inspector

### Media Query - Mobile First

Add a media query to hide the paragraphs on small screens.

_header.scss:

```css
	p {
		...
		@media (max-width: 480px){
			display: none;
		}
	}
```

Note: this is *not* a mobile first design pattern. It uses `max-width` to add display attributes to small screens.

Change it to use a `min-width` mobile first design pattern:

```css
p {
	display: none;
	@media (min-width: 480px){
		display: block;
		font-size: 1.5rem;
		text-transform: uppercase;
		line-height: 1.1;
		margin-bottom: 1rem;
	}
}
```

### Variables

Create and add _variables.scss to imports with:

```
//variables

$break-sm: 480px;
$break-med: 768px;

$max-width: 940px;

$link: #4e7c92;
$hover: #df3030;
$text: #333;

$med-gray: #666;
$light-gray: #ddd;
$dk-yellow: #dbd1b5;
```

Import it into `styles.scss`. Be sure to import it first in order to make the variables available to the subsequent imports.

Apply the color and break point variables to `_header.scss`. There are four such instances:

```css
header {
	max-width: $max-width;
	h1 {
		font-size: 3rem;
		margin: 0;
	}
	p {
		display: none;
		@media (min-width: $break-sm){
			display: block;
			font-size: 1.5rem;
			text-transform: uppercase;
			line-height: 1.1;
			margin-bottom: 1rem;
		}
	}
	h1 + p {
		padding-top: 1rem;
		border-top: 3px double $dk-yellow;
	}
	p + p {
		font-size: 1rem;
		line-height: 1.1;
		color: $med-gray;
	}
}
```


## Responsive Main Nav

Add a link `<a href="#" id="pull"></a>` to show the menu on small screens:

```css
<nav>
	<a href="#" id="pull"></a>
	<ul>
		<li><a href="#one">Intro</a></li>
		<li><a href="#two">Summary</a></li>
		<li><a href="#three">Skills</a></li>
		<li><a href="#four">Experience</a></li>
		<li><a href="#five">Education</a></li>
		<li><a href="#six">Contact</a></li>
	</ul>
</nav>
```

Create a sass partial `_navigation.scss` and import it into `styles.css` with `@import 'imports/navigation';`.

Small screen - hide the navigation

```css
nav {
	ul {
		display: none;
	}
}
```

Show and format the hamburger menu:

```css
nav {
	ul {
		display: none;
	}
	#pull {
		display: block;
		background-color: $link;
		height: 32px;
		padding-top: 12px;
		padding-left: 12px;
	}

	#pull:after {
		content:"";
		background: url(../img/nav-icon.png) no-repeat;
		width: 22px;
		height: 22px;
		background-size: cover; 
		display: inline-block;
	}
}
```


### Large Screen

Add media queries for medium and larger screens

Hide the hamburger on wider screens:

```css
a#pull {
	🔥
	@media (min-width: $break-sm) {
		display: none;
	}
}
```

Show the navigation:

```css
ul {
	display: none;
	@media (min-width: $break-sm){
		display: flex;
		justify-content: space-between;
		background: $link;
		text-align: center;
	}
}
```

Can't see the anchor tags:

```css
nav {
	🔥
	a {
		@media (min-width: $break-sm){
			color: #fff;
		}
	}
	🔥
}
```

Note - `space-around` is probably a better choice for the ul formatting here.

Format the list items (horizontal display)

```css
nav {
	🔥
	li {
		@media (min-width: $break-sm){
			padding: 0.5rem;
		}
	}
	🔥
}
```

Add a hover effect to the `<li>` tags using SASS ampersand notation:

```css
nav {
	🔥
	li {
		@media (min-width: $break-sm){
			padding: 0.5rem;
			flex-grow: 1;
			&:hover {
				background: $text;
			}
		}
	}
	🔥
}
```

Note the use of flex-grow to allow the li's to expand.

Here is the entire file for _navigation.scss:

```css
// _navigation.scss

nav {
	ul {
		display: none;
		@media (min-width: $break-sm){
			display: flex;
			justify-content: space-around;
			align-items: center;
			background: $link;
			text-align: center;
		}
	}
	li {
		@media (min-width: $break-sm){
			padding: 0.5rem;
			flex-grow: 1;
			&:hover {
				background: $text;
			}
		}
	}
	a {
		@media (min-width: $break-sm){
			color: #fff;
		}
	}
	#pull {
		display: block;
		background-color: $link;
		height: 32px;
		padding-top: 12px;
		padding-left: 12px;
		@media (min-width: $break-sm) {
			display: none;
		}
	}

	#pull:after {
		content:"";
		background: url(../img/nav-icon.png) no-repeat;
		width: 22px;
		height: 22px;
		background-size: cover; 
		display: inline-block;
	}
}
```


### Show/Hide Nav

Add our scripts at the bottom of the page before the closing body tag:

```html
<script>
  var hamburger = document.querySelector('#pull')
  var navbar = document.querySelector('nav ul')

  hamburger.addEventListener('click', showMenu)

  function showMenu(){
    navbar.classList.toggle('showme')
    event.preventDefault();
  }
</script>
```

add .showme class to the SASS:

```css
.showme { 
	display: block 
}
```

Add to existing SASS

```css
.showme {
	display: flex;
	flex-direction: column;
	position: absolute;
	width: 100%;
}
```

Decorate the list items:

```css
nav {
	🔥
	li {
		padding: 0.5rem;
		background: $light-gray;
		border-bottom: 1px solid #fff;
		@media (min-width: $break-sm){
			padding: 0.5rem;
			flex-grow: 1;
			&:hover {
				background: $text;
			}
		}
	}
	🔥
}
```

Check out the wide screen version. We will need to add some CSS to correct the styles.

1 add a blue background color for the li's at the small break point: `background: $link;`
2 add display block to the links and set their width to 100%
3 remove the padding where appropriate and add it to the anchor tags

```css
nav {
	ul {
		display: none;
		@media (min-width: $break-sm){
			display: flex;
			justify-content: space-around;
			align-items: center;
			background: $link;
			text-align: center;
		}
	}
	li {
		// padding: 0.5rem;
		background: $light-gray;
		border-bottom: 1px solid #fff;

		@media (min-width: $break-sm){
			background: $link;
			// padding: 0.5rem;
			flex-grow: 1;
			&:hover {
				background: $text;
			}
		}
	}
	a {
		padding: 0.5rem;
		display: block;
		width: 100%;
		@media (min-width: $break-sm){
			color: #fff;
		}
	}
	#pull {
		display: block;
		background-color: $link;
		height: 32px;
		padding-top: 12px;
		padding-left: 12px;
		@media (min-width: $break-sm) {
			display: none;
		}
	}

	#pull:after {
		content:"";
		background: url(../img/nav-icon.png) no-repeat;
		width: 22px;
		height: 22px;
		background-size: cover; 
		display: inline-block;
	}
}

.showme {
	display: flex;
	flex-direction: column;
	position: absolute;
	width: 100%;
}
```

Also, make the menu items extra easy to click on mobile:

```css
.showme li {
	padding: 1rem;
}
```

Check the navigation on both sizes and make adjustments as necessary.


### Animation with CSS

Since we cannot animate display block we use properties we can animate to hide the nav:

```css
nav {
	ul {
		// display: none;
		transform: translateY(-200px);
		max-height: 1px;
		opacity: 0;
		transition: all .3s;
		background: $light-gray;
```

Use the same properties to show it:

```
.showme { 
	transform: translateY(0);
	max-height: 1000px;
	opacity: 1;
	transition: all .3s;
}
```

We also need to get the hamburger out of the way:

```
a#pull {
	position: relative;
	z-index: 20;
	display: block;
```

Note that we have a problem in wider screen view - the nav is not showing.

Reset the ul:

```
		@media (min-width: $break-sm){
			display: flex;
			justify-content: space-between;
			background: $link;
			text-align: center;
			transform: translateY(0);
			max-height: 1000px;
		}
```

## Columns for Content

Content (effects multiple regions - re-examine the DOM).

In a new _structure.scss file:

```css
.content {
	max-width: 940px;
	margin: 0 auto;
	padding-bottom: 1.5em;
}
.content-main {
 	box-sizing: border-box;
	float: left;
	width: 60%;
	padding-right: 24px;
}
.content-sub {
	float: right;
	width: 40%;
}
```

Apply the second breakpoint variable to medium screen sizes and above only:

```css
@media (min-width: $break-two) {
	.content {
		max-width: 940px;
		margin: 0 auto;
		padding-bottom: 1.5em;
	}
	.content-main {
		box-sizing: border-box;
		float: left;
		width: 60%;
		padding-right: 24px;
	}
	.content-sub {
		float: right;
		width: 40%;
	}
}
```

### Box sizing 

The universal approach (applies to all elements):

```
*, *:before, *:after {
  box-sizing: inherit;
}
html {
  box-sizing: border-box;
}
```

Check and make corrections to the nav.

The Secondary div

```css
.secondary {
	background:#f8f7f3;
	border:1px solid #bfbfbf;
	padding:1em;
}
```

### Micro clearfix

```css
.clearfix:before,
.clearfix:after {
    content: " ";
    display: table;
}
.clearfix:after {
    clear: both;
}
```

Add the clearfix to the content and secondary divs

`<div class="content clearfix">`

`<div class="container secondary clearfix">`

### Responsive Images

iFrame and images need to expand and contract to fit. 

Note the inline width and height parameters for the iFrame in the HTML.

```css
img,
iframe {
  width: 100%;
}
```

### Video Switcher - JavaScript

The old school JavaScript

```
$('.content-video a').on('click',function(){
	$('.content-video a').removeClass('active');
	$(this).addClass('active');
	var videoToPlay = $(this).attr('href');
	$('iframe').attr('src',videoToPlay);
	console.log(videoToPlay);
	return false;
 });
```

```
const videoLinks = document.querySelectorAll('.content-video a')
const videoLinksArray = [...videoLinks]
videoLinksArray.forEach( videoLink => videoLink.addEventListener('click', selectVideo ))

function selectVideo(){
	console.log(this)
	event.preventDefault()
}
```

```
function selectVideo(){
	const videoToPlay = this.getAttribute('href')
	console.log(videoToPlay)
	event.preventDefault()
}
```

Add the iFrame

```
const iFrame = document.querySelector('iframe')
const videoLinks = document.querySelectorAll('.content-video a')
const videoLinksArray = [...videoLinks]
videoLinksArray.forEach( videoLink => videoLink.addEventListener('click', selectVideo ))

function selectVideo(){
	const videoToPlay = this.getAttribute('href')
	iFrame.setAttribute('src', videoToPlay)
	console.log(iFrame)
	event.preventDefault()
}
```

Active class

Add css for the active class (for the first video iframe link):

```css
.btn-list {
	padding: 6px;
	.active { 
		background: #87a3af;
		text-shadow: none; 
		color: #fff;
	}
}
```

```
const iFrame = document.querySelector('iframe')
const videoLinks = document.querySelectorAll('.content-video a')
const videoLinksArray = [...videoLinks]
videoLinksArray.forEach( videoLink => videoLink.addEventListener('click', selectVideo ))

function selectVideo(){
	removeActiveClass()
	this.classList.add('active')
	const videoToPlay = this.getAttribute('href')
	iFrame.setAttribute('src', videoToPlay)
	console.log(iFrame)
	event.preventDefault()
}

function removeActiveClass(){
	videoLinksArray.forEach( videoLink => videoLink.classList.remove('active'))
}
```

Format the video buttons

```css
.btn-list {
	padding: 6px;
	li {
		display: inline-block;
		margin-right:1.5rem;
		margin-bottom:1rem;
	}
	.active { 
		background: #87a3af;
		text-shadow: none; 
		color: #fff;
		padding: 0.25rem;
	}
}
```

### The Footer 

Note the need for border-box and the new variable.

```css
.footer {
	clear: both;
	margin-top: 40px;
	background-color: $dk-blue;
	min-height: 320px;
	.siteinfo {
		max-width: $max-width;
		margin: 0 auto;
		color: #fff;
		text-shadow: none;
		p, ul, .vcard {
			box-sizing: border-box;
			width: 33%;
			padding: 20px;
			float: left;
		}
	}
	.siteinfo a {
		color: #fff;
	}
} 
```

### Nav Sub

Integrate the JavaScript for nav-sub into the layout.

```css
.nav-sub {  
	margin-top: 16px;
	padding: 20px;
	background-color: #f8f7f3;
	border: 1px solid #bfbfbf;

	li { 
		margin:6px 0;
	}
	ul {
		display:none;
	}
	li:first-child ul {
		display:block;
	}
	> li > a { 
		font-weight:bold; 
	}
	ul li {
		padding-left:12px;
	}
}
```

Accordion for Nav Sub

```js
$('.nav-sub>li a').on('click tap', function(){
	$('.nav-sub ul').slideUp();
	$(this).next().slideToggle();
	console.log(this);
	return false;
});
```

```
const subnavLinks = document.querySelectorAll('.nav-sub > li a')
const subnavLinksArray = [...subnavLinks]
subnavLinksArray.forEach( subnavLink => subnavLink.addEventListener('click', openAccordion))

function openAccordion(){
	console.log(this)
	event.preventDefault()
}
```

Refine the selector (see [Combinators](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Simple_selectors))

```
const subnavLinks = document.querySelectorAll('.nav-sub > li > a')
console.log(subnavLinks)
const subnavLinksArray = [...subnavLinks]
subnavLinksArray.forEach( subnavLink => subnavLink.addEventListener('click', openAccordion))

function openAccordion(){
	console.log(this)
	event.preventDefault()
}
```

[DOM Traversal](https://www.w3schools.com/jsref/dom_obj_document.asp)

nextElementSibling, nextSibling, previousSibling, childNodes, firstChild ...

```
const subnavLinks = document.querySelectorAll('.nav-sub > li > a')
console.log(subnavLinks)
const subnavLinksArray = [...subnavLinks]
subnavLinksArray.forEach( subnavLink => subnavLink.addEventListener('click', openAccordion))

function openAccordion(){
	this.nextElementSibling.classList.toggle('active')
	event.preventDefault()
}
```

Idea - use `.nav-sub > li` as a selector for subnavLinks

Remove the active class

```
const subnavLinks = document.querySelectorAll('.nav-sub > li > a')
console.log(subnavLinks)
const subnavLinksArray = [...subnavLinks]
subnavLinksArray.forEach( subnavLink => subnavLink.addEventListener('click', openAccordion))

function openAccordion(){
	removeActiveClass()
	this.nextElementSibling.classList.toggle('active')
	event.preventDefault()
}

function removeActiveClass(){
	subnavLinksArray.forEach( subnavLink => subnavLink.nextElementSibling.classList.remove('active'))
}
```

Examine the image & the dom, the panels need the same treatment as .nav-sub

```css
.nav-sub, .content-sub .panel {
...
}
```

Rounded Corners

Add to rounded corners - multiple selectors

```css
.nav-collapse, .nav-sub, .content-sub .panel {
	border-radius: 10px;
}
```

### Small Images

```
.secondary .content-sub {
	li {
		float: left;
		width: 33.333%;
		padding: 10px;
	}
	li img {
		padding: 10px;
		background-color: #fff;
		border: 1px solid #bfbfbf;
		border-bottom-color: #7c7c7a;
		width: 100%;
		height: auto;
	}
}
```

Color & Scale Transition

```css
li img {
...
  	transition: all 0.2s linear;
}
li img:hover {
  transform: scale(1.1);
  box-shadow: 1px 1px 1px rgba(0,0,0,0.4);
}
```

Content Slider - examine image

```css
.content-slider {
	position: relative;
	p {
		padding: 6px;
		background: rgba(255,255,255,0.7);
		position: absolute;
		bottom: 0;
	}
}
```

Image Carousel - JavaScript

change the # links to point to high res images

```js
$('.image-tn a').on('click tap', function(){
    var imgsrc = $(this).attr('href');
    var titleText = $(this).find('img').attr('title');
    $('.content-slider > img').attr('src',imgsrc);
    $('.caption').html(titleText);
    return false;
});
```

```
const carouselLinks = document.querySelectorAll('.image-tn a')
const carouselLinksArray = [...carouselLinks]
const carousel = document.querySelector('.content-slider img')

carouselLinksArray.forEach( carouselLink => carouselLink.addEventListener('click', runCarousel ))

function runCarousel(){
	const imageHref = this.getAttribute('href')
	carousel.setAttribute('src', imageHref)
	event.preventDefault()
}
```

Set the text in the carousel.

Find the appropriate traversal.

```
function runCarousel(){
	const imageHref = this.getAttribute('href')
	const titleText = this.firstChild.title
	console.log(titleText)
	carousel.setAttribute('src', imageHref)
	event.preventDefault()
}
```

```
function runCarousel(){
	const imageHref = this.getAttribute('href')
	const titleText = this.firstChild.title
	carouselPara.innerHTML = titleText
	console.log(carouselPara)
	carousel.setAttribute('src', imageHref)
	event.preventDefault()
}
```

hide the h2 and p in small screen view!

The Headers

```css
h2 {
	margin-bottom: 12px;
	padding-bottom: 6px;
	font-size: 24px;
	letter-spacing: -1px;
}

h3, h4 {
	font-size: 16px;
	line-height: 1.25;
	margin-bottom: 20px;
}
```

- effects the panels

```css
h2 + h3 {
	margin-bottom: 7px;
}
```

Content Main > henry - Latest News

```css
.hentry {
  position: relative;
  float: left;
  width: 50%;
}
```

The little date area

```css
.hentry {
	position: relative;
	float: left;
	width: 50%;
	padding: 1rem;
	.published {
		position: absolute;
		top: 250px;
		left: 1rem;
		display: block;
		width: 50px;
		padding: 5px 10px;
		background-color: $link;
		font-size: 10px;
		text-align: center;
		text-transform: uppercase;
		color: #fff;
	}
	.day {
		font-size: 20px;
	}
	h4 {
		margin: 0 0 10px 60px;
		font-size: 20px;
	}
	p {
		margin-left: 60px;
	}
}
```

attribute selectors

```css
a[rel="alternate"] {
	padding-left: 20px;
	background: url(img/a-rss.png) no-repeat 0 50%;
}
```

with svg

```css
a[rel="alternate"] {
	padding-left: 20px;
	background: url(../img/feed-icon.svg) no-repeat 0 50%;
	-webkit-background-size: contain;
    background-size: contain;
}
```

Additional Tweaks for Mobile (need to test on phone)

the tap event in JS

```js
$('.image-tn a').on('click tap', function(){
    var imgsrc = $(this).attr('href');
    var titleText = $(this).find('img').attr('title');
    $('.content-slider > img').attr('src',imgsrc);
    $('.caption').html(titleText);
    return false;
});
```

the z-index for images and navbar

```css
nav {
	height: 40px;
	width:100%;
	background: $link;
	font-size: 1rem;
	position: fixed;
	z-index: 20;
	top: 0;
```

media queries for transform effects (on hover)

```css
.secondary .content-sub {
	li {
		float: left;
		width: 33.333%;
		padding: 10px;
	} 

	li img {
		padding: 10px;
		background-color: #fff;
		border: 1px solid #bfbfbf;
		border-bottom-color: #7c7c7a;
		width: 100%;
		height: auto;
		@media (min-width: $breakpoint-med){
			transition: all 0.2s linear;
			&:hover {
				-webkit-transform: scale(1.1);
				transform: scale(1.1);
				box-shadow: 1px 1px 4px rgba(0,0,0,0.4);
			}
		}
	}
}
```

Basic print styles

```css
 * { background: transparent !important; color: black !important;  } 
  a, a:visited { color: #444 !important; text-decoration: underline; }
  a[href]:after { content: " (" attr(href) ")"; }
  abbr[title]:after { content: " (" attr(title) ")"; }
  tr, img { page-break-inside: avoid; }
  p, h2, h3 { orphans: 3; widows: 3; }
  h2, h3{ page-break-after: avoid; }
```

add to media query:

```css
@media print { 	
}
```

run a test print





## Notes

### Mixins

```
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
}
$radius: 10px;
```


Add to the nav and ul

```html
<nav class="clearfix">
	<ul class="clearfix">
```
