
## 뉴로모픽 컴퓨팅 특징

- __집단화된 처리 및 메모리:__ 뇌에서 영감을 받은 뉴로모픽 컴퓨터 칩은 각각의 뉴런에 별도의 영역을 두는 대신 각 뉴런에서 데이터를 함께 처리하고 저장합니다. 처리와 메모리를 함께 배치함으로써 신경망 프로세서와 기타 뉴로모픽 프로세서는 폰 노이만 병목 현상을 피하고 고성능과 낮은 에너지 소비를 동시에 달성할 수 있습니다.

- __대규모 병렬 처리:__ 인텔 랩의 Loihi 2 와 같은 뉴로모픽 칩은 최대 100만 개의 뉴런을 가질 수 있습니다. 각 뉴런은 서로 다른 기능을 동시에 작동합니다. 이론적으로 뉴로모픽 컴퓨터는 뉴런 수만큼 많은 기능을 한 번에 수행할 수 있습니다. 이러한 유형의 병렬 기능은 뇌에서 뉴런이 무작위로 발화되는 것처럼 보이는 확률적 잡음을 모방합니다. 뉴로모픽 컴퓨터는 기존 컴퓨터보다 이 확률적 노이즈를 더 잘 처리하도록 설계되었습니다.

- __본질적으로 확장 가능:__ 뉴로모픽 컴퓨터에는 확장성을 가로막는 기존의 장애물이 없습니다. 더 큰 네트워크를 실행하기 위해 사용자는 뉴로모픽 칩을 더 추가하여 활성 뉴런의 수를 늘릴 수 있습니다.

- __이벤트 중심 계산:__ 개별 뉴런과 시냅스는 다른 뉴런의 스파이크에 반응하여 계산합니다. 즉, 실제로 스파이크를 처리하는 뉴런의 일부만 에너지를 사용하고 나머지 컴퓨터는 유휴 상태로 유지됩니다. 따라서 전력을 매우 효율적으로 사용할 수 있습니다.

- __높은 적응성과 가소성:__ 뉴로모픽 컴퓨터는 인간과 마찬가지로 외부 세계의 변화하는 자극에 유연하게 대응할 수 있도록 설계되었습니다. 스파이크 신경망(SNN) 아키텍처에서는 각 시냅스에 전압 출력이 할당되고 작업에 따라 이 출력을 조정합니다. SNN은 잠재적인 시냅스 지연과 뉴런의 전압 임계값에 따라 서로 다른 연결을 진화하도록 설계되었습니다.
연구자들은 가소성이 증가하면 뉴로모픽 컴퓨터가 학습하고 새로운 문제를 해결하며 새로운 환경에 빠르게 적응할 수 있을 것으로 기대하고 있습니다.

- __내결함성:__ 뉴로모픽 컴퓨터는 내결함성이 뛰어납니다. 인간의 뇌와 마찬가지로 정보가 여러 곳에 저장되므로 한 구성 요소에 장애가 발생해도 컴퓨터가 작동하는 데 지장이 없습니다.

## 뉴로모픽 컴퓨팅 활용 분야

- 무인 자동차
- 드론, 로봇
- 스마트 홈 기기
- 자연어, 음성 및 이미지 처리
- 데이터 분석
- 프로세스 최적화

---



# Neuromorphic fpga github
## Notable examples include:
TENNLab-UTK/fpga: This repository from the TENNLab Neuromorphic Computing group at the University of Tennessee focuses on FPGA-based neuromorphic elements, networks, processors, and associated tooling and software interfaces. It emphasizes the implementation of neuromorphic networks and processors on FPGAs for efficient hardware resource utilization and communication bandwidth, with options for "directive" or "stream" spike processing paradigms.

erictaur/Large-Scale-Reconfigurable-Neuromorphic-Architecture: This project details an FPGA-based design for a large-scale reconfigurable neuromorphic architecture.

open-neuromorphic/fpga-snntorch: This repository provides a tutorial and resources for training Spiking Neural Networks (SNNs) for hardware deployment on conventional GPUs and subsequent inference on embedded FPGAs like the AMD Kria KV260, utilizing High-Level Synthesis (HLS) with AMD Vitis HLS.

dgutierrezATC/TDE_vhdl: This repository presents a VHDL implementation of a Spike-based, Digital Time Difference Encoder (TDE) model for neuromorphic systems, designed to encode the relative timing between two events into a burst of spikes, with applications in areas like event-based optical flow estimation and sound source localization. 

ORNL/NeuroCoreX: This project from Oak Ridge National Laboratory offers an FPGA-based neuromorphic processor designed and implemented using VHDL.

These repositories represent a selection of the open-source efforts available on GitHub related to neuromorphic computing on FPGAs, 
covering topics from hardware architecture and design to software tools and specific neuromorphic models.

---

# Intel's Loihi neuromorphic research chips 
They are designed to accelerate bio-realistic neural network simulations by implementing brain-inspired principles directly in silicon, such as asynchronous spiking and event-driven communication. 
Researchers use the Loihi platform to implement and study models of biological neural systems, from insect brains to the mammalian olfactory bulb. 
## Key Aspects of Bio-Simulation on Loihi
- __Neuromorphic Hardware:__ Unlike conventional CPUs or GPUs, Loihi's architecture features many neuromorphic cores designed to emulate the dynamics of biological neurons and synapses with high energy efficiency. This asynchronous, event-driven approach mirrors how biological brains operate, with neurons only communicating when a "spike" event occurs.
- __Neuron Models:__ Loihi 2 offers greater programmability, allowing researchers to implement more complex and bio-realistic neuron models, such as the Izhikevich model, which can replicate a wide range of intricate neuronal behaviors beyond the simpler Leaky Integrate-and-Fire (LIF) model of its predecessor.
- __Targeted Biological Systems:__ Research efforts have successfully mapped and simulated specific biological systems on the Loihi hardware:
  - __Olfactory System:__ A bio-realistic network based on the mammalian olfactory bulb architecture was implemented to demonstrate online learning and spike timing for odor classification.
  - __Basal Ganglia:__ A spiking neural network (SNN) model of the basal ganglia, a brain region critical for decision-making and reward-based learning, has been implemented on Loihi 2 to perform tasks like the Go/No-Go task.
  - __Insect Brain Connectome:__ Researchers have demonstrated a "nontrivial, biologically realistic connectome" of the Drosophila melanogaster (fruit fly) brain on the Loihi 2 chip, a significant step in brain-scale simulations.
- __Software Frameworks:__ The open-source Lava software framework and other tools like NengoLoihi are used to program the chips and provide the necessary interface to map complex neural models onto the hardware. Emulators like Brian2Loihi also exist, allowing for software prototyping before deployment on the physical chip. 

By using neuromorphic chips like Loihi, researchers aim to overcome the power consumption and speed limitations of traditional general-purpose computers when running large-scale, detailed brain simulations, advancing the development of highly efficient, bio-inspired artificial intelligence systems. 

---

# IBM truenorth vs Intel loihi

IBM's TrueNorth and Intel's Loihi are prominent neuromorphic chips, with TrueNorth excelling at scaled, 
power-efficient pattern recognition and Loihi focusing on flexibility, on-chip learning, and more extensive inter-neuron connectivity. 

## Comparison of IBM TrueNorth and Intel Loihi

| Feature | IBM TrueNorth | Intel Loihi |
|---------|---------------|-------------|
| Release Year | 2014 | 2017 (Loihi), 2021 (Loihi 2) |
| Process Technology | 28 nm | 14 nm (Loihi) |
| Neurons | 1 million | ~131,072 (Loihi), 1 million (Loihi 2) |
| Synapses | 256 million | ~130 million (Loihi), 120 million (Loihi 2) |
| Learning Capability| Primarily for inference; limited on-chip learning | Strong emphasis on on-chip learning (e.g., spike-timing-dependent plasticity), adaptation |
| Connectivity | More limited inter-neuron connectivity | Supports larger-scale, more complex hierarchical connectivity patterns |
| Power Consumption | Extremely low (claimed ~70 milliwatts for real-time operation) | Very power efficient; up to 1,000 times better energy efficiency for certain workloads compared to conventional processors |
| Primary Use Case | Efficient pattern recognition and processing sensory data (e.g., vision, audition) | Real-time AI applications, robotics, and scenarios requiring continuous adaptation and decision-making |

## Key Differences

- __Focus on Learning:__ Loihi's architecture specifically supports on-chip learning algorithms, allowing it to adapt to new information in real-time. 
TrueNorth is more focused on highly energy-efficient, scaled pattern inference.
- __Connectivity:__ Researchers have found that Loihi supports more flexible and larger-scale inter-neuron connectivity compared to TrueNorth's more limited connectivity.
- __Neuron/Synapse Count:__ While TrueNorth initially had a higher neuron and synapse count than the original Loihi, Intel's second-generation Loihi 2 closed this gap, matching TrueNorth's neuron count and offering significant improvements in other areas.
- __Programmability:__ Loihi offers greater flexibility for researchers to implement custom neuron models and programming paradigms (via microcode), making it a versatile research platform. 

__Both chips__ demonstrate the potential of neuromorphic computing to offer substantial power savings compared to conventional processors by using event-driven, brain-inspired architectures. Neither are available for general consumer purchase; instead, they are primarily aimed at academic and industrial research and development. IBM has since developed a successor named NorthPole, which focuses on highly efficient AI inference for large models and is not strictly a neuromorphic system in the same vein as TrueNorth. 

# The Lava framework for Loihi, how to Simulate on CPU/GPU
The Lava framework is designed for cross-platform execution, allowing you to simulate the behavior of Spiking Neural Networks (SNNs) 
on conventional CPU and GPU architectures, as well as specialized neuromorphic hardware like Intel's Loihi chips. 

## How to Simulate on CPU/GPU
Lava uses a flexible process model approach where the same abstract SNN process can have different underlying implementations (ProcessModels) tailored to specific hardware backends. The default execution backend is the CPU, which uses Python-based models for easy development and testing before deployment to more specialized hardware. 

To run a simulation on a CPU or GPU:

1. efine your SNN as a Lava Process: You build your network using Lava's Python API, which involves defining Process objects that communicate via event-based message passing.
2. Specify the Execution Backend: Lava determines which ProcessModel to use based on the specified backend in the RunConfig. By default, it will use the Python-based models for CPU execution.
3. Run the Simulation: Once the network is defined, you can run it using the Lava runtime environment. 

The general workflow involves:

- __Prototyping on CPU:__ Most SNN algorithms can be developed and tested on a standard CPU using the Python-based PyLoihiProcessModel (despite the name, these models are designed for CPU execution). This allows for rapid iteration and debugging using familiar tools.
- __Leveraging the GPU (where available):__ While the CPU backend is standard, Lava is extensible to support custom implementations and other hardware backends. For GPU support, specific ProcessModels or integrations (e.g., via a low-level interface called Magma, which handles compilation for different backends) are used to take advantage of parallel processing capabilities.
- __Deployment:__ After testing on the CPU/GPU, the same high-level code can be compiled and deployed to neuromorphic hardware like Loihi 2 by changing the RunConfig. 

For detailed instructions and examples on building and running models, refer to the official Lava documentation and tutorials, which provide walk-throughs for creating and executing networks on the CPU. The framework handles the complexities of running the asynchronous, 
event-based computation on sequential or parallel hardware, abstracting the underlying differences for the user.  


# Chinese neuromorphic chips: tianjic vs speck

The primary difference is their core architectural philosophy: the Tianjic chip is a hybrid platform supporting 
both conventional artificial neural networks (ANNs) and spiking neural networks (SNNs), 
while the Speck chip is a dedicated, fully asynchronous event-driven neuromorphic processor. 

Here is a detailed comparison:

|Feature | Tianjic | Speck |
|--------|---------|-------|
| Architecture | Hybrid (supports both ANN and SNN models) | Fully asynchronous, event-driven SNN processor |
| Design Goal | To bridge the gap between traditional deep learning and biologically plausible SNNs, enabling versatile learning scenarios | To provide ultra-low power, low-latency processing for edge applications like mobile and IoT devices |
| Power Efficiency | Highly efficient, requiring significantly less energy than a GPU for tasks like image classification (e.g., 39x less for some CNNs) | Extremely power-efficient (milliwatt-level operation), often orders of magnitude more efficient than even other edge-computing hardware like the Jetson Nano |
| Latency | Efficient, though hybrid nature might introduce some overhead compared to pure SNN | Ultra-low latency, processing a single spike with microsecond-level latency (e.g., 3.36 μs) |
| Neuron Model Support | Supports a variety of coding schemes and models due to its hybrid nature | Primarily designed for specific SNN models, with some standard models like the Leaky Integrate-and-Fire (LIF) not fully supported in some earlier versions |
| Primary Use Cases | Versatile applications across various AI tasks, including image processing, natural language processing, and spatio-temporal pattern recognition | Best suited for edge computing with asynchronous sensors (like DVS cameras) where immediate, power-efficient processing of event streams is crucial, such as robotics vision or gesture recognition |

In summary, the Tianjic offers more versatility by accommodating both major neural network paradigms, while the Speck is a specialized, highly optimized solution for specific, low-power, event-driven tasks. 


# CHIP speck development environment

The SynSense Speck chip development environment uses dedicated Development Kits and an open-source software toolchain 
for building neuromorphic vision applications. 

## Hardware

- __Speck Development Kit:__ The primary hardware platform is the Speck Development Kit, powered by the Speck chip, which combines a dynamic vision sensor (DVS) and a neuromorphic processor (320,000 neurons) on a single system-on-chip (SoC).
- __Connectivity:__ The kit can interact with a host machine and includes an ultra-low-power Bluetooth controller chip for connectivity in some versions.
- __Vision Module:__ The kit typically includes an M12-mount or built-in lens, and the DVS sensor provides event-based input streams for real-time processing. 

## Software

The software environment for the Speck chip is open-source and based on Python libraries, 
designed to facilitate the training and deployment of spiking neural networks (SNNs). 

- __Sinabs:__ This is the primary open-source Python library for converting deep learning models from frameworks like PyTorch and Keras into equivalent SNNs that can run on the Speck chip.
- __Samna:__ This software tool is used for interacting with the Speck chip via the host machine, including chip configuration, data management, and simulation.
- __Rockpool:__ Another open-source Python library used in SynSense's broader product family, which aids in application development. 

## Development Process

The development process involves using the software toolchain to: 
1. Train conventional neural networks (CNNs) in standard frameworks (e.g., PyTorch).
2. Convert the trained models into SNNs using libraries like Sinabs.
3. Deploy and validate the SNN models onto the physical Speck chip using the development kit and Samna software. 

This environment allows engineers to leverage existing machine learning expertise to develop ultra-low-power, 
event-driven applications such as gesture recognition, object classification, 
and presence detection, all running on the edge with milliwatt power consumption. 

---

__The Speck chip development environment__, provided by the company SynSense, 
is a complete hardware and software solution designed for building event-driven, ultra-low-power neuromorphic vision applications. 

## Hardware

The core of the development environment is the Speck Development Kit, 
which features the Speck chip (a System-on-Chip) that integrates an event-based Dynamic Vision Sensor (DVS) 
and a neuromorphic processor with 320,000 neurons. 

Key hardware components and features:

- __Ultra-Low Power Consumption:__ The system operates with dynamic power consumption of less than five milliwatts.
- __Real-time Processing:__ It enables real-time processing for tasks like gesture recognition and object classification.
- __Sensor Integration:__ The DVS directly provides a stream of events to the on-chip processor, which reduces latency by eliminating the need for external data transfer.
- __Connectivity:__ The kit includes options for connecting to a host machine via USB and commonly used peripherals, sometimes including an ultra-low-power Bluetooth controller. 

## Software

SynSense provides an open-source software toolchain that allows engineers to train and deploy 
spiking convolutional neural networks (SCNNs) on the Speck chip. 

Key software tools:

- __Sinabs:__ An open-source Python library used to convert deep learning models from popular frameworks like PyTorch into equivalent SCNNs suitable for the Speck chip.
- __Samna:__ A software tool that acts as the primary interface for users to interact with and configure the Speck Dev Kit and manage the deployment pipeline.
- __Rockpool:__ Another open-source Python library that simplifies application development for SynSense's neuromorphic hardware, including the Xylo-Audio board. 

This integrated hardware and software approach provides a comprehensive environment for developing a wide range of neuromorphic applications, from robotics to industrial monitoring. 


