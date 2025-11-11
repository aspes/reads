
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


