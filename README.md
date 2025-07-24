# Self-Correcting-RAG-Agent
This repository contains the implementation of an advanced, agentic Retrieval-Augmented Generation (RAG) system that moves beyond simple pipelines by incorporating a dynamic, self-correcting workflow to ensure answer reliability and accuracy.

## Situation
Standard Retrieval-Augmented Generation (RAG) pipelines are powerful but brittle. They often fail silently when faced with common issues:
- Irrelevant Context: The vector store returns documents that are not relevant to the user's specific query, leading to "garbage-in, garbage-out."
- LLM Hallucination: The language model generates an answer that is not factually grounded in the provided context.
- Incomplete Answers: The generated answer is factually correct but fails to address the user's actual question.
- These failure points make standard RAG unsuitable for production systems where trust and reliability are critical.

## Objectives
The primary goal of this project was to design and build a more resilient and intelligent RAG agent that could overcome these limitations. The key objectives were to:
- **Create a Dynamic Pipeline:** Move from a static, linear chain to a dynamic graph that can loop, branch, and self-correct.
- **Ensure Contextual Relevance:** Implement a mechanism to programmatically grade the relevance of retrieved documents.
- **Eliminate Hallucinations:** Build a verification step to ensure the agent's answers are strictly grounded in the provided sources.
- **Automate Fallbacks:** Enable the agent to recognize when its internal knowledge is insufficient and dynamically use external tools (like web search) to find better information.

## Approaches
To achieve these objectives, a cyclical, stateful agent was built using LangGraph. This graph-based architecture allows for complex reasoning loops and conditional decision-making.

The agent operates in a multi-step, corrective loop:
<img width="4525" height="1227" alt="Drawing" src="https://github.com/user-attachments/assets/e245861d-df77-4bab-bcec-2f9b1deae729" />

- **Relevance Grading:** An LLM-based grader assesses each retrieved document. Irrelevant documents are discarded. If all documents are irrelevant, the agent triggers a web search.
- **Dynamic Tool Use:** The Tavily web search tool is used as a fallback to gather new, relevant context when the initial retrieval is poor.
- **Answer Verification:** After generation, two more LLM graders check the answer. The first verifies it is grounded in the sources (preventing hallucination), and the second confirms it addresses the user's question. If either check fails, the agent loops back to either re-generate or perform another web search.

## Impacts
This agentic approach results in a significantly more robust and trustworthy Q&A system.
- **Increased Reliability:** The multi-step verification process drastically reduces the likelihood of incorrect or hallucinatory answers.
- **Enhanced Adaptability:** The agent is not locked into a static knowledge base. Its ability to perform web searches allows it to answer questions about recent events and topics not covered in the initial documents.
- **Transparent Reasoning:** By using LangSmith tracing, the agent's decision-making process at each step is fully observable, making it easier to debug and trust.
- **Developer-Ready:** The project is structured as a Jupyter Notebook, allowing for easy experimentation and integration.
