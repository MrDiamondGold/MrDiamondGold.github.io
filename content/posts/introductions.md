+++
title = "Introduction"
cover = ""
description = "Who I am and what I do."
+++

## About Me



## Project History
### // 2023
Non-photorealistic rendering material with a cell-shading inspiration.

{{< image src="win64-4b9-editor_w9xUYhIh2Y.png" alt="Stylized render" position="center" style="border-radius: 8px;" >}}
{{< code language="gdshader" title="Material Shader Code" id="1" expand="Show" collapse="Hide" isCollapsed="true" >}}
shader_type spatial;
render_mode depth_draw_opaque, diffuse_toon, specular_disabled;

const float INV_PI = 1.0 / PI;

global uniform sampler2D light_ramp: source_color, hint_default_white, repeat_disable, filter_linear;

uniform bool use_max_light = false;

uniform float rim_theshhold = 0.5;
uniform vec3 rim_color: source_color = vec3(0.01, 0.01, 0.01);

uniform float specular_power = 8.0;
uniform vec3 specular_color: source_color = vec3(1.0);

uniform vec2 texture_scale = vec2(1.0);

uniform sampler2D albedo_texture: source_color, hint_default_white;
uniform vec3 albedo_color: source_color = vec3(1.0);

uniform vec3 emission: source_color = vec3(0.0);
uniform float emission_strength = 1.0;

uniform sampler2D normal_texture;

void fragment() {
	METALLIC = 0.0;
	ROUGHNESS = 0.5;
	
	float rim = 1.0 - dot(VIEW, NORMAL);
	float rim_strength = float(rim > rim_theshhold);
	vec3 rim_col = texture(light_ramp, vec2(rim_strength, 0.0)).rgb * rim_color;
	
	vec4 albedo = texture(albedo_texture, UV * texture_scale) * vec4(albedo_color, 1.0);
	vec3 vertex_color = COLOR.rgb;
	
	vec3 color = mix(vertex_color, albedo.rgb, 1.0 - albedo.a);
	
	ALBEDO = color - rim_col;
	
	vec3 normal = texture(normal_texture, UV * texture_scale).xyz;
	if (length(normal) < 0.99) {
		NORMAL_MAP = normal;
	}
	
	EMISSION = emission * emission_strength;
}

void light() {
	vec3 light = normalize(LIGHT);
	vec3 normal = normalize(NORMAL);
	vec3 view = normalize(VIEW);
	
	highp float specular_number = 0.999;
	vec3 reflected = reflect(-light, normal);
	float spec = dot(view, reflected);
	spec = float(spec > pow(specular_number, specular_power)) * ATTENUATION;
	
	float diffuse_strength = ((dot(light, normal) + 1.0) * 0.5) * ATTENUATION;
	vec3 diff = texture(light_ramp, vec2(diffuse_strength, 0.0)).rgb;
	diff *= LIGHT_COLOR * INV_PI;
	
	vec3 specular = spec * specular_color * LIGHT_COLOR * INV_PI;
	
	if (use_max_light) {
		DIFFUSE_LIGHT = max(diff, DIFFUSE_LIGHT);
		SPECULAR_LIGHT = max(specular, SPECULAR_LIGHT);
	} else {
		DIFFUSE_LIGHT += diff;
		SPECULAR_LIGHT += specular;
	}
}
{{< /code >}}<br>

---

Simon Says in C using SFML in 564 loc [here on github](https://github.com/MrDiamondGold/SimonSays).<br>

---

### // 2022
Side on 2D platformer game made in seven days for the kajam 2022 game jam about a man dreaming about making the spiciest hoagie he's ever had, [play online](https://replit.com/@RafeHall/Once-Upon-a-Hoagie?v=1) at repl.it, or look at the code on [github](https://github.com/MrDiamondGold/once_upon_a_hoagie).

{{< image src="5dgyzjzm.bmp" alt="Screenshot of game jam game called Once Upon a Hoagie" position="center" style="border-radius: 8px;" >}}<br>

---

### // 2021
Top down 3D horror game about an autonomous mining drone following directives as aliens roam the complex, [play online](https://mrdg.itch.io/miner-incident) at itch.io or look at the code on [github](https://github.com/MrDiamondGold/miner_incident).

{{< image src="31o5fg5k.bmp" alt="Screenshot of game jam game called Miner Incident" position="center" style="border-radius: 8px;" >}}<br>

---

### // 2015, Late 2019 to Early 2020
A backup version of my first proper godot project and various projects made throughout late 2019 being showcased.
{{< youtube id="_ixf2-52XOs" style="margin: 0px; padding=0px;" >}}<br>