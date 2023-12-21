# Compute Helper

This Godot 4 plugin adds in a ComputeHelper class that keeps track of compute shaders and their uniforms.
Here's a simple example of a shader that reads and then writes to a texture (ideally in the render thread):
```gdscript
ComputeHelper.fmt.width = image_size.x
ComputeHelper.fmt.height = image_size.y
ComputeHelper.fmt.format = RenderingDevice.DATA_FORMAT_R32G32B32A32_SFLOAT
ComputeHelper.fmt.usage_bits = RenderingDevice.TEXTURE_USAGE_SAMPLING_BIT + RenderingDevice.TEXTURE_USAGE_STORAGE_BIT + RenderingDevice.TEXTURE_USAGE_CAN_UPDATE_BIT

var image := Image.create(image_size.x, image_size.y, false, Image.FORMAT_RGBAF)
image.fill(Color.BLACK)

var compute_shader := ComputeHelper.create("res://compute-shader.glsl")
var input_texture := ImageUniform.create(image)
var output_texture := SharedImageUniform.create(input_texture)
compute_shader.add_uniform_array([input_texture, output_texture])

var work_groups := Vector3i(image_size.x, image_size.y, 1)
compute_shader.run(work_groups)
ComputeHelper.sync()

image = output_texture.get_image()
```
For more information on compute shaders in Godot 4, here are some useful resources:
- [Official Compute Texture Demo Project](https://github.com/godotengine/godot-demo-projects/tree/master/compute/texture)
- ["Godot 4 - Conway's Game Of Life (Full Lesson)" Video Tutorial (Uses C#)](https://www.youtube.com/watch?v=VQhi2w1E0iU)
- ["Everything About Textures in Compute Shaders!" Article](https://nekotoarts.github.io/teaching/compute-shader-textures)
- ["How to use Compute Shaders in Godot 4" Video](https://www.youtube.com/watch?v=5CKvGYqagyI)

And while you're here, here's some similar plugins you might want to look at:
- [Godot-GPU-Computer](https://github.com/PGComai/Godot-GPU-Computer)
- [Compute Shader Studio](https://github.com/pascal-ballet/ComputeShaderStudio)
