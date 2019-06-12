## Understanding OpenCl

OpenCl, developed by The Khronos Group is designed to make potential usage of heterogenous computing model removing the limitation of vendor based proprietary programming languages. Providing leverage of cross vendor programming, OpenCl will be used in gprMax to accelerate the FDTD Solver by making use of GPUS, CPUs and their combination. 

OpenCl Architecture comprises Platform Model, Execution Model and Memory Model. 

#### Platform Model

This model defines relation between the host device and opencl supported devices. Host devices is any CPU with standard operation system whereas the OpenCl device can be any GPU, CPU, DSPs which supports OpenCl. 

OpenCl devices comprises of Computing Units which further contains Processing Elements. Depending upon the architecture of opencl devices, same OpenCl kernel can behave differntly on different architectures. 

#### Execution Model

OpenCl Execution model has two components ie, kernels and host programs. Kernels are basic unit of executable code that runs on or more opencl devices, which can be data- or task- parallel. The host program executes on host system, defines contexts, queues kernels execution instances using command queues. 

**gprMax** will be using PyOpenCl for writing host programs and C99 standard for device kernel code. PyOpenCl as a host is responsible for setting up and managing the execution of kernels on the OpenCl device through the use of context. Host program will query for resources like Devices (set of OpenCl devices), Program Objects (program source that implements a kernel), Kernels (OpenCl functions that execute on the device) and Memory Objects (set of memory buffers). 

Device kernel codes run on each work-item but on different data. Work-item are the most simplest element of execution. Work-item are clubbed into Work-groups. Depending upon device architecture dimensions and sizes of work group varies. Unlike CUDA codes where for global index for a thread is calculated using simple arithemetic, OpenCl kernel codes uses `get_global_id(dims)`. All work-items in the same work-group are executed together on the same device. The reason for executing on one device is to allow work-items to share local memory and synchronization. Global work-items are independent and cannot be synced. 

![Computing Architecture](../static/img/_posts/work_item_group.png)

#### Memory Model

OpenCl has four memory types depending on their site of usage and accessibility ie, Global, Constant, Local and Private memory. 

- Global memory is a memory region in which all work-items and work-groups have read and write access on both the compute device and host. This region of memory can be allocated only by the host during runtime. 

- Constant memory is a region of global memory that stays constant throughout the execution of the kernel. Work-items have only read access to this region. The host is permitted both read and write access. 

- Local memory is a region of memory used for data-sharing by work-items in a work-group. All work-items in the same work-group have both read and write access. 

- Private memory is a region that is accessible to only one work-item.

![Memory Model](../static/img/_posts/memory.png)

