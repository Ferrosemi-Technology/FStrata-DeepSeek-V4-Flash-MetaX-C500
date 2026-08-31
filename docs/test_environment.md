# 测试环境

## 平台身份

发布结果来自 Cv-MemoEngine DeepSeek-V4 Hot66/128K 交付配置。

| 组件 | 环境 |
| --- | --- |
| 引擎 | Cv-MemoEngine Hot66/128K 交付配置 |
| 模型 | DeepSeek-V4-Flash W8A8 |
| GPU 平台 | 2 × 沐曦 C500，单卡 64 GB 级显存 |
| 并行方式 | TP2 张量并行 |
| GPU 精度 | W8A8 INT8 权重路径 |
| CPU 专家精度 | KT AMXINT8 |
| CPU 专家运行时 | KTransformers CPUInfer |
| CPU | 2 × Intel Xeon Platinum 8468V |
| CPU 拓扑 | 2 个 NUMA 节点 |
| 主存 | 约 1 TB 级双 NUMA 服务器内存 |
| 操作系统 | Ubuntu 22.04.3 |
| Python | 3.10.10 |
| PyTorch | 2.8.0+metax3.7.1.17.dsv4 |
| SGLang | 0.5.12+maca3.7.1.110.dsv4 |
| KT kernel | 0.6.3.post1 |
| mcoplib | 0.4.7+maca3.7.1.13.dsv4.torch2.8 |
| MXMACA | 3.7.1.13-dsv4 |
| KMD / 驱动 | 3.9.6 |

## 运行配置

- Hot66/128K profile；
- 两个 GPU rank，每个 rank 使用本地 CPU 专家运行时；
- 两个 KT CPUInfer 线程池，分别映射到 NUMA 0 和 NUMA 1；
- GPU W8A8 专家与 CPU AMXINT8 专家组成混合 MoE 执行路径；
- SGLang serving 与 DeepSeek-V4 attention/runtime 集成；
- 对经过验证的稳定形状启用 CUDA/MACA 图执行；
- 交付配置启用 EAGLE 推测服务路径；
- 上下文容量配置为 132,000 tokens，可用 token 上限为 131,840。

## 硬件资源观测

| 资源 | 观测值 |
| --- | ---: |
| 每个 rank 的模型权重驻留 | 39.37 GB |
| 每个 rank 的图工作区 | 0.58 GB |
| 每个 rank 的可用运行余量 | 约 11.60 GB |
| 接近容量上限时的剩余余量 | 约 8.61 GB/GPU |
| GPU 峰值温度 | 46 °C |
| CPU 封装峰值温度 | 76 °C / 75 °C |
| 整机平均功耗 | 约 1,116.8 W |
| 整机峰值功耗 | 约 1,130 W |

正式负载期间未观察到持续热降频。

