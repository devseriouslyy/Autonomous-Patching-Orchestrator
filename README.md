
# Autonomous Patching Orchestrator (APO)
### Agentic AI + Ansible for Enterprise Linux Fleet Management
## 📌 Project Overview
**APO** is an Intelligent Infrastructure Agent designed to transform traditional "toil-heavy" OS patching into a self-reasoning, autonomous workflow. While standard automation (Ansible) executes tasks, APO provides the **cognitive layer**: analyzing system health, making "Go/No-Go" decisions, and performing deep semantic analysis of post-patch logs.
This project was developed to solve the real-world complexity of managing large-scale Linux environments, reducing manual intervention in the patching lifecycle.
## 🏗️ Architecture
The system utilizes a "Brain and Hands" approach:
 * **The Brain (Agentic Layer):** Built with **LangGraph/CrewAI**, utilizing LLMs to reason through pre-checks and log anomalies.
 * **The Hands (Execution Layer):** **Ansible** playbooks triggered via ansible-runner to ensure idempotent state management.
## 🚀 Key Features
### 1. Context-Aware Pre-Checks
Unlike static scripts, the Agent evaluates the **server's role** before patching:
 * Identifies critical services (e.g., Database, Web Server).
 * Checks for active user sessions or high-load conditions.
 * **Decision Logic:** If a primary DB node is active, the agent will attempt to failover or delay patching based on the cluster state.
### 2. Semantic Log Reasoning
Post-patching, the Agent parses /var/log/dnf.log (or yum.log) and journalctl:
 * Detects "silent" failures or deprecated library warnings that standard exit codes might miss.
 * Generates a human-readable "Executive Summary" of the patching health.
### 3. Automated Self-Healing & Rollback
If a service fails to start post-reboot:
 * The Agent analyzes the systemd error log.
 * Attempts a restart or configuration fix.
 * If unresolved, it automatically triggers an Ansible rollback playbook to restore the previous stable state.
## 🛠️ Tech Stack
 * **Orchestration:** Python 3.11+, LangGraph
 * **LLM:** Qwen2.5-7B-Instruct 
 * **Automation:** Ansible Core
 * **Infrastructure:** Azure (Linux VMs), RHEL/Ubuntu
 * **Infrastructure detailed** 2 Azure VMs -
 * VM1 (Ubuntu 22.04, Ollama installed, Qwen2.5-7B-Instruct, Port 11434 open to VM2 only (NSG rule in Azure)) 
 * VM2 (Ubuntu 22.04
Python 3.11+ with requests, langchain (optional), pyyaml
Ansible installed (pip install ansible or apt)
SSH key pair configured for all target VMs
Your APO agent code — a Python script/service that orchestrates the workflow
Ansible playbooks: pre_patch_checks.yml, patch.yml, post_patch_validate.yml, rollback.yml
An Ansible inventory file listing your target hosts
## 📖 How it Works
 1. **Discovery:** Agent queries the inventory to identify pending patches.
 2. **Reasoning:** Agent executes a "Pre-Check" tool (Ansible) and evaluates the JSON output.
 3. **Action:** If health is green, Agent calls the patch_execution tool.
 4. **Verification:** Agent reads post-patch logs and system status.
 5. **Reporting:** Agent sends a summary via Slack/Email/Jira.
## 📈 Business Impact
 * **Reliability:** Eliminated human error in post-patch log verification.
 * **Scalability:** Allows a single engineer to oversee patching for 100+ hosts simultaneously.
 * **Compliance:** Generates an AI-summarized audit trail for every patching cycle.
