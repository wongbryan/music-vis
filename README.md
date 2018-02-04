# Music Vis With Shaders

## Dev instructions

This will take you through how to start your part of the project.

1. Open up terminal and change directory (cd) into the folder you have this project saved in. If you type in 

ls

The output should be all the files in the current folder. It should include:

index.html
bower_components
README.md
assets

2. Make your own branch. Each person will be making their own scene. Thus, we're gonna be working on separate branches (look up Git branches to get an idea of what branches are and how they work). To make a branch, run:

git checkout -b your_name_here

Git "checkout" is a command that lets you look at other people's branches. The flag -b specifies that you're creating a new branch, and then checking it out. 

3. Set up a dev server. Run the command:

http-server

and you should now have a local server running on port 8080. Open Google Chrome and go to

localhost:8080

4. Now we can start working on the actual scene. Open up index.html in your favorite text editor (most of you probably have Sublime) and take a look around. Most of the code is stuff I set up for you and you don't have to worry about it. What we're going to be working with is this portion of code:

`<script type="fsh" id="fragmentShader">
    
    uniform float time;
    uniform float freqData[16];
    uniform float volume;

    void main(){

        vec3 color = vec3(volume, 0., 0.);
        float opacity = 1.;
        gl_FragColor = vec4(color, opacity);

    }

</script>`

This shader program determines what gets outputted onto your screen. Let's break it down. 

The goal of the shader is to tell your computer what color to make each pixel on your screen. This shader program is run for EVERY pixel on your screen and gives each pixel a final output color. How do we do this? Well...

gl_FragColor is a special global variable. You set gl_FragColor to the color you want the current pixel to be (remember we're running this for every pixel). This program is written in GLSL, which has some weird types. Here's what the types mean:

vec4 is a 4-dimensional vector. Basically, it's just a piece of information with 4 parts. A vec3 is a 3D vector, and a vec2 is two dimensional. You can use vectors to represent different types of information. For example, you can represent a point in 3D space with a vector:

(0, 0, 1)

This would represent the point at x=0, y=0, z=1. In order to create a vector in GLSL, you'd think it would be something like this:

`vec3 my_vector = (0, 0, 1)`

but you have to use vec3 as a constructor. So you do this:

`vec3 my_vector = vec3(0.0, 0.0, 1.0);`

NOTICE THAT I PUT A DECIMAL POINT AFTER EACH NUMBER! GLSL is strongly typed and basically all functions take a floating point value instead of an int. If you don't know what that means don't worry about it, just know that whenever you write a number, put a decimal after it or else you'll get a bunch of errors.

So what does gl_FragColor = vec4(...) do? We're creating a 4 dimensional vector that stores this information:

(red, green, blue, opacity)

Where (0., 0., 0., 1.) is black and (1., 1., 1., 1.) is white. These are just RGB values for the final output color we talked about earlier. 

I should also mention that vectors can take in other vectors in their constructors. You can do stuff like this:

`vec4 my_vec_4 = vec4(vec3(0., 0., 0.), 1.);`

and the value of my_vec_4 will be (0., 0., 0., 1.).

Finally, we should look at the variables at the top of the program. What are uniforms? Floats?

Your computer has a CPU and a GPU. When you write projects for CS32 or whatever and run/compile them, this all happens on your CPU. However, rendering graphic happens on your GPU. For our program, we declared variables "time," "freqData," and "volume" on the CPU. They exist in the CPU and the GPU has no way of accessing these variables. Thus we send these variables to the GPU and declare them at the top of our shader program. It might be confusing that you don't explicitly set their values at the top like this:

`uniform float time = 10.;`

But this is because we're assigning the values to the variables on the CPU, in Javascript. If you look at the rest of the code, you can see that I'm updating the values for each variable in the JS script. 

Anyway, variables that you pass from the CPU to the GPU are called uniforms. After declaring a variable as a uniform, you specify its type (just like in C++). You can do stuff like this:

`uniform int my_integer;
uniform vec3 my_vector3;
uniform int my_array_of_integers[16];`

So our uniforms are:

A floating point value for time, a floating point value for the current volume of the song (songs get louder and quieter as they progress) and an array of frequency data.

The frequency data is an array of 16 floating point values, ranging from 0 to 255. Each entry of the array represents how loud a certain range of sound frequency is in the song. I think(?) the first couple entries represent how loud high notes are, and the last few entries represent how loud the low notes are. So you can do stuff like this:

`float volume_of_high_pitched_sound = freqData[0];
float volume_of_bass = freqData[15];`

And use it however you want in the shader. KEEP IN MIND THAT THESE VALUES VARY FROM 0 TO 255, and RGBA values vary from 0 to 1. So you have to divide frequency values by 255 before using them as color, or whatever. 

So what do you do now? Try playing with the shader program and using different parts of the data to render different colors. Get used to writing GLSL code. You'll have a lot of errors at first, and you can view them by going to more tools -> developer tools in google chrome. It will tell you if there's an error with your code. Also, here's some useful GLSL reference:

https://thebookofshaders.com/
http://www.shaderific.com/glsl-functions/

These links tell you about different GLSL functions. The book of shaders is also a great tutorial on different techniques we'll be using, so I would read up on that too. Have fun!

