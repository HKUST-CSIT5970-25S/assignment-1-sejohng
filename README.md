[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: JIANG Shijun
### Student Id: 21134775
### Email: sjiangaz@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > #### Name: Phoronix Test Suite (PTS)
    > #### Reason Reason for choosing: It's a comprehensive and widely recognized open-source benchmark tool that provides accurate and detailed performance data for both CPU and memory. It is also flexible enough to support multiple types of tests and configurations, making it ideal for benchmarking EC2 instances.
    > #### Configuration
    > Test Selection: To test CPU performance, you can choose a test like `pts/cpu`. To test memory, you can choose a test like `pts/memory` or a mixed load.  
    > Number of Iterations: Specify iterations in order to meet statistical significance, i.e. 3-5 iterations.  
    > Test Length: Depending on load, you can decide on each test duration. For CPU, 1-5 minutes can be suitable; you can select a duration as a function of the amount of memory (for instance, 2-10 minutes).
    > Test Specifics: You can also specify whether you will be carrying out the tests at different CPU speeds or at a given load.
    > #### Description of Measurement Findings
    > CPU Performance: The output will likely be in values in terms of either execution time or throughput. For instance, `pts/cpu` will present scores in terms of CPU performance in activities like integer operation, floating-point operation, etc.  
    > Memory Performance: The memory benchmark will provide you with information on bandwidth, latency, and read/write operation throughput. The memory benchmark will inform you about how efficiently a load is handled by EC2 in terms of its memory.
    > #### Example of results
    > CPU: The result can be in seconds as a measure of CPU time spent in computing a computational operation.  
Memory: The outcome might be in terms of memory throughput in GB/s, which represents how much data is transferred from and to memory in a second.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro`  |  3750 MIPS (Compression) ; 3123 MIPS (Decompression)  | Average : 10545.80 MB/s    |
    | `t2.medium` |  10087 MIPS (Compression) ; 5883 MIPS (Decompression)  |  Average : 19310.49 MB/s   |
    | `c5d.large` |  7658 MIPS (Compression) ; 4864 MIPS (Decompression)  |  Average: 14109.94 MB/s  |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.
  #### CPU Performance:
  > The `t2.micro` supports a comparatively modest level of CPU performance (3750 MIPS in compression, 3123 MIPS in decompression).
  > The `t2.medium` comes with a higher CPU performance (10087 MIPS compression, 5883 MIPS decompression), which is much more than that of `t2.micro` though with just a doubling in terms of vCPUs.
  > The `c5d.large` enjoys a modest CPU increase (7658 MIPS compression, 4864 MIPS decompression), with a more dramatic increase in vCPUs (2 in `t2.medium` up to 4 in `c5d.large`). Its CPU is slower than `t2.medium`, which is why more vCPUs do enhance performance, but which kind of instance (like `t2` versus `c5d`) as well as underlying architecture also play a role.
  #### Memory Performance:
  > Memory also behaves in a similar manner. The lowest memory performance is that of the t2.micro (10545.80 MB/s), followed by much higher memory throughput (19310.49 MB/s), which is practically a doubling in terms of its counterpart, the t2.micro. The c5d.large also gives a lower memory performance (14109.94 MB/s), although it is more in terms of both its vCPUs as well as its memory.
## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |  3000          | rtt min/avg/max/mdev = 0.330/0.374/0.539/0.060 ms   |
    | `m5.large` - `m5.large`   |  4790          | rtt min/avg/max/mdev = 0.187/0.200/0.221/0.010 ms   |
    | `c5n.large` - `c5n.large` |  4780          | rtt min/avg/max/mdev = 0.189/0.197/0.218/0.007 ms   |
    | `t3.medium` - `c5n.large` |  3600          | rtt min/avg/max/mdev = 0.783/0.815/0.887/0.031 ms   |
    | `m5.large` - `c5n.large`  |  4720          | rtt min/avg/max/mdev = 0.206/0.218/0.255/0.013 ms   |
    | `m5.large` - `t3.medium`  |  3740          | rtt min/avg/max/mdev = 0.841/1.016/2.273/0.419 ms   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

    This is a comparison between TCP bandwidth and RTT between different EC2 instance types in a shared location. Generally, connections between similar-type instances will have smaller RTT as well as more bandwidth.  
    In connections between a mix of different instance types, both TCP bandwidth as well as RTT will be more variable, with some having much higher RTT as well as smaller bandwidth. The implication is that network performance is not merely influenced by instance type, but also by which two instance types are communicating, with similar-type connections having overall better performance.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |      391       |  61.732  |
    | N. Virginia - N. Virginia |     4790       |   0.194  |
    | Oregon - Oregon           |     4790       |   0.160  |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
