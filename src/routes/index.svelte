<script>

	let Complex = class Complex {
		constructor(x, y) {
			this.re = x;
			this.im = y;
		};
		
		
		abs() {
			return Math.hypot(this.re, this.im);
		};

		angle() {
			let a = this.abs();
			if (a == 0) {
				return 0;
			}
			if (this.im > 0) {
				return Math.acos(this.re / a);
			} else {
				return -Math.acos(this.re / a);
			}
		};
		
		add(z) {
			return new Complex(this.re + z.re, this.im + z.im);
		};

		pow(n) {
			let a = this.angle() * n;
			let r = Math.pow(this.abs(), n);
			return new Complex(Math.cos(a)*r, Math.sin(a)*r);
		};

	};

	let n = 2;
	
	function IsStable(c) {
		let z = new Complex(0.0, 0.0);
		for (let i = 0; i < 30; i++) {
			z = (z.pow(n)).add(c);
			if (z.abs() > 4) {
				return false;
			}
		}
		return true;
	}
	let calcTimeoutId;

	function handleValueChange() {
		if (calcTimeoutId) clearTimeout(calcTimeoutId);
		calcTimeoutId = setTimeout(redraw, 300)
	};
	
	let canvas;

	function redraw() {
		let h = canvas.height;
		let w = canvas.width;
		var ctx = canvas.getContext('2d');
		var img = ctx.getImageData(0, 0, h, w);

		for (let y = 0; y < h; y++) {
			for (let x = 0; x < w; x++) {
				if (IsStable(new Complex(4*(x - (w/2))/w, 4*(y - (h/2))/h))) {
					ctx.fillStyle = "#FFCC66";
					ctx.fillRect(x, y, 1, 1);
				} else {
					ctx.fillStyle = "#333333";
					ctx.fillRect(x, y, 1, 1);
				}
			}
		};
		ctx.drawImage(img);
	};
</script>

<h1>Mandelbrot++</h1>
<br>
<p1>The value of  'n' in "z = z^n + c" is: {n}</p1>
<br>

<input on:input={handleValueChange} bind:value={n} type="range" min=0 max=20 step=0.1 style="width: 30%">

<canvas bind:this={canvas} id="Mandel" width="400" height="200" style="border: 1px; background-color:#333">
</canvas>

<style>
canvas {
	width: 100%;
	height: 100%;
}
</style>
