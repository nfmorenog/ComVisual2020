## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/nfmorenog/ComVisual2020/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).
### Processing example
---
title: Fireworks
layout: tutorial
thumbnail: /examples/p5js/images/fireworks-2.png
tagline: Ooh! Aah!
sort-key: 100
meta-title: P5.js Fireworks Example
meta-description: Use P5.js to put on a fieworks show!
meta-image: /examples/p5js/images/fireworks-1.png
tags: [example, p5.js, javascript, es6]
---

<div id="sketch-holder"></div>

Click or tap anywhere to launch a firework! :fireworks:

I spent a couple hours on a plane on July 4 this year, so I filled the time by putting together a little fireworks show.

---

<script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.6.1/p5.min.js"></script>
<script>
let particles = [];
const gravity = .25;
const colors = ['red', 'orange', 'yellow', 'lime', 'cyan', 'magenta', 'white'];
let endColor;
let houses;

function setup() {
	pixelDensity(1);
	const canvas = createCanvas(600, 600);
	canvas.parent('sketch-holder');
	endColor = color(64, 0);
	makeHouses();
}

function makeHouses() {
	houses = createGraphics(width, height);
	houses.strokeWeight(2);
	const houseCount = 10;
	const houseWidth = width / houseCount;
	const houseWindowWidth = 10;
	const houseWindowHeight = 15;
	for (let i = 0; i < houseCount; i++) {
		const houseHeight = random(35, 100);
		houses.fill(128);
		houses.rect(houseWidth * i, height - houseHeight, houseWidth, houseHeight * 2);

		for (let windowY = height - houseHeight + 10; windowY < height - houseWindowHeight - 5; windowY += houseWindowHeight + 5) {
			houses.fill(random() < 0.25 ? 'yellow' : 64);
			houses.rect(houseWidth * i + 12, windowY, houseWindowWidth, houseWindowHeight);

			houses.fill(random() < 0.25 ? 'yellow' : 64);
			houses.rect(houseWidth * (i + 1) - 12 - houseWindowWidth, windowY, houseWindowWidth, houseWindowHeight);

		}
	}
}

function mousePressed() {
	if(mouseX > 0 && mouseX < width && mouseY > 0 && mouseY < height){
		particles.push(new Firework(mouseX, height));
		return false;	
	}
}

function draw() {
	background(64);
	particles.forEach((p) => {
		p.step();
		p.draw();
	});
	particles = particles.filter((p) => p.isAlive);

	image(houses, 0, 0);
}

class Particle {
	constructor(x, y, xSpeed, ySpeed, pColor, size) {
		this.x = x;
		this.y = y;
		this.xSpeed = xSpeed;
		this.ySpeed = ySpeed;
		this.color = pColor;
		this.size = size;
		this.isAlive = true;
		this.trail = [];
		this.trailIndex = 0;
	}

	step() {
		this.trail[this.trailIndex] = createVector(this.x, this.y);
		this.trailIndex++;
		if (this.trailIndex > 10) {
			this.trailIndex = 0;
		}
		this.x += this.xSpeed;
		this.y += this.ySpeed;

		this.ySpeed += gravity;

		if (this.y > height) {
			this.isAlive = false;
		}
	}

	draw() {
		this.drawTrail();
		fill(this.color);
		noStroke();
		rect(this.x, this.y, this.size, this.size);

	}

	drawTrail() {
		let index = 0;

		for (let i = this.trailIndex - 1; i >= 0; i--) {
			const tColor = lerpColor(color(this.color), endColor,
				index / this.trail.length);
			fill(tColor);
			noStroke();
			rect(this.trail[i].x, this.trail[i].y, this.size, this.size);
			index++;
		}

		for (let i = this.trail.length - 1; i >= this.trailIndex; i--) {
			const tColor = lerpColor(color(this.color), endColor,
				index / this.trail.length);
			fill(tColor);
			noStroke();
			rect(this.trail[i].x, this.trail[i].y, this.size, this.size);
			index++;
		}
	}
}

class Firework extends Particle {
	constructor(x, y) {
		super(x, y, random(-2, 2), random(-10, -15),
			random(colors), 10);
		this.countdown = random(30, 60);
	}

	step() {
		super.step();

		this.countdown--;
		if (this.countdown <= 0) {
			const explosionSize = random(20, 50);
			for (let i = 0; i < explosionSize; i++) {

				const speed = random(5, 10);
				const angle = random(TWO_PI);
				const xSpeed = cos(angle) * speed;
				const ySpeed = sin(angle) * speed;

				particles.push(new Particle(this.x, this.y,
					xSpeed, ySpeed,
					this.color, 5
				));
			}
			this.isAlive = false;
		}
	}
}
</script>



```javascript
let particles = [];
const gravity = .25;
const colors = ['red', 'orange', 'yellow', 'lime', 'cyan', 'magenta', 'white'];
let endColor;
let houses;

function setup() {
	pixelDensity(1);
	createCanvas(600, 600);
	endColor = color(64, 0);
	makeHouses();
}

function makeHouses() {
	houses = createGraphics(width, height);
	houses.strokeWeight(2);
	const houseCount = 10;
	const houseWidth = width / houseCount;
	const houseWindowWidth = 10;
	const houseWindowHeight = 15;
	for (let i = 0; i < houseCount; i++) {
		const houseHeight = random(35, 100);
		houses.fill(128);
		houses.rect(houseWidth * i, height - houseHeight, houseWidth, houseHeight * 2);

		for (let windowY = height - houseHeight + 10; windowY < height - houseWindowHeight - 5; windowY += houseWindowHeight + 5) {
			houses.fill(random() < 0.25 ? 'yellow' : 64);
			houses.rect(houseWidth * i + 12, windowY, houseWindowWidth, houseWindowHeight);

			houses.fill(random() < 0.25 ? 'yellow' : 64);
			houses.rect(houseWidth * (i + 1) - 12 - houseWindowWidth, windowY, houseWindowWidth, houseWindowHeight);

		}
	}
}

function mousePressed() {
	particles.push(new Firework(mouseX, height));
}

function draw() {
	background(64);
	particles.forEach((p) => {
		p.step();
		p.draw();
	});
	particles = particles.filter((p) => p.isAlive);

	image(houses, 0, 0);
}

class Particle {
	constructor(x, y, xSpeed, ySpeed, pColor, size) {
		this.x = x;
		this.y = y;
		this.xSpeed = xSpeed;
		this.ySpeed = ySpeed;
		this.color = pColor;
		this.size = size;
		this.isAlive = true;
		this.trail = [];
		this.trailIndex = 0;
	}

	step() {
		this.trail[this.trailIndex] = createVector(this.x, this.y);
		this.trailIndex++;
		if (this.trailIndex > 10) {
			this.trailIndex = 0;
		}
		this.x += this.xSpeed;
		this.y += this.ySpeed;

		this.ySpeed += gravity;

		if (this.y > height) {
			this.isAlive = false;
		}
	}

	draw() {
		this.drawTrail();
		fill(this.color);
		noStroke();
		rect(this.x, this.y, this.size, this.size);

	}

	drawTrail() {
		let index = 0;

		for (let i = this.trailIndex - 1; i >= 0; i--) {
			const tColor = lerpColor(color(this.color), endColor,
				index / this.trail.length);
			fill(tColor);
			noStroke();
			rect(this.trail[i].x, this.trail[i].y, this.size, this.size);
			index++;
		}

		for (let i = this.trail.length - 1; i >= this.trailIndex; i--) {
			const tColor = lerpColor(color(this.color), endColor,
				index / this.trail.length);
			fill(tColor);
			noStroke();
			rect(this.trail[i].x, this.trail[i].y, this.size, this.size);
			index++;
		}
	}
}

class Firework extends Particle {
	constructor(x, y) {
		super(x, y, random(-2, 2), random(-10, -15),
			random(colors), 10);
		this.countdown = random(30, 60);
	}

	step() {
		super.step();

		this.countdown--;
		if (this.countdown <= 0) {
			const explosionSize = random(20, 50);
			for (let i = 0; i < explosionSize; i++) {

				const speed = random(5, 10);
				const angle = random(TWO_PI);
				const xSpeed = cos(angle) * speed;
				const ySpeed = sin(angle) * speed;

				particles.push(new Particle(this.x, this.y,
					xSpeed, ySpeed,
					this.color, 5
				));
			}
			this.isAlive = false;
		}
	}
}
```

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/nfmorenog/ComVisual2020/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
