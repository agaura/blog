---
title: "Switcher"
date: 2022-06-09T16:21:02+02:00
draft: true
categories: [Projects]
tags: [shader, dither]
katex: true
---

# Shader

<html>
 <!-- D3.js -->
    <script src="https://d3js.org/d3.v6.min.js"></script>
    <script src="https://unpkg.com/d3-simple-slider"></script>    <!-- topojson -->
    <script src="https://unpkg.com/topojson@3"></script>    <!-- WebGL -->
    <script src="https://webgl2fundamentals.org/webgl/resources/webgl-utils.js"></script>
	<b style="font-size:30px" id='btn' onmouseover="this.style.cursor='pointer';" width = 20px>▶️</b>
    <style>
        input[type="file"] {
            display: none;
        }        .custom-file-upload {
            border: 1px solid #ccc;
            display: inline-block;
            padding: 6px 12px;
            cursor: pointer;
        }
    </style>    <div align="center">
        <label class="custom-file-upload">
            <input id="uploadImage" type="file" name="myPhoto" onchange="PreviewImage();" />
            <i class="fa fa-cloud-upload"></i>  Upload
        </label>
    </div>    <div align="center" style="padding-top: 20px">
        <canvas id="canvas"></canvas>
        <!--
for most samples webgl-utils only provides shader compiling/linking and
canvas resizing because why clutter the examples with code that's the same in every sample.
See https://webgl2fundamentals.org/webgl/lessons/webgl-boilerplate.html
and https://webgl2fundamentals.org/webgl/lessons/webgl-resizing-the-canvas.html
for webgl-utils, m3, m4, and webgl-lessons-ui.
-->
    </div>
      <div align="center" id="slider-fill">
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
        style="position:relative; top:-23px;">
        <linearGradient id="Gradient2" x1="0" x2="0" y1="0" y2="1">
            <stop offset="0%" stop-color="#d60071" />
            <stop offset="20%" stop-color="#c04e00" />
            <stop offset="30%" stop-color="#917200" />
            <stop id='example' offset="45%" stop-color="#468a00" />
            <stop offset="50%" stop-color="#008a79" />
            <stop offset="60%" stop-color="#0083a8" />
            <stop offset="70%" stop-color="#4b5dff" />
            <stop offset="100%" stop-color="#b200df" />
        </linearGradient>
        <path fill="url(#Gradient2)"
            d="M12 0c-4.87 7.197-8 11.699-8 16.075 0 4.378 3.579 7.925 8 7.925s8-3.547 8-7.925c0-4.376-3.13-8.878-8-16.075zm.462 20.471c2.56-1.049 4.124-4.889 3.021-8.853 3.798 4.909.754 9.393-3.021 8.853z" /></svg>
</div>
<div align="center" id="slider-fill2"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" style = "position:relative; top:-23px;"><path fill='gray' d="M22.266 14.724c-.064.898-.242 1.766-.522 2.586l1.601.675c-.163.461-.353.909-.568 1.344l-1.602-.676c-.383.751-.855 1.448-1.402 2.078l1.229 1.229c-.323.364-.667.709-1.032 1.032l-2.262-2.262c1.874-1.59 3.125-4.039 3.125-6.735 0-4.878-3.954-8.832-8.832-8.832s-8.833 3.954-8.833 8.832c0 2.505 1.107 5.024 3.126 6.735l-2.293 2.27-1.031-1.031 1.259-1.238c-.562-.647-1.046-1.365-1.434-2.139l-1.606.665c-.212-.436-.399-.885-.559-1.348l1.605-.664c-.268-.802-.439-1.646-.5-2.521h-1.735v-1.459h1.734c.063-.897.242-1.765.522-2.585l-1.6-.675c.163-.461.352-.91.567-1.344l1.602.675c.383-.75.855-1.448 1.403-2.078l-1.229-1.229c.323-.365.668-.709 1.032-1.031l1.229 1.229c.647-.562 1.366-1.045 2.141-1.433l-.665-1.606c.435-.212.885-.399 1.347-.558l.665 1.604c.802-.267 1.647-.438 2.522-.5v-1.734h1.459v1.733c.899.063 1.767.242 2.587.522l.675-1.6c.462.162.911.353 1.345.567l-.676 1.602c.751.383 1.449.854 2.08 1.402l1.229-1.229c.365.322.709.666 1.032 1.03l-1.229 1.229c.562.647 1.045 1.366 1.434 2.14l1.606-.665c.212.436.399.885.558 1.348l-1.604.664c.269.802.439 1.646.501 2.521h1.733v1.459h-1.734zm-3.434-.729c0 3.767-3.064 6.832-6.832 6.832s-6.833-3.065-6.833-6.832 3.065-6.832 6.833-6.832 6.832 3.065 6.832 6.832zm-7.36-1.81l-2.158-2.158-1.415 1.413 2.158 2.158 1.415-1.413z"/></svg></div>
<div align="center" id="slider-fill3"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
        style="position:relative; top:-23px;">
        <path fill='gray'
            d="M14 19h-4c-.276 0-.5.224-.5.5s.224.5.5.5h4c.276 0 .5-.224.5-.5s-.224-.5-.5-.5zm0 2h-4c-.276 0-.5.224-.5.5s.224.5.5.5h4c.276 0 .5-.224.5-.5s-.224-.5-.5-.5zm.25 2h-4.5l1.188.782c.154.138.38.218.615.218h.895c.234 0 .461-.08.615-.218l1.187-.782zm3.75-13.799c0 3.569-3.214 5.983-3.214 8.799h-1.989c-.003-1.858.87-3.389 1.721-4.867.761-1.325 1.482-2.577 1.482-3.932 0-2.592-2.075-3.772-4.003-3.772-1.925 0-3.997 1.18-3.997 3.772 0 1.355.721 2.607 1.482 3.932.851 1.478 1.725 3.009 1.72 4.867h-1.988c0-2.816-3.214-5.23-3.214-8.799 0-3.723 2.998-5.772 5.997-5.772 3.001 0 6.003 2.051 6.003 5.772zm4-.691v1.372h-2.538c.02-.223.038-.448.038-.681 0-.237-.017-.464-.035-.69h2.535zm-10.648-6.553v-1.957h1.371v1.964c-.242-.022-.484-.035-.726-.035-.215 0-.43.01-.645.028zm-3.743 1.294l-1.04-1.94 1.208-.648 1.037 1.933c-.418.181-.822.401-1.205.655zm10.586 1.735l1.942-1.394.799 1.115-2.054 1.473c-.191-.43-.423-.827-.687-1.194zm-3.01-2.389l1.038-1.934 1.208.648-1.041 1.941c-.382-.254-.786-.473-1.205-.655zm-10.068 3.583l-2.054-1.472.799-1.115 1.942 1.393c-.264.366-.495.763-.687 1.194zm13.707 6.223l2.354.954-.514 1.271-2.425-.982c.21-.397.408-.812.585-1.243zm-13.108 1.155l-2.356 1.06-.562-1.251 2.34-1.052c.173.433.371.845.578 1.243zm-1.178-3.676h-2.538v-1.372h2.535c-.018.226-.035.454-.035.691 0 .233.018.458.038.681z" /></svg></div>
<div align="center" id="slider-fill5"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" style = "position:relative; top:-23px;"><path fill = 'gray' d="M6 11v-4l-6 5 6 5v-4h12v4l6-5-6-5v4z"/></svg></div>
<div align="center" id="slider-fill4"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
        style="position:relative; top:-23px;">
        <path fill='gray'
            d="M12 0l-11 6v12.131l11 5.869 11-5.869v-12.066l-11-6.065zm9 11.623l-3 1.569v-3.26l3-1.601v3.292zm-13-.654l3 1.625v3.186l-3-1.614v-3.197zm.9-1.799l2.986-1.603 3.132 1.688-3.014 1.608-3.104-1.693zm4.1 3.43l3-1.6v3.238l-3 1.569v-3.207zm4.138-4.475l-3.139-1.691 2.801-1.503 3.11 1.715-2.772 1.479zm-2.424-4.345l-2.825 1.517-2.728-1.47 2.834-1.546 2.719 1.499zm-7.649 1.19l2.711 1.46-2.973 1.596-2.67-1.456 2.932-1.6zm-1.065 4.908v3.204l-3-1.636v-3.216l3 1.648zm-3 3.843l3 1.636v3.185l-3-1.611v-3.21zm5 5.888v-3.169l3 1.614v3.146l-3-1.591zm5-1.545l3-1.569v3.104l-3 1.601v-3.136zm5 .468v-3.083l3-1.569v3.051l-3 1.601z" /></svg></div>
    <script type="text/javascript">
        "use strict";        var vertexShaderSource = `#version 300 es
        // an attribute is an input (in) to a vertex shader.
// It will receive data from a buffer
in vec2 a_position;
in vec2 a_texCoord;// Used to pass in the resolution of the canvas
uniform vec2 u_resolution;// Used to pass the texture coordinates to the fragment shader
out vec2 v_texCoord;// all shaders have a main function
void main() {// convert the position from pixels to 0.0 to 1.0
vec2 zeroToOne = a_position / u_resolution;// convert from 0->1 to 0->2
vec2 zeroToTwo = zeroToOne * 2.0;// convert from 0->2 to -1->+1 (clipspace)
vec2 clipSpace = zeroToTwo - 1.0;gl_Position = vec4(clipSpace * vec2(1, -1), 0, 1);// pass the texCoord to the fragment shader
// The GPU will interpolate this value between points.
v_texCoord = a_texCoord;
}
`;        var fragmentShaderSource = `#version 300 es
// fragment shaders don't have a default precision so we need
// to pick one. highp is a good default. It means "high precision"
precision highp float;float rand(vec2 co) {
    return fract(sin(dot(co.xy ,vec2(12.9898,78.233))) * 43758.5453);
}// Copyright(c) 2021 Björn Ottosson
//
// Permission is hereby granted, free of charge, to any person obtaining a copy of
// this softwareand associated documentation files(the "Software"), to deal in
// the Software without restriction, including without limitation the rights to
// use, copy, modify, merge, publish, distribute, sublicense, and /or sell copies
// of the Software, and to permit persons to whom the Software is furnished to do
// so, subject to the following conditions :
// The above copyright noticeand this permission notice shall be included in all
// copies or substantial portions of the Software.
// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.IN NO EVENT SHALL THE
// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
// SOFTWARE.
#define M_PI 3.1415926535897932384626433832795
float cbrt( float x )
{
    return sign(x)*pow(abs(x),1.0f/3.0f);
}float srgb_transfer_function(float a)
{
	return .0031308f >= a ? 12.92f * a : 1.055f * pow(a, .4166666666666667f) - .055f;
}float srgb_transfer_function_inv(float a)
{
	return .04045f < a ? pow((a + .055f) / 1.055f, 2.4f) : a / 12.92f;
}vec3 linear_srgb_to_oklab(vec3 c)
{
	float l = 0.4122214708f * c.r + 0.5363325363f * c.g + 0.0514459929f * c.b;
	float m = 0.2119034982f * c.r + 0.6806995451f * c.g + 0.1073969566f * c.b;
	float s = 0.0883024619f * c.r + 0.2817188376f * c.g + 0.6299787005f * c.b;	float l_ = cbrt(l);
	float m_ = cbrt(m);
	float s_ = cbrt(s);	return vec3(
		0.2104542553f * l_ + 0.7936177850f * m_ - 0.0040720468f * s_,
		1.9779984951f * l_ - 2.4285922050f * m_ + 0.4505937099f * s_,
		0.0259040371f * l_ + 0.7827717662f * m_ - 0.8086757660f * s_
	);
}vec3 oklab_to_linear_srgb(vec3 c)
{
	float l_ = c.x + 0.3963377774f * c.y + 0.2158037573f * c.z;
	float m_ = c.x - 0.1055613458f * c.y - 0.0638541728f * c.z;
	float s_ = c.x - 0.0894841775f * c.y - 1.2914855480f * c.z;	float l = l_ * l_ * l_;
	float m = m_ * m_ * m_;
	float s = s_ * s_ * s_;	return vec3(
		+4.0767416621f * l - 3.3077115913f * m + 0.2309699292f * s,
		-1.2684380046f * l + 2.6097574011f * m - 0.3413193965f * s,
		-0.0041960863f * l - 0.7034186147f * m + 1.7076147010f * s
	);
}// Finds the maximum saturation possible for a given hue that fits in sRGB
// Saturation here is defined as S = C/L
// a and b must be normalized so a^2 + b^2 == 1
float compute_max_saturation(float a, float b)
{
	// Max saturation will be when one of r, g or b goes below zero.	// Select different coefficients depending on which component goes below zero first
	float k0, k1, k2, k3, k4, wl, wm, ws;	if (-1.88170328f * a - 0.80936493f * b > 1.f)
	{
		// Red component
		k0 = +1.19086277f; k1 = +1.76576728f; k2 = +0.59662641f; k3 = +0.75515197f; k4 = +0.56771245f;
		wl = +4.0767416621f; wm = -3.3077115913f; ws = +0.2309699292f;
	}
	else if (1.81444104f * a - 1.19445276f * b > 1.f)
	{
		// Green component
		k0 = +0.73956515f; k1 = -0.45954404f; k2 = +0.08285427f; k3 = +0.12541070f; k4 = +0.14503204f;
		wl = -1.2684380046f; wm = +2.6097574011f; ws = -0.3413193965f;
	}
	else
	{
		// Blue component
		k0 = +1.35733652f; k1 = -0.00915799f; k2 = -1.15130210f; k3 = -0.50559606f; k4 = +0.00692167f;
		wl = -0.0041960863f; wm = -0.7034186147f; ws = +1.7076147010f;
	}	// Approximate max saturation using a polynomial:
	float S = k0 + k1 * a + k2 * b + k3 * a * a + k4 * a * b;	// Do one step Halley's method to get closer
	// this gives an error less than 10e6, except for some blue hues where the dS/dh is close to infinite
	// this should be sufficient for most applications, otherwise do two/three steps
    float k_l = +0.3963377774f * a + 0.2158037573f * b;
	float k_m = -0.1055613458f * a - 0.0638541728f * b;
	float k_s = -0.0894841775f * a - 1.2914855480f * b;	{
		float l_ = 1.f + S * k_l;
		float m_ = 1.f + S * k_m;
		float s_ = 1.f + S * k_s;		float l = l_ * l_ * l_;
		float m = m_ * m_ * m_;
		float s = s_ * s_ * s_;		float l_dS = 3.f * k_l * l_ * l_;
		float m_dS = 3.f * k_m * m_ * m_;
		float s_dS = 3.f * k_s * s_ * s_;		float l_dS2 = 6.f * k_l * k_l * l_;
		float m_dS2 = 6.f * k_m * k_m * m_;
		float s_dS2 = 6.f * k_s * k_s * s_;		float f = wl * l + wm * m + ws * s;
		float f1 = wl * l_dS + wm * m_dS + ws * s_dS;
		float f2 = wl * l_dS2 + wm * m_dS2 + ws * s_dS2;		S = S - f * f1 / (f1 * f1 - 0.5f * f * f2);
	}	return S;
}// finds L_cusp and C_cusp for a given hue
// a and b must be normalized so a^2 + b^2 == 1
vec2 find_cusp(float a, float b)
{
	// First, find the maximum saturation (saturation S = C/L)
	float S_cusp = compute_max_saturation(a, b);	// Convert to linear sRGB to find the first point where at least one of r,g or b >= 1:
	vec3 rgb_at_max = oklab_to_linear_srgb(vec3( 1, S_cusp * a, S_cusp * b ));
	float L_cusp = cbrt(1.f / max(max(rgb_at_max.r, rgb_at_max.g), rgb_at_max.b));
	float C_cusp = L_cusp * S_cusp;	return vec2( L_cusp , C_cusp );
}// Finds intersection of the line defined by 
// L = L0 * (1 - t) + t * L1;
// C = t * C1;
// a and b must be normalized so a^2 + b^2 == 1
float find_gamut_intersection(float a, float b, float L1, float C1, float L0, vec2 cusp)
{
	// Find the intersection for upper and lower half seprately
	float t;
	if (((L1 - L0) * cusp.y - (cusp.x - L0) * C1) <= 0.f)
	{
		// Lower half	
        t = cusp.y * L0 / (C1 * cusp.x + cusp.y * (L0 - L1));
	}
	else
	{
		// Upper half		// First intersect with triangle
		t = cusp.y * (L0 - 1.f) / (C1 * (cusp.x - 1.f) + cusp.y * (L0 - L1));		// Then one step Halley's method
		{
			float dL = L1 - L0;
			float dC = C1;			float k_l = +0.3963377774f * a + 0.2158037573f * b;
			float k_m = -0.1055613458f * a - 0.0638541728f * b;
			float k_s = -0.0894841775f * a - 1.2914855480f * b;			float l_dt = dL + dC * k_l;
			float m_dt = dL + dC * k_m;
			float s_dt = dL + dC * k_s;
			// If higher accuracy is required, 2 or 3 iterations of the following block can be used:
			{
				float L = L0 * (1.f - t) + t * L1;
				float C = t * C1;				float l_ = L + C * k_l;
				float m_ = L + C * k_m;
				float s_ = L + C * k_s;				float l = l_ * l_ * l_;
				float m = m_ * m_ * m_;
				float s = s_ * s_ * s_;				float ldt = 3.f * l_dt * l_ * l_;
				float mdt = 3.f * m_dt * m_ * m_;
				float sdt = 3.f * s_dt * s_ * s_;				float ldt2 = 6.f * l_dt * l_dt * l_;
				float mdt2 = 6.f * m_dt * m_dt * m_;
				float sdt2 = 6.f * s_dt * s_dt * s_;				float r = 4.0767416621f * l - 3.3077115913f * m + 0.2309699292f * s - 1.f;
				float r1 = 4.0767416621f * ldt - 3.3077115913f * mdt + 0.2309699292f * sdt;
				float r2 = 4.0767416621f * ldt2 - 3.3077115913f * mdt2 + 0.2309699292f * sdt2;				float u_r = r1 / (r1 * r1 - 0.5f * r * r2);
				float t_r = -r * u_r;				float g = -1.2684380046f * l + 2.6097574011f * m - 0.3413193965f * s - 1.f;
				float g1 = -1.2684380046f * ldt + 2.6097574011f * mdt - 0.3413193965f * sdt;
				float g2 = -1.2684380046f * ldt2 + 2.6097574011f * mdt2 - 0.3413193965f * sdt2;				float u_g = g1 / (g1 * g1 - 0.5f * g * g2);
				float t_g = -g * u_g;				float b = -0.0041960863f * l - 0.7034186147f * m + 1.7076147010f * s - 1.f;
				float b1 = -0.0041960863f * ldt - 0.7034186147f * mdt + 1.7076147010f * sdt;
				float b2 = -0.0041960863f * ldt2 - 0.7034186147f * mdt2 + 1.7076147010f * sdt2;				float u_b = b1 / (b1 * b1 - 0.5f * b * b2);
				float t_b = -b * u_b;				t_r = u_r >= 0.f ? t_r : 10000.f;
				t_g = u_g >= 0.f ? t_g : 10000.f;
				t_b = u_b >= 0.f ? t_b : 10000.f;				t += min(t_r, min(t_g, t_b));
			}
		}
	}	return t;
}float find_gamut_intersection(float a, float b, float L1, float C1, float L0)
{
	// Find the cusp of the gamut triangle
	vec2 cusp = find_cusp(a, b);	return find_gamut_intersection(a, b, L1, C1, L0, cusp);
}vec3 gamut_clip_preserve_chroma(vec3 rgb)
{
	if (rgb.r < 1.f && rgb.g < 1.f && rgb.b < 1.f && rgb.r > 0.f && rgb.g > 0.f && rgb.b > 0.f)
		return rgb;	vec3 lab = linear_srgb_to_oklab(rgb);	float L = lab.x;
	float eps = 0.00001f;
	float C = max(eps, sqrt(lab.y * lab.y + lab.z * lab.z));
	float a_ = lab.y / C;
	float b_ = lab.z / C;	float L0 = clamp(L, 0.f, 1.f);	float t = find_gamut_intersection(a_, b_, L, C, L0);
	float L_clipped = L0 * (1.f - t) + t * L;
	float C_clipped = t * C;	return oklab_to_linear_srgb(vec3( L_clipped, C_clipped * a_, C_clipped * b_ ));
}vec3 gamut_clip_project_to_0_5(vec3 rgb)
{
	if (rgb.r < 1.f && rgb.g < 1.f && rgb.b < 1.f && rgb.r > 0.f && rgb.g > 0.f && rgb.b > 0.f)
		return rgb;	vec3 lab = linear_srgb_to_oklab(rgb);	float L = lab.x;
	float eps = 0.00001f;
	float C = max(eps, sqrt(lab.y * lab.y + lab.z * lab.z));
	float a_ = lab.y / C;
	float b_ = lab.z / C;	float L0 = 0.5;	float t = find_gamut_intersection(a_, b_, L, C, L0);
	float L_clipped = L0 * (1.f - t) + t * L;
	float C_clipped = t * C;	return oklab_to_linear_srgb(vec3( L_clipped, C_clipped * a_, C_clipped * b_ ));
}vec3 gamut_clip_project_to_L_cusp(vec3 rgb)
{
	if (rgb.r < 1.f && rgb.g < 1.f && rgb.b < 1.f && rgb.r > 0.f && rgb.g > 0.f && rgb.b > 0.f)
		return rgb;	vec3 lab = linear_srgb_to_oklab(rgb);	float L = lab.x;
	float eps = 0.00001f;
	float C = max(eps, sqrt(lab.y * lab.y + lab.z * lab.z));
	float a_ = lab.y / C;
	float b_ = lab.z / C;	// The cusp is computed here and in find_gamut_intersection, an optimized solution would only compute it once.
	vec2 cusp = find_cusp(a_, b_);	float L0 = cusp.x;	float t = find_gamut_intersection(a_, b_, L, C, L0);	float L_clipped = L0 * (1.f - t) + t * L;
	float C_clipped = t * C;	return oklab_to_linear_srgb(vec3( L_clipped, C_clipped * a_, C_clipped * b_ ));
}vec3 gamut_clip_adaptive_L0_0_5(vec3 rgb, float alpha)
{
	if (rgb.r < 1.f && rgb.g < 1.f && rgb.b < 1.f && rgb.r > 0.f && rgb.g > 0.f && rgb.b > 0.f)
		return rgb;	vec3 lab = linear_srgb_to_oklab(rgb);	float L = lab.x;
	float eps = 0.00001f;
	float C = max(eps, sqrt(lab.y * lab.y + lab.z * lab.z));
	float a_ = lab.y / C;
	float b_ = lab.z / C;	float Ld = L - 0.5f;
	float e1 = 0.5f + abs(Ld) + alpha * C;
	float L0 = 0.5f * (1.f + sign(Ld) * (e1 - sqrt(e1 * e1 - 2.f * abs(Ld))));	float t = find_gamut_intersection(a_, b_, L, C, L0);
	float L_clipped = L0 * (1.f - t) + t * L;
	float C_clipped = t * C;	return oklab_to_linear_srgb(vec3( L_clipped, C_clipped * a_, C_clipped * b_ ));
}vec3 gamut_clip_adaptive_L0_L_cusp(vec3 rgb, float alpha)
{
	if (rgb.r < 1.f && rgb.g < 1.f && rgb.b < 1.f && rgb.r > 0.f && rgb.g > 0.f && rgb.b > 0.f)
		return rgb;	vec3 lab = linear_srgb_to_oklab(rgb);	float L = lab.x;
	float eps = 0.00001f;
	float C = max(eps, sqrt(lab.y * lab.y + lab.z * lab.z));
	float a_ = lab.y / C;
	float b_ = lab.z / C;	// The cusp is computed here and in find_gamut_intersection, an optimized solution would only compute it once.
	vec2 cusp = find_cusp(a_, b_);	float Ld = L - cusp.x;
	float k = 2.f * (Ld > 0.f ? 1.f - cusp.x : cusp.x);	float e1 = 0.5f * k + abs(Ld) + alpha * C / k;
	float L0 = cusp.x + 0.5f * (sign(Ld) * (e1 - sqrt(e1 * e1 - 2.f * k * abs(Ld))));	float t = find_gamut_intersection(a_, b_, L, C, L0);
	float L_clipped = L0 * (1.f - t) + t * L;
	float C_clipped = t * C;	return oklab_to_linear_srgb(vec3( L_clipped, C_clipped * a_, C_clipped * b_ ));
}float toe(float x)
{
	float k_1 = 0.206f;
	float k_2 = 0.03f;
	float k_3 = (1.f + k_1) / (1.f + k_2);
	return 0.5f * (k_3 * x - k_1 + sqrt((k_3 * x - k_1) * (k_3 * x - k_1) + 4.f * k_2 * k_3 * x));
}float toe_inv(float x)
{
	float k_1 = 0.206f;
	float k_2 = 0.03f;
	float k_3 = (1.f + k_1) / (1.f + k_2);
	return (x * x + k_1 * x) / (k_3 * (x + k_2));
}vec2 to_ST(vec2 cusp)
{
	float L = cusp.x;
	float C = cusp.y;
	return vec2( C / L, C / (1.f - L) );
}// Returns a smooth approximation of the location of the cusp
// This polynomial was created by an optimization process
// It has been designed so that S_mid < S_max and T_mid < T_max
vec2 get_ST_mid(float a_, float b_)
{
	float S = 0.11516993f + 1.f / (
		+7.44778970f + 4.15901240f * b_
		+ a_ * (-2.19557347f + 1.75198401f * b_
			+ a_ * (-2.13704948f - 10.02301043f * b_
				+ a_ * (-4.24894561f + 5.38770819f * b_ + 4.69891013f * a_
					)))
		);	float T = 0.11239642f + 1.f / (
		+1.61320320f - 0.68124379f * b_
		+ a_ * (+0.40370612f + 0.90148123f * b_
			+ a_ * (-0.27087943f + 0.61223990f * b_
				+ a_ * (+0.00299215f - 0.45399568f * b_ - 0.14661872f * a_
					)))
		);	return vec2( S, T );
}vec3 get_Cs(float L, float a_, float b_)
{
	vec2 cusp = find_cusp(a_, b_);	float C_max = find_gamut_intersection(a_, b_, L, 1.f, L, cusp);
	vec2 ST_max = to_ST(cusp);
	// Scale factor to compensate for the curved part of gamut shape:
	float k = C_max / min((L * ST_max.x), (1.f - L) * ST_max.y);	float C_mid;
	{
		vec2 ST_mid = get_ST_mid(a_, b_);		// Use a soft minimum function, instead of a sharp triangle shape to get a smooth value for chroma.
		float C_a = L * ST_mid.x;
		float C_b = (1.f - L) * ST_mid.y;
		C_mid = 0.9f * k * sqrt(sqrt(1.f / (1.f / (C_a * C_a * C_a * C_a) + 1.f / (C_b * C_b * C_b * C_b))));
	}	float C_0;
	{
		// for C_0, the shape is independent of hue, so vec2 are constant. Values picked to roughly be the average values of vec2.
		float C_a = L * 0.4f;
		float C_b = (1.f - L) * 0.8f;		// Use a soft minimum function, instead of a sharp triangle shape to get a smooth value for chroma.
		C_0 = sqrt(1.f / (1.f / (C_a * C_a) + 1.f / (C_b * C_b)));
	}	return vec3( C_0, C_mid, C_max );
}vec3 okhsl_to_srgb(vec3 hsl)
{
	float h = hsl.x;
	float s = hsl.y;
	float l = hsl.z;	if (l == 1.0f)
	{
		return vec3( 1.f, 1.f, 1.f );
	}	else if (l == 0.f)
	{
		return vec3( 0.f, 0.f, 0.f );
	}	float a_ = cos(2.f * M_PI * h);
	float b_ = sin(2.f * M_PI * h);
	float L = toe_inv(l);	vec3 cs = get_Cs(L, a_, b_);
	float C_0 = cs.x;
	float C_mid = cs.y;
	float C_max = cs.z;
    float mid = 0.8f;
	float mid_inv = 1.25f;	float C, t, k_0, k_1, k_2;	if (s < mid)
	{
		t = mid_inv * s;		k_1 = mid * C_0;
		k_2 = (1.f - k_1 / C_mid);		C = t * k_1 / (1.f - k_2 * t);
	}
	else
	{
		t = (s - mid)/ (1.f - mid);		k_0 = C_mid;
		k_1 = (1.f - mid) * C_mid * C_mid * mid_inv * mid_inv / C_0;
		k_2 = (1.f - (k_1) / (C_max - C_mid));		C = k_0 + t * k_1 / (1.f - k_2 * t);
	}	vec3 rgb = oklab_to_linear_srgb(vec3( L, C * a_, C * b_ ));
	return vec3(
		srgb_transfer_function(rgb.r),
		srgb_transfer_function(rgb.g),
		srgb_transfer_function(rgb.b)
	);
}vec3 srgb_to_okhsl(vec3 rgb)
{
	vec3 lab = linear_srgb_to_oklab(vec3(
		srgb_transfer_function_inv(rgb.r),
		srgb_transfer_function_inv(rgb.g),
		srgb_transfer_function_inv(rgb.b)
		));	float C = sqrt(lab.y * lab.y + lab.z * lab.z);
	float a_ = lab.y / C;
	float b_ = lab.z / C;	float L = lab.x;
	float h = 0.5f + 0.5f * atan(-lab.z, -lab.y) / M_PI;	vec3 cs = get_Cs(L, a_, b_);
	float C_0 = cs.x;
	float C_mid = cs.y;
	float C_max = cs.z;	// Inverse of the interpolation in okhsl_to_srgb:
    float mid = 0.8f;
	float mid_inv = 1.25f;	float s;
	if (C < C_mid)
	{
		float k_1 = mid * C_0;
		float k_2 = (1.f - k_1 / C_mid);		float t = C / (k_1 + k_2 * C);
		s = t * mid;
	}
	else
	{
		float k_0 = C_mid;
		float k_1 = (1.f - mid) * C_mid * C_mid * mid_inv * mid_inv / C_0;
		float k_2 = (1.f - (k_1) / (C_max - C_mid));		float t = (C - k_0) / (k_1 + k_2 * (C - k_0));
		s = mid + (1.f - mid) * t;
	}	float l = toe(L);
	return vec3( h, s, l );
}
vec3 okhsv_to_srgb(vec3 hsv)
{
	float h = hsv.x;
	float s = hsv.y;
	float v = hsv.z;	float a_ = cos(2.f * M_PI * h);
	float b_ = sin(2.f * M_PI * h);
	vec2 cusp = find_cusp(a_, b_);
	vec2 ST_max = to_ST(cusp);
	float S_max = ST_max.x;
	float T_max = ST_max.y;
	float S_0 = 0.5f;
	float k = 1.f- S_0 / S_max;	// first we compute L and V as if the gamut is a perfect triangle:	// L, C when v==1:
	float L_v = 1.f   - s * S_0 / (S_0 + T_max - T_max * k * s);
	float C_v = s * T_max * S_0 / (S_0 + T_max - T_max * k * s);	float L = v * L_v;
	float C = v * C_v;	// then we compensate for both toe and the curved top part of the triangle:
	float L_vt = toe_inv(L_v);
	float C_vt = C_v * L_vt / L_v;	float L_new = toe_inv(L);
	C = C * L_new / L;
	L = L_new;	vec3 rgb_scale = oklab_to_linear_srgb(vec3( L_vt, a_ * C_vt, b_ * C_vt ));
	float scale_L = cbrt(1.f / max(max(rgb_scale.r, rgb_scale.g), max(rgb_scale.b, 0.f)));	L = L * scale_L;
	C = C * scale_L;	vec3 rgb = oklab_to_linear_srgb(vec3( L, C * a_, C * b_ ));
	return vec3(
		srgb_transfer_function(rgb.r),
		srgb_transfer_function(rgb.g),
		srgb_transfer_function(rgb.b)
	);
}vec3 srgb_to_okhsv(vec3 rgb)
{
	vec3 lab = linear_srgb_to_oklab(vec3(
		srgb_transfer_function_inv(rgb.r),
		srgb_transfer_function_inv(rgb.g),
		srgb_transfer_function_inv(rgb.b)
		));	float C = sqrt(lab.y * lab.y + lab.z * lab.z);
	float a_ = lab.y / C;
	float b_ = lab.z / C;	float L = lab.x;
	float h = 0.5f + 0.5f * atan(-lab.z, -lab.y) / M_PI;	vec2 cusp = find_cusp(a_, b_);
	vec2 ST_max = to_ST(cusp);
	float S_max = ST_max.x;
	float T_max = ST_max.y;
	float S_0 = 0.5f;
	float k = 1.f - S_0 / S_max;	// first we find L_v, C_v, L_vt and C_vt
    float t = T_max / (C + L * T_max);
	float L_v = t * L;
	float C_v = t * C;	float L_vt = toe_inv(L_v);
	float C_vt = C_v * L_vt / L_v;	// we can then use these to invert the step that compensates for the toe and the curved top part of the triangle:
	vec3 rgb_scale = oklab_to_linear_srgb(vec3( L_vt, a_ * C_vt, b_ * C_vt ));
	float scale_L = cbrt(1.f / max(max(rgb_scale.r, rgb_scale.g), max(rgb_scale.b, 0.f)));	L = L / scale_L;
	C = C / scale_L;	C = C * toe(L) / L;
	L = toe(L);	// we can now compute v and s:
    float v = L / L_v;
	float s = (S_0 + T_max) * C_v / ((T_max * S_0) + T_max * k * C_v);	return vec3 (h, s, v );
}vec3 hsl2rgb( in vec3 c )
{
    vec3 rgb = clamp( abs(mod(c.x*6.0+vec3(0.0,4.0,2.0),6.0)-3.0)-1.0, 0.0, 1.0 );    return c.z + c.y * (rgb-0.5)*(1.0-abs(2.0*c.z-1.0));
}vec3 rgb2hsl( in vec3 c ){
  float h = 0.0;
	float s = 0.0;
	float l = 0.0;
	float r = c.r;
	float g = c.g;
	float b = c.b;
	float cMin = min( r, min( g, b ) );
	float cMax = max( r, max( g, b ) );	l = ( cMax + cMin ) / 2.0;
	if ( cMax > cMin ) {
		float cDelta = cMax - cMin;
        //s = l < .05 ? cDelta / ( cMax + cMin ) : cDelta / ( 2.0 - ( cMax + cMin ) ); Original
		s = l < .0 ? cDelta / ( cMax + cMin ) : cDelta / ( 2.0 - ( cMax + cMin ) );
		if ( r == cMax ) {
			h = ( g - b ) / cDelta;
		} else if ( g == cMax ) {
			h = 2.0 + ( b - r ) / cDelta;
		} else {
			h = 4.0 + ( r - g ) / cDelta;
		}		if ( h < 0.0) {
			h += 6.0;
		}
		h = h / 6.0;
	}
	return vec3( h, s, l );
}
bool inDither( float h, float antiHue, float antiRad) {
	return (((h > antiHue - antiRad) && (h < antiHue + antiRad))) ||
	(((h - 1.0f > antiHue - antiRad) && (h - 1.0f < antiHue + antiRad))) ||
	(((1.0f + h > antiHue - antiRad) && (1.0f + h < antiHue + antiRad)));
}
float ditherHue( float h, float r, float rp, float antiRad) {
	float rightBound = h - antiRad * rp;
    if (rightBound < -0.5f) rightBound = rightBound + 1.0f;
    float leftBound = h + antiRad * rp;
    if (leftBound > 0.5f) leftBound = leftBound - 1.0f;
	if ((h > h - antiRad) && (h < h + antiRad))
        if (((h - (h - antiRad)) / (2.0f * antiRad)) < r)
            h = rightBound;
        else h = leftBound;
    else if ((h - 1.0f > h - antiRad) && (h - 1.0f < h + antiRad))
        if (((h - 1.0f - (h - antiRad)) / (2.0f * antiRad)) < r)
            h = rightBound;
        else h = leftBound;
    else if ((1.0f + h > h - antiRad) && (1.0f + h < h + antiRad))
        if (((1.0f + h - (h - antiRad)) / (2.0f * antiRad)) < r)
            h = rightBound;
        else h = leftBound;
	if (h < 0.0) h = h + 1.0;
	return h;
}
float dither( float v, float r, float antiRad, float lim) {
    float lowerBound = max(0.0, v - antiRad);
    if (v - antiRad < lim) lowerBound = mix(max(0.0, v - antiRad), lim, antiRad);
    float upperBound = min(lowerBound + 2.0 * antiRad, 1.0 - lim);
    if (v + antiRad > 1.0 - lim) {
        upperBound = mix(min(1.0, v + antiRad), 1.0 - lim, antiRad);
        lowerBound = max(lowerBound, upperBound - 2.0 * antiRad);
    }
    return mix(lowerBound, upperBound, r);
}
float decide( float base, float lop, float rop, float p, float r) {
    if (p > 0.0) {
        if (r > p) return base;
        else return rop;
    }
    else {
        if (r > -1.0 * p) return base;
        else return lop;
    }
}
uniform sampler2D u_image;
uniform float time;
uniform vec2 res;
uniform float hue;
uniform float sat;
uniform float lum;
uniform float lim;
// the texCoords passed in from the vertex shader.
in vec2 v_texCoord;// we need to declare an output for the fragment shader
out vec4 outColor;void main()
{
    vec2 uv = v_texCoord;
    float iTime = time / 1.0;
	vec4 tex = texture(u_image, uv);
    vec3 hsl = srgb_to_okhsl(tex.rgb);
    vec2 biguv = floor(uv*res)/res;
    biguv = biguv + 0.5*1.0/res;
    vec3 bighsl = srgb_to_okhsl(texture(u_image, biguv).rgb);
    float r1 = rand(vec2(biguv.x, iTime));
    r1 = rand(vec2(r1, biguv.y));
    float r2 = rand(vec2(biguv.x, biguv.y));
    r2 = rand(vec2(r2, iTime));
    float r3 = rand(vec2(r1, r2));
    vec3 col = okhsl_to_srgb(vec3(decide(bighsl.x, bighsl.z, bighsl.y, hue, r1),
        decide(bighsl.y, bighsl.x, bighsl.z, sat, r2),
        decide(bighsl.z, bighsl.y, bighsl.x, lum, r3)));
    outColor = vec4(col,tex.w);
}`;        // Get A WebGL context
        /** @type {HTMLCanvasElement} */
		var toggle = true;
		const btn = document.getElementById('btn');
        var canvas = document.querySelector("canvas");
        var gl = canvas.getContext("webgl2");        // setup GLSL program
        var program = webglUtils.createProgramFromSources(gl,
            [vertexShaderSource, fragmentShaderSource]);        // look up where the vertex data needs to go.
        var positionAttributeLocation = gl.getAttribLocation(program, "a_position");
        var texCoordAttributeLocation = gl.getAttribLocation(program, "a_texCoord");
		var locs = {
        'time': gl.getUniformLocation(program, "time"),
        'hue': gl.getUniformLocation(program, "hue"),
        'sat': gl.getUniformLocation(program, "sat"),
        'lum': gl.getUniformLocation(program, "lum"),
        'res': gl.getUniformLocation(program, "res"),
        'lim': gl.getUniformLocation(program, "lim")
    };
        var timeLocation = gl.getUniformLocation(program, "time");
        var hueLocation = gl.getUniformLocation(program, "antiHue");
        var radLocation = gl.getUniformLocation(program, "antiRad");
        var image = new Image();
		function resizeCanvas(image, multiplier) {
			const maxWidth = Math.min(600, screen.width * 0.85);
            multiplier = multiplier || 1;
            const width = Math.min(image.width, maxWidth) | 0;
            const height = Math.min(image.height, image.height/image.width * maxWidth) | 0;
            if (canvas.width !== width || canvas.height !== height) {
                canvas.width = width;
                canvas.height = height;
                return true;
            }
            return false;
        }
		function firstRender(time) {
            time = time || 0;            // lookup uniforms
            var resolutionLocation = gl.getUniformLocation(program, "u_resolution");
            var imageLocation = gl.getUniformLocation(program, "u_image");
			var resLocation = gl.getUniformLocation(program, "res");
            var vao = gl.createVertexArray();            // and make it the one we're currently working with
            gl.bindVertexArray(vao);            // Create a buffer and put a single pixel space rectangle in
            // it (2 triangles)
            var positionBuffer = gl.createBuffer();            // Turn on the attribute
            gl.enableVertexAttribArray(positionAttributeLocation);            // Bind it to ARRAY_BUFFER (think of it as ARRAY_BUFFER = positionBuffer)
            gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);            // Tell the attribute how to get data out of positionBuffer (ARRAY_BUFFER)
            gl.vertexAttribPointer(
                positionAttributeLocation, 2, gl.FLOAT, false, 0, 0);            // provide texture coordinates for the rectangle.
            var texCoordBuffer = gl.createBuffer();
            gl.bindBuffer(gl.ARRAY_BUFFER, texCoordBuffer);
            gl.bufferData(gl.ARRAY_BUFFER, new Float32Array([
                0.0, 0.0,
                1.0, 0.0,
                0.0, 1.0,
                0.0, 1.0,
                1.0, 0.0,
                1.0, 1.0,
            ]), gl.STATIC_DRAW);            // Turn on the attribute
            gl.enableVertexAttribArray(texCoordAttributeLocation);            // Tell the attribute how to get data out of texCoordBuffer (ARRAY_BUFFER)
            gl.vertexAttribPointer(
                texCoordAttributeLocation, 2, gl.FLOAT, false, 0, 0);            // Create a texture.
            var texture = gl.createTexture();            // make unit 0 the active texture uint
            // (ie, the unit all other texture commands will affect
            gl.bindTexture(gl.TEXTURE_2D, texture);            // Set the parameters so we don't need mips and so we're not filtering
            // and we don't repeat
            gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.NEAREST);
            gl.texImage2D(gl.TEXTURE_2D,
                0,
                gl.RGBA,
                gl.RGBA,
                gl.UNSIGNED_BYTE,
                image);
			resizeCanvas(image);            // Tell WebGL how to convert from clip space to pixels
            gl.viewport(0, 0, gl.canvas.width, gl.canvas.height);            // Clear the canvas
            gl.useProgram(program);            // Bind the attribute/buffer set we want.
            // pixels to clipspace in the shader
            gl.uniform2f(resolutionLocation, image.width, image.height);            // Tell the shader to get the texture from texture unit 0
            gl.uniform1i(imageLocation, 0);            // Update the time
            gl.uniform1f(locs.time, time * 0.001);
            gl.uniform1f(locs.hue, sliderFill.value());
            gl.uniform1f(locs.sat, sliderFill2.value());
            gl.uniform1f(locs.lum, sliderFill3.value());
            gl.uniform2f(locs.res, image.width*Math.pow(1 - sliderFill4.value(), 4) + 1,image.height*Math.pow(1 - sliderFill4.value(), 4) + 1);
            gl.uniform1f(locs.lim, sliderFill5.value() / 2.0);
            // in setRectangle puts data in the position buffer
            gl.bindBuffer(gl.ARRAY_BUFFER, positionBuffer);            // Set a rectangle the same size as the image.
            setRectangle(gl, 0, 0, image.width, image.height);            // Draw the rectangle.
            gl.drawArrays(gl.TRIANGLES, 0, 6);
        }
		function render(time) {
        	time = time || 0;
        	gl.uniform1f(locs.time, time * 0.001);
            gl.uniform1f(locs.hue, sliderFill.value());
            gl.uniform1f(locs.sat, sliderFill2.value());
            gl.uniform1f(locs.lum, sliderFill3.value());
            gl.uniform2f(locs.res, image.width*Math.pow(1 - sliderFill4.value(), 4) + 1,image.height*Math.pow(1 - sliderFill4.value(), 4) + 1);
            gl.uniform1f(locs.lim, sliderFill5.value() / 2.0);
        	gl.drawArrays(gl.TRIANGLES, 0, 6);
        	if (!toggle) requestAnimationFrame(render);
    	}
		function setRectangle(gl, x, y, width, height) {
            var x1 = x;
            var x2 = x + width;
            var y1 = y;
            var y2 = y + height;
            gl.bufferData(gl.ARRAY_BUFFER, new Float32Array([
                x1, y1,
                x2, y1,
                x1, y2,
                x1, y2,
                x2, y1,
                x2, y2,
            ]), gl.STATIC_DRAW);
        }
		function PreviewImage() {
            var oFReader = new FileReader();
            oFReader.readAsDataURL(document.getElementById("uploadImage").files[0]);
			oFReader.onload = function (oFREvent) {
                image.src = oFREvent.target.result;
            };
			image.onload = function () {
                firstRender();
			};
        }
		function componentToHex(c) {
			var hex = c.toString(16);
			return hex.length == 1 ? "0" + hex : hex;
		}
		function rgbToHex(r, g, b) {
			return "#" + componentToHex(r) + componentToHex(g) + componentToHex(b);
		}
		function hexToRgb(hex) {
			var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
				return result ? {
				r: parseInt(result[1], 16),
				g: parseInt(result[2], 16),
				b: parseInt(result[3], 16)
  				} : null;
			}
		function hslToRgb(h, s, l){
		    var r, g, b;
    		if(s == 0){
       			 r = g = b = l; // achromatic
    		}else{
        		var hue2rgb = function hue2rgb(p, q, t){
	        	    if(t < 0) t += 1;
   	    		    if(t > 1) t -= 1;
    	    	    if(t < 1/6) return p + (q - p) * 6 * t;
	        	    if(t < 1/2) return q;
    	    	    if(t < 2/3) return p + (q - p) * (2/3 - t) * 6;
            		return p;
        		}
        		var q = l < 0.5 ? l * (1 + s) : l + s - l * s;
        		var p = 2 * l - q;
        		r = hue2rgb(p, q, h + 1/3);
        		g = hue2rgb(p, q, h);
        		b = hue2rgb(p, q, h - 1/3);
    		}
    		return [Math.round(r * 255), Math.round(g * 255), Math.round(b * 255)];
		}
		function rgbToHsl(r, g, b){
    		r /= 255, g /= 255, b /= 255;
    		var max = Math.max(r, g, b), min = Math.min(r, g, b);
    		var h, s, l = (max + min) / 2;
    		if(max == min){
       		 h = s = 0; // achromatic
    		}else{
        		var d = max - min;
        		s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
        		switch(max){
            		case r: h = (g - b) / d + (g < b ? 6 : 0); break;
            		case g: h = (b - r) / d + 2; break;
            		case b: h = (r - g) / d + 4; break;
        		}
        		h /= 6;
    		}
    		return [h, s, l];
		}
	var data = [0, 1.0];
    var data2 = [-1.0, 1.0];
    var sliderFill = d3
        .sliderBottom()
        .min(d3.min(data2))
        .max(d3.max(data2))
        .width(Math.min(300,screen.width*0.55))
        .tickFormat(d3.format('.0%'))
        .ticks(5)
        .default(0.0)
        .fill('#415794')
        .handle(
            d3
                .symbol()
                .type(d3.symbolCircle)
                .size(250)
        )
        .on('onchange', val => {
            if (toggle) render();
        });
    var gFill = d3
        .select('div#slider-fill')
        .append('svg')
        .attr('width', Math.min(350,screen.width*0.7))
        .attr('height', 65)
        .append('g')
        .attr('transform', 'translate(30,30)')
        .attr('fill', '#aaaaaa')
    gFill.call(sliderFill);
    var sliderFill2 = d3
        .sliderBottom()
        .min(d3.min(data2))
        .max(d3.max(data2))
        .width(Math.min(300,screen.width*0.55))
        .tickFormat(d3.format('.0%'))
        .ticks(5)
        .default(0.0)
        .fill('#415794')
        .handle(
            d3
                .symbol()
                .type(d3.symbolCircle)
                .size(250)
        )
        .on('onchange', val => {
            if (toggle) render();
        });
    var gFill2 = d3
        .select('div#slider-fill2')
        .append('svg')
        .attr('width', Math.min(350,screen.width*0.7))
        .attr('height', 65)
        .append('g')
        .attr('transform', 'translate(30,30)')
        .attr('fill', '#aaaaaa');
    gFill2.call(sliderFill2);
    var sliderFill3 = d3
        .sliderBottom()
        .min(d3.min(data2))
        .max(d3.max(data2))
        .width(Math.min(300,screen.width*0.55))
        .tickFormat(d3.format('.0%'))
        .ticks(5)
        .default(0.0)
        .fill('#415794')
        .handle(
            d3
                .symbol()
                .type(d3.symbolCircle)
                .size(250)
        )
        .on('onchange', val => {
            if (toggle) render();
        });
    var gFill3 = d3
        .select('div#slider-fill3')
        .append('svg')
        .attr('width', Math.min(350,screen.width*0.7))
        .attr('height', 65)
        .append('g')
        .attr('transform', 'translate(30,30)')
        .attr('fill', '#aaaaaa');
    gFill3.call(sliderFill3);
    var sliderFill4 = d3
        .sliderBottom()
        .min(d3.min(data))
        .max(d3.max(data))
        .width(Math.min(300,screen.width*0.55))
        .tickFormat(d3.format('.0%'))
        .ticks(5)
        .default(0.0)
        .fill('#415794')
        .handle(
            d3
                .symbol()
                .type(d3.symbolCircle)
                .size(250)
        )
        .on('onchange', val => {
            if (toggle) render();
        });
    var gFill4 = d3
        .select('div#slider-fill4')
        .append('svg')
        .attr('width', Math.min(350,screen.width*0.7))
        .attr('height', 65)
        .append('g')
        .attr('transform', 'translate(30,30)')
        .attr('fill', '#aaaaaa');
    gFill4.call(sliderFill4);
		window.onload=function(){
  			btn.addEventListener('click', function handleClick() {
				toggle = !toggle;
				if (toggle) btn.textContent = '▶️';
				else btn.textContent = '⏸';
				render();
			});
		}
    var sliderFill5 = d3
            .sliderBottom()
            .min(d3.min(data))
            .max(d3.max(data))
            .width(Math.min(300,screen.width*0.55))
            .tickFormat(d3.format('.0%'))
            .ticks(5)
            .default(0.50)
            .fill('#415794')
			.handle(
                d3
                .symbol()
                .type(d3.symbolCircle)
                .size(250)
            )
			.on('onchange', val => {
				if (toggle) render();
            });
		var gFill5 = d3
            .select('div#slider-fill5')
            .append('svg')
            .attr('width', Math.min(350,screen.width*0.7))
            .attr('height', 65)
            .append('g')
            .attr('transform', 'translate(30,30)')
			.attr('fill', '#aaaaaa');
		gFill5.call(sliderFill5);
	image.src = "/flower.jpeg";
	image.onload = function() {firstRender();}
		</script>
</html>

# Controls

* ▶️/⏸ Button: Controls time, causes noisiness to evolve over time and smoothens dither
* <html>
    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24"
        style="position:relative; top:2px;">
        <linearGradient id="Gradient2" x1="0" x2="0" y1="0" y2="1">
            <stop offset="0%" stop-color="#d60071" />
            <stop offset="20%" stop-color="#c04e00" />
            <stop offset="30%" stop-color="#917200" />
            <stop id='example' offset="45%" stop-color="#468a00" />
            <stop offset="50%" stop-color="#008a79" />
            <stop offset="60%" stop-color="#0083a8" />
            <stop offset="70%" stop-color="#4b5dff" />
            <stop offset="100%" stop-color="#b200df" />
        </linearGradient>
        <path fill="url(#Gradient2)"
            d="M12 0c-4.87 7.197-8 11.699-8 16.075 0 4.378 3.579 7.925 8 7.925s8-3.547 8-7.925c0-4.376-3.13-8.878-8-16.075zm.462 20.471c2.56-1.049 4.124-4.889 3.021-8.853 3.798 4.909.754 9.393-3.021 8.853z" /></svg>
            </html> Slider: Displayed hue is a random hue with "X% confidence" (i.e. the hue is chosen from 1-X% of the spectrum centered on the hue)
* <html>
    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" style = "position:relative; top:2px;"><path fill='gray' d="M22.266 14.724c-.064.898-.242 1.766-.522 2.586l1.601.675c-.163.461-.353.909-.568 1.344l-1.602-.676c-.383.751-.855 1.448-1.402 2.078l1.229 1.229c-.323.364-.667.709-1.032 1.032l-2.262-2.262c1.874-1.59 3.125-4.039 3.125-6.735 0-4.878-3.954-8.832-8.832-8.832s-8.833 3.954-8.833 8.832c0 2.505 1.107 5.024 3.126 6.735l-2.293 2.27-1.031-1.031 1.259-1.238c-.562-.647-1.046-1.365-1.434-2.139l-1.606.665c-.212-.436-.399-.885-.559-1.348l1.605-.664c-.268-.802-.439-1.646-.5-2.521h-1.735v-1.459h1.734c.063-.897.242-1.765.522-2.585l-1.6-.675c.163-.461.352-.91.567-1.344l1.602.675c.383-.75.855-1.448 1.403-2.078l-1.229-1.229c.323-.365.668-.709 1.032-1.031l1.229 1.229c.647-.562 1.366-1.045 2.141-1.433l-.665-1.606c.435-.212.885-.399 1.347-.558l.665 1.604c.802-.267 1.647-.438 2.522-.5v-1.734h1.459v1.733c.899.063 1.767.242 2.587.522l.675-1.6c.462.162.911.353 1.345.567l-.676 1.602c.751.383 1.449.854 2.08 1.402l1.229-1.229c.365.322.709.666 1.032 1.03l-1.229 1.229c.562.647 1.045 1.366 1.434 2.14l1.606-.665c.212.436.399.885.558 1.348l-1.604.664c.269.802.439 1.646.501 2.521h1.733v1.459h-1.734zm-3.434-.729c0 3.767-3.064 6.832-6.832 6.832s-6.833-3.065-6.833-6.832 3.065-6.832 6.833-6.832 6.832 3.065 6.832 6.832zm-7.36-1.81l-2.158-2.158-1.415 1.413 2.158 2.158 1.415-1.413z"/></svg>
            </html> Slider: Displays saturation is a random saturation with "X% confidence" (i.e. the saturation is chosen from roughly 1-X% of the spectrum centered on the saturation)
* <html>
    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24"
        style="position:relative; top:2px;">
        <path fill='gray'
            d="M14 19h-4c-.276 0-.5.224-.5.5s.224.5.5.5h4c.276 0 .5-.224.5-.5s-.224-.5-.5-.5zm0 2h-4c-.276 0-.5.224-.5.5s.224.5.5.5h4c.276 0 .5-.224.5-.5s-.224-.5-.5-.5zm.25 2h-4.5l1.188.782c.154.138.38.218.615.218h.895c.234 0 .461-.08.615-.218l1.187-.782zm3.75-13.799c0 3.569-3.214 5.983-3.214 8.799h-1.989c-.003-1.858.87-3.389 1.721-4.867.761-1.325 1.482-2.577 1.482-3.932 0-2.592-2.075-3.772-4.003-3.772-1.925 0-3.997 1.18-3.997 3.772 0 1.355.721 2.607 1.482 3.932.851 1.478 1.725 3.009 1.72 4.867h-1.988c0-2.816-3.214-5.23-3.214-8.799 0-3.723 2.998-5.772 5.997-5.772 3.001 0 6.003 2.051 6.003 5.772zm4-.691v1.372h-2.538c.02-.223.038-.448.038-.681 0-.237-.017-.464-.035-.69h2.535zm-10.648-6.553v-1.957h1.371v1.964c-.242-.022-.484-.035-.726-.035-.215 0-.43.01-.645.028zm-3.743 1.294l-1.04-1.94 1.208-.648 1.037 1.933c-.418.181-.822.401-1.205.655zm10.586 1.735l1.942-1.394.799 1.115-2.054 1.473c-.191-.43-.423-.827-.687-1.194zm-3.01-2.389l1.038-1.934 1.208.648-1.041 1.941c-.382-.254-.786-.473-1.205-.655zm-10.068 3.583l-2.054-1.472.799-1.115 1.942 1.393c-.264.366-.495.763-.687 1.194zm13.707 6.223l2.354.954-.514 1.271-2.425-.982c.21-.397.408-.812.585-1.243zm-13.108 1.155l-2.356 1.06-.562-1.251 2.34-1.052c.173.433.371.845.578 1.243zm-1.178-3.676h-2.538v-1.372h2.535c-.018.226-.035.454-.035.691 0 .233.018.458.038.681z" /></svg>
            </html> Slider: Displays luminance is a random luminance with "X% confidence" (i.e. the luminance is chosen from roughly 1-X% of the spectrum centered on the luminance)
* <html>
    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24"
        style="position:relative; top:2px;">
        <path fill='gray' d="M6 11v-4l-6 5 6 5v-4h12v4l6-5-6-5v4z" /></svg>
            </html> Slider: Determines the reduced width of the luminance spectrum, so 0% means 0% reduced, 50% means that luminance values are taken roughly between 0.25 and 0.75 (depending on the above luminance slider), and 100% meaning that all luminance values are 0.5 (depending on the above luminance slider)
* <html>
    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24"
        style="position:relative; top:2px;">
        <path fill='gray'
            d="M12 0l-11 6v12.131l11 5.869 11-5.869v-12.066l-11-6.065zm9 11.623l-3 1.569v-3.26l3-1.601v3.292zm-13-.654l3 1.625v3.186l-3-1.614v-3.197zm.9-1.799l2.986-1.603 3.132 1.688-3.014 1.608-3.104-1.693zm4.1 3.43l3-1.6v3.238l-3 1.569v-3.207zm4.138-4.475l-3.139-1.691 2.801-1.503 3.11 1.715-2.772 1.479zm-2.424-4.345l-2.825 1.517-2.728-1.47 2.834-1.546 2.719 1.499zm-7.649 1.19l2.711 1.46-2.973 1.596-2.67-1.456 2.932-1.6zm-1.065 4.908v3.204l-3-1.636v-3.216l3 1.648zm-3 3.843l3 1.636v3.185l-3-1.611v-3.21zm5 5.888v-3.169l3 1.614v3.146l-3-1.591zm5-1.545l3-1.569v3.104l-3 1.601v-3.136zm5 .468v-3.083l3-1.569v3.051l-3 1.601z" /></svg>
            </html> Slider: Changes the size of the resolution