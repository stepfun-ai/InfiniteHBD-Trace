# InfiniteHBD-Trace

> 🏆 This work has been **accepted to [ACM SIGCOMM 2025](https://conferences.sigcomm.org/sigcomm/2025/)**. It provides a real-world fault trace dataset for large-scale GPU clusters used in LLM pretraining workloads.

This project open-sources fault trace data from **400** GPU servers, with fault events impacting up to **231 distinct servers**. These servers are randomly picked across a GPU cluster comprising hundreds of nodes and thousands of GPUs. The dataset, covering **348 days of fault data starting from March 30, 2024**, is primarily used to support large-scale **pretraining of large language models (LLMs)** and provides realistic fault-tolerance testing data for simulation experiments. The dataset has been used in the *InfiniteHBD*, which includes detailed statistical analysis (Paper link: [https://arxiv.org/abs/2502.03885](https://arxiv.org/abs/2502.03885)).


## Project Description

This repository contains the following files:
- **fault_trace.json**: The fault trace dataset.
- **fault_statistics.json**: Hierarchical breakdown of fault types including Level, Class, and Description, with raw counts.
- **README.md**: Project documentation.

## Trace Description

The trace records a series of node fault events in the GPU cluster. Each event is stored in JSON format. An example is shown below:

```json
{
    "node_id": "067eb1e2-ea0b-4069-b64e-5df892642f88",
    "event_time": 8.6112,
    "event_type": "fault_start",
    "fault_type": {
        "Level": "Hardware Failure",
        "Class": "GPU",
        "Desc": "GPU xid Error"
    }
},
{
    "node_id": "067eb1e2-ea0b-4069-b64e-5df892642f88",
    "event_time": 8.8896,
    "event_type": "fault_end",
    "fault_type": {
        "Level": "Hardware Failure",
        "Class": "GPU",
        "Desc": "GPU xid Error"
    }
}
```

### Field Descriptions

- **node_id**  
  UUID as node identifier. The specific information about the nodes has been anonymized.

- **event_time**  
  The relative time of the event (unit: days). The time value is calculated relative to the first event in the trace. All events are sorted in ascending order by `event_time`.

- **event_type**  
  The type of event. There are only two possible types:
  - **fault_start**: Indicates a fault occurred, and the node became unavailable.
  - **fault_end**: Indicates the fault has ended, and the node was repaired and returned to normal operation.

- **fault_type**  
  A hierarchical description of the fault with the following fields:
  - **Level**: High-level category of the fault, e.g., *Hardware Failure* or *Software Failure*.
  - **Class**: A more specific fault type, such as *NIC*, *GPU*, etc.
  - **Desc**: The most fine-grained description of the fault, e.g., *NIC Lost*, *GPU DBE > Threshold*, etc.

## Fault Statistics (Percentage Overview)

> **Note**: All values are expressed as percentages of total faults. Full breakdown including subclasses and counts are shown in `fault_statistics.json`.

| Level            | Class                         | Percentage (%) |
| ---------------- | ----------------------------- | -------------- |
| Hardware Failure | GPU                           | 27.05          |
|                  | Parameter Plane Cable         | 6.85           |
|                  | NIC                           | 5.14           |
|                  | Power Supply                  | 4.45           |
|                  | Fan                           | 5.65           |
|                  | Motherboard Battery           | 0.17           |
|                  | Other Failures                | 0.17           |
|                  | Motherboard                   | 0.68           |
|                  | CPU                           | 0.51           |
|                  | RAID Card                     | 0.17           |
|                  | Riser Card                    | 0.17           |
|                  | **Subtotal**                  | **51.03**      |
| Software Failure | Other Failures                | 2.05           |
|                  | File System                   | 0.51           |
|                  | Software Tool                 | 1.20           |
|                  | Operating System              | 0.17           |
|                  | Firmware                      | 0.17           |
|                  | **Subtotal**                  | **4.11**       |
| Other Failure    | Unknown Error                 | 24.66          |
|                  | System Crash                  | 0.17           |
|                  | Stress Test Failure           | 16.61          |
|                  | Change                        | 0.68           |
|                  | Training Task Troubleshooting | 2.40           |
|                  | Test                          | 0.34           |
|                  | **Subtotal**                  | **44.86**      |


## Citation

If you use this dataset in your research, please cite the following paper:

```bibtex
@misc{shou2025infinitehbdbuildingdatacenterscalehighbandwidth,
      title={InfiniteHBD: Building Datacenter-Scale High-Bandwidth Domain for LLM with Optical Circuit Switching Transceivers}, 
      author={Chenchen Shou and Guyue Liu and Hao Nie and Huaiyu Meng and Yu Zhou and Yimin Jiang and Wenqing Lv and Yelong Xu and Yuanwei Lu and Zhang Chen and Yanbo Yu and Yichen Shen and Yibo Zhu and Daxin Jiang},
      year={2025},
      eprint={2502.03885},
      archivePrefix={arXiv},
      primaryClass={cs.NI},
      url={https://arxiv.org/abs/2502.03885}, 
}
```
