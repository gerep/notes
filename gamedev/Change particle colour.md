
```python
var current_colour0: Color = _grad.get_color(0)

var current_colour1: Color = _grad.get_color(1)

var lerp0: Color = current_colour0.lerp(_base_colour, x)

var lerp1: Color = current_colour1.lerp(Color(_base_colour, 0.0), x)

print(current_colour0, current_colour1, lerp0, lerp1)

_grad.set_color(0, lerp0)

_grad.set_color(1, lerp1)
```