# EON Capability Roadmap

Capabilities to add to the autonomous engine, each mapped to open-source
reference repos found in an Oct 2026 GitHub survey (100 repos, live star counts
in `ai_repos_100.csv`). Stars are approximate. Treat each repo as a design
reference first and a dependency second; check its license before reuse.

## 1. Durable orchestration (foundation)
**Goal:** long-running workflows that checkpoint, resume, and replay.
- Reference: langchain-ai/langgraph (graph state, checkpoints, time-travel), google/adk-python, strands-agents, pydantic/pydantic-ai, camel-ai/camel, kyegomez/swarms, deepset-ai/haystack
- Build: workflow graph runner with persisted state per step; resume after restart; run history already exists, extend it with step-level checkpoints.
- Done when: a killed run resumes from its last completed step.

## 2. Tiered, temporal memory
**Goal:** the engine remembers users, tasks, and facts over time.
- Reference: mem0ai/mem0 (scoped user/agent/run memory), topoteretes/cognee (graph pipeline), Letta/MemGPT (self-editing core/recall/archival tiers), Zep Graphiti (facts with validity time), supermemoryai/supermemory, volcengine/OpenViking, MemTensor/MemOS
- Build: memory module with add/search/update/delete; scopes (user, agent, run); timestamped facts; a background consolidation job.
- Done when: a fact stated in one session is retrieved in a later one, and superseded facts rank below current ones.

## 3. Policy, approval gates and sandboxing
**Goal:** extend the existing Safety page into enforceable runtime policy.
- Reference: openai-agents handoff/approval pattern, huggingface/smolagents (sandboxed code actions), arcboxlabs/arcbox and e2b-dev/open-computer-use (isolated machines), NVIDIA/garak, promptfoo/promptfoo (red teaming)
- Build: per-tool permission policy, approval queue for consequential actions, sandboxed code execution, red-team test suite in CI.

## 4. Interoperability: MCP and A2A
**Goal:** plug in tools and other agents without custom glue.
- Reference: a2aproject/A2A, MCP servers (topic: mcp-server), microsoft agent framework
- Build: MCP client in the tool layer; optional A2A endpoint so external agents can call EON workflows.

## 5. Computer-use and browser agents
**Goal:** act on websites and desktop apps, not just APIs.
- Reference: browser-use/browser-use, simular-ai/Agent-S, trycua/cua, microsoft/magentic-ui, microsoft/fara, bytedance/UI-TARS-desktop, web-infra-dev/midscene
- Build: browser tool with screenshot + DOM observation, action log feeding the audit log, approval gate on irreversible actions.

## 6. Multimodal and real-time I/O
**Goal:** replace local canvas "studio" and Web Speech with real models when keys or GPUs are available.
- Reference: vllm-project/vllm-omni (omni serving), OpenBMB/MiniCPM-V and MiniCPM-o (live audio-vision), OpenGVLab/InternVL, QwenLM Qwen-VL, deepseek-ai/Janus (understand + generate), RVC-Boss/GPT-SoVITS (voice), TEN-framework/ten-framework (real-time voice agents), calesthio/OpenMontage (agentic video pipeline)
- Build: provider slots already exist; add a vision-input path to chat and a full-duplex voice mode behind the existing voice/text toggle.

## 7. Retrieval and knowledge graphs
**Goal:** upgrade the Research pipeline.
- Reference: HKUDS/LightRAG, microsoft/graphrag, run-llama/llama_index, infiniflow/ragflow
- Build: graph-aware retrieval and citation tracking on top of search, fetch, validate, summarize.

## 8. Evaluation and observability
**Goal:** measure agent quality and cost.
- Reference: langfuse/langfuse, Arize-ai/phoenix, comet-ml/opik, confident-ai/deepeval, mlflow/mlflow, open-compass/VLMEvalKit
- Build: trace every workflow step, regression evals in CI, cost circuit breakers (see entropy-zero).

## Suggested order
1. Checkpointed orchestration  2. Memory  3. Policy and sandbox  4. MCP/A2A  5. Eval and tracing  6. Browser agent  7. Retrieval upgrade  8. Multimodal and voice
