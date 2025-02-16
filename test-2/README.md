[Model and Scheduler](https://huggingface.co/docs/diffusers/en/using-diffusers/write_own_pipeline) are integral to understanding how to generate images.

1. Load the model and scheduler:

```
from diffusers import DDPMScheduler, UNet2DModel

scheduler = DDPMScheduler.from_pretrained("google/ddpm-cat-256")
model = UNet2DModel.from_pretrained("google/ddpm-cat-256", use_safetensors=True).to("cuda")
```

2. Set the number of timesteps to run the denoising process for:
    - NOTE: the number of timesteps represents the stages between an actual image and noise.

```
scheduler.set_timesteps(50)
```

3. Setting the scheduler timesteps creates a tensor with evenly spaced elements in it, 50 in this example. Each element corresponds to a timestep at which the model denoises an image. When you create the denoising loop later, you’ll iterate over this tensor to denoise an image:

    - NOTE: 1000 represents pure noise while 0 represents the image itself. (I think)
```
scheduler.timesteps
tensor([980, 960, 940, 920, 900, 880, 860, 840, 820, 800, 780, 760, 740, 720,
    700, 680, 660, 640, 620, 600, 580, 560, 540, 520, 500, 480, 460, 440,
    420, 400, 380, 360, 340, 320, 300, 280, 260, 240, 220, 200, 180, 160,
    140, 120, 100,  80,  60,  40,  20,   0])
```

4. Create some random noise with the same shape as the desired output:

    - NOTE: shape simply refers to the size of the output, e.g. 256x256
```
import torch

sample_size = model.config.sample_size
noise = torch.randn((1, 3, sample_size, sample_size), device="cuda")
```

5. Now write a loop to iterate over the timesteps. At each timestep, the model does a UNet2DModel.forward() pass and returns the noisy residual. The scheduler’s step() method takes the noisy residual, timestep, and input and it predicts the image at the previous timestep. This output becomes the next input to the model in the denoising loop, and it’ll repeat until it reaches the end of the timesteps array.

    - NOTE: using current inputs (noisy residual, timestep, and input) it uses the model to predict the previous timestep.

```
input = noise

for t in scheduler.timesteps:
    with torch.no_grad():
        noisy_residual = model(input, t).sample
    previous_noisy_sample = scheduler.step(noisy_residual, t, input).prev_sample
    input = previous_noisy_sample
```

6. The last step is to convert the denoised output into an image:

```
from PIL import Image
import numpy as np

image = (input / 2 + 0.5).clamp(0, 1).squeeze()
image = (image.permute(1, 2, 0) * 255).round().to(torch.uint8).cpu().numpy()
image = Image.fromarray(image)
image
```

latent diffusion model = lower dimension representation of image (scaled down) for memory efficiency.

encoder -> compresses image

decoder -> convert compressed representation back to image

