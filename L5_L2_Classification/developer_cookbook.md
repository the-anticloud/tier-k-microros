# Developer Cookbook — K_MICROROS
**Stack:** C (on MCU), Python 3.11 (on agent/bridge), micro-ROS 2.x, FreeRTOS/Zephyr, AIOSS_FORMAT
**Domain:** micro-ROS: ROS2 on microcontrollers for Anticloud robot edge nodes

## micro-ROS agent bridge
```python
# On Ubuntu 22.04 with ROS2 Humble
from k_microros import MicroROSAgent

agent = MicroROSAgent(
    transport="serial",
    port="/dev/ttyUSB0",
    baud=115200,
    pax_model="./pax-27b-q4.gguf",
    aioss_chain="./microros.aioss"
)

agent.start()
# PAX now receives sensor data from MCU and sends commands back
for msg in agent.rx_stream():
    print(f"MCU topic: {msg.topic}, data: {msg.payload}")
    cmd = agent.pax_decide(msg)
    if cmd:
        agent.publish(cmd)
```

## AIOSS Chain Append
```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()
```
