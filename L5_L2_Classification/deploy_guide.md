# Deploy Guide — K_MICROROS
**Tier:** TIER_9_ROBOTICS_IOT | **Stack:** C (on MCU), Python 3.11 (on agent/bridge), micro-ROS 2.x, FreeRTOS/Zephyr, AIOSS_FORMAT
**Air-gap capable after initial setup.**

## Prerequisites
MCU: STM32, ESP32 (with micro-ROS firmware). Agent bridge: Ubuntu 22.04 + ROS2 Humble. 4GB RAM.

## Environment
MCU with micro-ROS firmware. micro-ROS agent on Ubuntu 22.04. ROS2 Humble. USB or serial bridge.

## AIOSS Integration
```bash
aioss init --module K_MICROROS --output ./k_microros.aioss
aioss append --chain ./k_microros.aioss --payload ./output.bin --module K_MICROROS
aioss verify --chain ./k_microros.aioss
```

## Air-Gap Setup
```bash
pip download -r requirements.txt -d ./wheels/
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="K_MICROROS",
    aioss_chain="./K_MICROROS.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./K_MICROROS.aioss --verbose
python -m K_MICROROS.tests.smoke
```
