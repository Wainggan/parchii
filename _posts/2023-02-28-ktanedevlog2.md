---
title: "ktane - demake attempt part 2"
---

i did more work on the demake, and I made quite a bit of progress!

i added the various edge widgets of the bomb, and 2 new modules: simple wires and simon-says. they seem to function as accurate to the game as I could get it, but I'm not sure how accurate the modules are when they strike, or rather when they should strike... it's much more difficult to test that than testing if it solves correctly.

i also modified the module api a bit to make it nicer to use, here's a what the button looks like now:

```js
const module = createModule({
	// "width" and "height" get applied to the module
	width: 7, height: 7,

	// everything else is given to a special "state" object
	stripColor: 'black',
	laterStripColor: "",
	buttonColor: "",
	buttonText: "",
	// how long you hold the button until the strip color appears
	pressBuffer: 20,
	// wether the button should be held or should be pressed and released immideately
	toHold: false,

	// runs after the state variables are initialized
	init(a, s) {
		// "s" is the state object shared by the whole module

		const buttonColors = ['red', 'blue', 'white', 'yellow', 'black'];
		s.buttonColor = buttonColors[Math.floor(Math.random() * buttonColors.length)];
		
		const buttonText = ['abort', 'detonate', 'hold', 'press'];
		s.buttonText = buttonText[Math.floor(Math.random() * buttonText.length)];
		
		const stripColors = ['red', 'blue', 'white', 'yellow'];
		s.laterStripColor = stripColors[Math.floor(Math.random() * stripColors.length)];
		
		// if the button should be held or not
		if (s.buttonColor == 'blue' && s.buttonText == 'abort') {
			s.toHold = true;
		} else if (a.getTotalBatteries() > 1 && s.buttonText == 'detonate') {
			s.toHold = false;
		} else if (s.buttonColor == 'white' && a.hasIndicator('CAR')) {
			s.toHold = true;
		} else if (a.getTotalBatteries() > 2 && a.hasIndicator('FRK')) {
			s.toHold = false;
		} else if (s.buttonColor == 'yellow') {
			s.toHold = true;
		} else if (s.buttonColor == 'red' && s.buttonText == 'hold') {
			s.toHold = false;
		} else {
			s.toHold = true;
		}
		
	}
}, [
	createElement((e, a, s) => {
		e.x = 0;
		e.y = 1;
		e.width = 5;
		e.height = 5;
		e.draw = (ctx, x, y, w, h) => {
			ctx.fillStyle = s.buttonColor;
			ctx.beginPath();
			ctx.ellipse(x+w/2, y+h/2, w/2-4, h/2-4, 0, 0, 2*Math.PI);
			ctx.fill();
			
			ctx.fillStyle = 'white'
			if (s.buttonColor == 'yellow' || s.buttonColor == 'white') {
				ctx.fillStyle = 'black'
			}
			ctx.textAlign = 'center';
			ctx.textBaseline = 'middle';
			ctx.fillText(s.buttonText, x+w/2, y+w/2);
		
		}
		e.onHold = () => {
			s.pressBuffer--;
			if (s.pressBuffer <= 0) {
				s.stripColor = s.laterStripColor;
			}
		}
		e.onRelease = () => {
			// rules
			if (s.pressBuffer > 0 && !s.toHold) {
				a.solve();
			} else if (s.toHold) {
				const t = a.getMinute().toString() + a.getSecond().toString();
				if (s.stripColor == 'blue' && t.includes('4')) {
				a.solve();
				} else if (s.stripColor == 'white' && t.includes('1')) {
				a.solve();
				} else if (s.stripColor == 'yellow' && t.includes('5')) {
				a.solve();
				} else if (t.includes('1')) {
				a.solve();
				} else {
				a.strike();
				}
			} else {
				a.strike();
			}
			
			// reset the button
			s.stripColor = 'black';
			s.pressBuffer = 20;
		
		}
	}),
	createElement((e, a, s) => {
		e.name = 'strip'
		
		e.x = 5;
		e.y = 1;
		e.width = 2;
		e.height = 5;
		e.padding = 8;
		e.clickable = false;
		
		e.draw = (ctx, x, y, w, h) => {
			ctx.fillStyle = s.stripColor;
			ctx.fillRect(x, y, w, h);
		}
		
	}),
])
```

much better than before, I think. 

that's all I have for now, have a good day, random stranger :3
