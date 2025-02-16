# image-generation
 
## Test 1

Test 1 was done using MinImagen following [this](https://www.assemblyai.com/blog/minimagen-build-your-own-imagen-text-to-image-model/) tutorial

### Issues
- Package dependencies
    - Setting up the environment was difficult and required changes with respect to the tutorial.

## Test-2

Test 2 was done using the huggingface.co code segment using the weights from the `google/ddpm-cat-256` library.

### Issues
- Time efficiency
    - Although simpler than MinImagen, even more of the process is obscured, and the end result took 2 hours to produce 1 image.
- Forgot to save
    - When it came time for image to be produced, a line of code to save the image was not included, and therefore, all effort lost.

These issues prove necessary for change in approach

## Test-3

Test 3 was attempted using [stable-diffusion-3.5](https://huggingface.co/stabilityai/stable-diffusion-3.5-large).

The overall idea would be to follow test 2, utilizing a library's image weights, but then properly optimizing for time efficiency.

### Issues
- GPU Usage
    - When attempting some of the suggested libraries for time efficiency, the GPU was unable to be located.

## Overall Issues

- Including files in repo
    - When attempting to bring the files into the main repo on a separate branch, the size of the dependencies caused errors.
    - In the process of making this repo, the code from the test-1 && test-3 folders was **wiped from existence.**
    - To remedy this for marks, I tried to recover the code that was once used.

- Blackbox
    - The pipeline of using pytorch to create images would take very long to properly understand, and even longer to explain.

## Learning Outcomes

1. Image generation is deeply layered when using online libraries
1. Image generation can be very costly when dealing with image weights

## What now?

After speaking with Bowen, our team has pivoted to making it's own original image generation algorithm with much simpler guidelines that can be easily understood.

**NOTE: This README.md was added to explain the work that was done during S2 W4. If this does not consistute a 6/6 for Code & Testing, then it would be greatly appreciated if you could clarify what you hope for at this stage in the project.**