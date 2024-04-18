---
title: "keep talking and nobody explodes - demake attempt"
---

decided to try remaking "[keep talking and nobody explodes](https://keeptalkinggame.com)", a very good game you should get, as a much simpler 2d web game, just for the funsies amd because I very much like the game.

i have quite a few ambitions for my demake, namely I want to create a very simple api for people to create their own modules with javascript, built right into game. 

as of today, here's my progress!

## progress!!

{% include picture.html src="/asset/image/post/2023-02-28/screenshot1" alt="a single button module. the ui shows a timer with 4 minutes and 51 seconds left" %}

here's a single primitive looking button module, which I'm using to test and demonstrate the interactivity.

you can hold down on the button, and half a second later, the strip shows a (currently hardcoded) color.

{% include picture.html src="/asset/image/post/2023-02-28/screenshot2" alt="the button module's strip is yellow colored now" %}

letting go of the button automatically solves it.

{% include picture.html src="/asset/image/post/2023-02-28/screenshot3" alt="the module is outlined green now" %}

very simple stuff. unfortunately I can't implement the actual button rules until I get batteries and indicators working. <br/>
later on I'll attempt to implement every vanilla module.

### api

here's the first iteration of the api:

```js
// createModule(arrayOfElements, [width = 4], [height = 4])
// creates a module object. accepts an array of elements to append as child objects

// createElement(func)
// creates an element object. accepts a function that takes the new element and an object representing an api that interacts with the game state

const module = createModule([
	createElement((e, a) => {
		e.x = 0;
		e.y = 0;
		e.width = 4;
		e.height = 4;

		e.onRelease = () => {
			console.log("e");
			a.solve();
		}
	})
]);
modules.push(module);
```

that creates a module that has a single "button" that takes up the entire module. clicking the button will print "e" to the console and solve the module.

here's the code that creates that button module:

```js
const module = createModule([
	createElement((e, a) => {
		e.x = 0;
		e.y = 1;
		e.width = 5;
		e.height = 5;
		e.draw = (ctx, x, y, w, h) => {
			ctx.fillStyle = 'red';
			ctx.beginPath();
			ctx.ellipse(x+w/2, y+h/2, w/2-4, h/2-4, 0, 0, 2*Math.PI);
			ctx.fill();
			
			ctx.fillStyle = 'white'
			ctx.textAlign = 'center';
			ctx.textBaseline = 'middle';
			ctx.fillText('Press', x+w/2, y+w/2);
		}
		// underscores before property names mean they're element scoped state. I'll need to make module scoped state
		e._pressBuffer = 20;
		e.onHold = () => {
			e._pressBuffer--;
			if (e._pressBuffer <= 0) {
				a.getElement('strip')._color = 'yellow';
			}
		}
		e.onRelease = () => {
			a.getElement('strip')._color = 'black';
			a.solve();
		}
	}),
	createElement((e, a) => {
		e.name = 'strip'
		
		e.x = 5;
		e.y = 1;
		e.width = 2;
		e.height = 5;
		e.padding = 8;
		e.clickable = false;
		
		e._color = 'black'
		e.draw = (ctx, x, y, w, h) => {
			ctx.fillStyle = e._color;
			ctx.fillRect(x, y, w, h);
		}
	}),
	], 7, 7));
modules.push(module);
```

... yikes. a bit to messy if you ask me. i'll continue to iterate on it.

thats all I have for now. have a nice day :3
