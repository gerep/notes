[[game-dev]] [[tags/shaders]] 

TEXTURE: will use the current texture (Sprite)
UV: a map

Scrolling background
```c
shader_type canvas_item;

uniform vec2 tiling = vec2(1.0,1.0);
uniform vec2 offset;

void vertex() {
	UV = UV * tiling + (offset * TIME);
}
```

Flash
```c
shader_type canvas_item;

uniform vec4 flash_colour: source_color = vec4(1.0);
uniform float flash_pct : hint_range(0.0, 1.0, 0.1);

void fragment()
{
	vec4 original_colour = texture(TEXTURE, UV);

	COLOR = mix(original_colour, flash_colour, flash_pct);
	COLOR.a *= original_colour.a;
}
```

Gray scale
```c
shader_type canvas_item;

void fragment() {
	vec4 original_colour = texture(TEXTURE, UV);
	float grayscale = (original_colour.r + original_colour.g + original_colour.b)/3.0;

	COLOR = vec4(vec3(grayscale), original_colour.a);
}
```

Map to gradient
```c
shader_type canvas_item;

uniform sampler2D gradient_texture;

void fragment() {
	vec4 original_colour = texture(TEXTURE, UV);
	float grayscale = (original_colour.r + original_colour.g + original_colour.b)/3.0;
	vec4 gradient_colour = texture(gradient_texture, vec2(grayscale));

	COLOR = vec4(gradient_colour.rgb, original_colour.a);
}
```

Screen reading
```c
shader_type canvas_item;

uniform sampler2D gradient_texture;

void fragment() {
	vec4 original_colour = texture(TEXTURE, UV);
	float grayscale = (original_colour.r + original_colour.g + original_colour.b)/3.0;
	vec4 gradient_colour = texture(gradient_texture, vec2(grayscale));

	COLOR = vec4(gradient_colour.rgb, original_colour.a);
}
```

Dissolve
```c
shader_type canvas_item;

uniform sampler2D noise_text;
uniform float dissolve_pct: hint_range(0.0, 1.0, 0.1) = 0.0f;

void fragment() {
	vec4 original_colour = texture(TEXTURE, UV);
	vec4 final_colour = original_colour;
	vec4 noise = texture(noise_text, UV);

	if(dissolve_pct > noise.r) {
		final_colour.a = 0.0;
	}

	COLOR = final_colour;
}
```

Masking
```c
shader_type canvas_item;

uniform sampler2D mask_texture;

void fragment() {
	vec4 mask_colour = texture(mask_texture, UV);
	vec4 original_colour = texture(TEXTURE, UV);
	//COLOR = mask_colour;
	if(original_colour.a > 0.0) {
		COLOR.a = mask_colour.r;
	}
}
```

Distortion
```c
shader_type canvas_item;

uniform sampler2D screen_texture: hint_screen_texture, filter_nearest;

uniform sampler2D noise_text: repeat_enable;
uniform sampler2D noise_text2: repeat_enable;

uniform vec2 offset1 = vec2(0.1);
uniform vec2 offset2 = vec2(0.2);

uniform float distotion_strength: hint_range(-1.0, 1.0) = 0.1;

void fragment() {
	vec4 noise_colour1 = texture(noise_text, UV + (TIME * offset1));
	vec4 noise_colour2 = texture(noise_text2, UV + (TIME * offset2));

	float final_noise = (noise_colour1.r * noise_colour2.r) * distotion_strength;
	vec4 final_colour = texture(screen_texture, SCREEN_UV + final_noise);

	COLOR = final_colour;
}
```
