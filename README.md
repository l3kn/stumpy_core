# stumpy_core

## Usage

This library is not very useful on its own
but there are a few other libraries
that allow creating a canvas from an image file
or saving one to an image file.

* <https://github.com/stumpycr/stumpy_png>
* <https://github.com/stumpycr/stumpy_gif>

```crystal
include StumpyCore

rainbow = Canvas.new(256, 256)
(0...255).each do |x|
  (0...255).each do |y|
    # RGBA.from_rgb_n(values, bit_depth) is an internal helper method
    # that creates an RGBA object from a rgb triplet with a given bit depth
    color = RGBA.from_rgb_n(x, y, 255, 8)
    rainbow[x, y] = color
  end
end

# It is also possible to provide a background color,
# the default is transparent black `RGBA.new(0, 0, 0, 0)`
white = Canvas.new(256, 256, RGBA.from_hex("#ffffff"))

# If the colors for all pixels are already known when creating the canvas,
# the block syntax can be used to simplify the code:

rainbow2 = Canvas.new(256, 256) { |x, y| RGBA.from_rgb_n(x, y, 255, 8) }

checkerboard = Canvas.new(256, 256) do |x, y|
  if ((x / 32) + (y / 32)).odd?
    RGBA.from_hex("#ffffff")
  else
    RGBA.from_hex("#000000")
  end
end

spectrum  = Canvas.new(361, 101) { |x, y| RGBA.from_hsl(x, 100, y) }
spectrum2 = Canvas.new(361, 101) { |x, y| RGBA.from_hsba([x, 100, y], 1) }
```

![rainbow image](images/rainbow.png)
![checkerboard image](images/checkerboard.png)
![spectrum image](images/spectrum.png)
![spectrum2 image](images/hsv-spectrum.png)


## Interface

* `StumpyCore::RGBA`, 16-bit rgba color
  * `rgba.r` red channel
  * `rgba.g` green channel
  * `rgba.b` blue channel
  * `rgba.a` alpha channel
  * `rgba.to_rgba8` returns a tuple of 4 8-bit values `{ r, g, b, a}`
  * `rgba.to_rgb8` returns a tuple of 3 8-bit values  `{ r, g, b }`
  * Helper functions to create colors from n-bit values:
    * `StumpyCore::RGBA.from_gray_n(grayscale_value, n)`
    * `StumpyCore::RGBA.from_graya_n(grayscale_value, alpha, n)`
    * `StumpyCore::RGBA.from_rgb_n(r, g, b, n)`
    * `StumpyCore::RGBA.from_rgba_n(r, g, b, alpha, n)` where alpha = `0..1`
    * `StumpyCore::RGBA.from_hsl(h, s, l)` where

        h = `0..360°`, s = `0..100%`, l = `0..100%`
    * `StumpyCore::RGBA.from_hsla(h, s, l, alpha)`
    * `StumpyCore::RGBA.from_hsv(h, s, v)` *or*

        `StumpyCore::RGBA.from_hsb(h, s, b)` where

        h = `0..360°`, s = `0..100%`, b *or* v = `0..100%`
    * `StumpyCore::RGBA.from_hsva(h, s, v, alpha)` *or*

        `StumpyCore::RGBA.from_hsba(h, s, b, alpha)` where alpha = `0..1`
    * All of the above (except `from_gray_n`) work with tuples/arrays, too:

        (`StumpyCore::RGBA.from_rgba_n({r, g, b, alpha}, n)`)
    * `StumpyCore::RGBA.from_hex(color)`, e.g. `color = "#ff0000"`

* `StumpyCore::Canvas`, two dimensional Array of RGBA value
  * `canvas.width`
  * `canvas.height`
  * `canvas[x, y] = color` to set a color, `x` and `y` need to be in range!
  * `canvas[x, y]` to get a color, `x` and `y` need to be in range!
  * `canvas.set(x, y, color)` same as `canvas[x, y] = color`
  * `canvas.get(x, y)` same as `canvas[x, y]`
  * `canvas.safe_set(x, y, color)` returns true if setting the color was successful
  * `canvas.safe_get(x, y)` returns `nil` if `x` or `y` are invalid
  * `canvas.wrapping_set(x, y, color)` `set` combined with modulo, to ensure `x` and `y` are in range
  * `canvas.wrapping_get(x, y)` `get` combined with modulo, to ensure `x` and `y` are in range

## Contributors

Thanks goes to these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
| [<img src="https://avatars.githubusercontent.com/u/2060269?v=3" width="100px;"/><br /><sub>Leon</sub>](https://github.com/l3kn)<br />[💻](https://github.com/l3kn/stumpy_core/commits?author=l3kn "Code") | [<img src="https://avatars.githubusercontent.com/u/1298501?v=3" width="100px;"/><br /><sub>Ian Rash</sub>](http://broken-kami.tumblr.com)<br />[💻](https://github.com/l3kn/stumpy_core/commits?author=redcodefinal "Code") | [<img src="https://avatars2.githubusercontent.com/u/26842759?v=3" width="100px;"/><br /><sub>Sam</sub>](https://github.com/Demonstrandum)<br />[💻](https://github.com/l3kn/stumpy_core/commits?author=Demonstrandum "Code") |
| :---: | :---: | :---: |
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind welcome!
