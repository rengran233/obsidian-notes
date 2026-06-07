1. 是否贴近真实高价值业务场景；
  2. 是否能展示复杂 LLM workflow；
  3. 是否有评估体系；
  4. 是否能落到产品闭环；
  5. 是否能体现工程化能力；
  6. 是否容易公开展示且不涉及版权 / 隐私；
  7. 是否能讲出指标提升。


• 绑定 CLI：换模型/换 SDK 必须改流水线；
  • 难做并发控制：每次都起子进程；
  • 难做 token 计量、成本统计；
  • 难做 retry/timeout/rate-limit；
  • 难做结构化输出：CLI 输出文本，结构化解析麻烦；
  • 难做单元测试：必须真调 CLI 或写 fake binary；
  • 难支持流式输出：CLI 的 stdout 不是稳定 token 流；
  • 难做 LLM Judge / Verifier 这类纯文本短任务：起一个子进程开销过高。



  • 修正现有 MNN 加载与校验；
  • 修 JNI 稳定性；
  • 增强模型目录检查；
  • 增加模型市场页；
  • 增加 ModelScope 下载器；
  • 增加下载任务、进度、断点续传、校验；
  • 增加本地模型 registry；
  • 增加已下载模型选择 / 删除 / 切换；
  • 给模型打 modality 标签；
  • 保留文本化观察；
  • 增加本地 OCR；
  • 预留并实现本地 VLM；
  • Executor / Reflector 支持视觉不足 fallback；
  • 增加 mmap、参数配置、健康检查、session pool；
  • 补齐测试和真机性能验证。

  ## 1. 本地 MNN 稳定可运行

  下载了模型必须能加载、能推理、失败有明确错误。

  ## 2. 模型市场 MVP

  不要让用户手动找模型，只给推荐模型列表。

  ## 3. ModelScope 白名单下载

  只支持已验证可运行的模型，一键下载、进度、校验。

  ## 4. 已下载模型管理 + 一键分配角色

  下载后能看到、能删除、能选择给 Manager / Executor / Reflector / Notetaker。

  ## 5. 明确文本化限制和视觉 fallback

  文本模型能用于基础任务，但 Executor / Reflector 不能假装自己会看图。