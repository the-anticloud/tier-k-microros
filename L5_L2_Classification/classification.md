# L5 Narrow / L2 General Classification — K_MICROROS
**Platform:** Anticloud | **Tier:** TIER_9_ROBOTICS_IOT | **PAX:** 27B
**IP:** USPTO pending 2026, Anticloud FZ LLE, 0-1.gg | **License:** Apache-2.0

## L5 Narrow
K_MICROROS integrates micro-ROS for Anticloud robot edge nodes: publishing sensor data and receiving control commands via ROS2 DDS from full Anticloud nodes. Narrow scope: Anticloud MCU-based robot controllers.

## L2 General
L2 General: K_MICROROS connects the deepest robotic edges (MCU motor controllers, sensor heads) to the Anticloud ROS2 graph. L_ROS2CORE and L_MOVEIT2 both receive data from K_MICROROS edge nodes.

## PAX 27B Integration
PAX 27B on the full ROS2 node interprets micro-ROS sensor data and publishes control commands back to K_MICROROS nodes, closing the AI-in-the-loop robot control cycle.

## AIOSS Audit Chain
Every micro-ROS message (topic hash + message hash + timestamp + DDS participant hash) is chained: H_n = SHA3-256(H_{n-1} || entry_hash_n || timestamp_n).
Offline-verifiable, tamper-evident, zero cloud dependency.

## Regulatory / Compliance
ISO 13482 (robot safety). IEC 62443-4-2 (IoT). ROS2 REP standards.
