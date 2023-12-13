### Reinforcement learning from human feedback (RLHF)

The good reference:
https://huyenchip.com/2023/05/02/rlhf.html

IPO
https://arxiv.org/abs/2310.12036

https://www.youtube.com/playlist?list=PLoROMvodv4rMFqRtEuo6SGjY4XbRIVRd4

https://github.com/LAION-AI/Open-Assistant


https://github.com/lucidrains/PaLM-rlhf-pytorch

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