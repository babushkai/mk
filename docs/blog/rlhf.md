### Reinforcement learning from human feedback (RLHF)

The good reference:
https://huyenchip.com/2023/05/02/rlhf.html

IPO
https://arxiv.org/abs/2310.12036

https://www.youtube.com/playlist?list=PLoROMvodv4rMFqRtEuo6SGjY4XbRIVRd4

https://github.com/LAION-AI/Open-Assistant


https://github.com/lucidrains/PaLM-rlhf-pytorch

<<<<<<< HEAD
https://github.com/CarperAI/trlx

### Inference

vLLM has far more throughput with batching, but that is also a WIP feature for llama.cpp.
Current standing is something like:

- vLLM is the fastest overall with batching, and has decent (but not SOTA) 4 bit quantization.

- Llama.cpp has the best hybrid CPU/GPU inference by far, has the most bells and whistles, has good and very flexible quantization, and is reasonably fast in CUDA without batching (but is getting batching soon). It has opencl and rocm backends, but support is focused on CUDA/Metal/CPU. Its the best backend for dGPUs that wont fit the whole model, and is otherwise a jack of all trades.

- MLC-LLM (with the TVM Vulkan backend) is the king of speed on IGPs, mobile devices and AMD/Intel dGPUs without having to fuss with a ROCM install. Its extremely fast on Nvidia dGPUS even without CUDA. It theoretically has "easy" support for webGPU and exotic hardware like FPGAs or AI blocks. But its 4-bit quantization was not as good as llama.cpp, last I checked.

- exLLAMAv2 has, by far, the best quantization for squeezing models onto small GPUs, and is the fastest CUDA (and ROCM?) backend with no batching. Its feature rich with a frontend like text-gen-ui

- Plain HF Transformers is... a fine default, but the master of none. The best use case is probably for testing research implementations.

=======
https://github.com/CarperAI/trlx 

Quantization with int int8-bit

```python
import torch
import torch.quantization

# Define a simple model
class SimpleModel(torch.nn.Module):
    def __init__(self):
        super(SimpleModel, self).__init__()
        self.fc = torch.nn.Linear(5, 2)

    def forward(self, x):
        x = self.fc(x)
        return x

# Create model instance and example input
model = SimpleModel()
example_input = torch.rand(1, 5)

# Quantize the model to int8
quantized_model = torch.quantization.quantize_dynamic(
    model, {torch.nn.Linear}, dtype=torch.qint8
```

https://huggingface.co/blog/hf-bitsandbytes-integration

Adafactor
https://paperswithcode.com/method/adafactor
>>>>>>> 8146939a302e75bc5ae167d0c4c63cbede27cb46
