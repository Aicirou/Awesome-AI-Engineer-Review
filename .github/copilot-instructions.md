# AI Coding Agent Instructions

## Repository Overview

This is a **knowledge repository** documenting AI/ML/Computer Science concepts and industry trends. It contains:
- Conference notes (NVIDIA GTC, Agentic AI Summit Berkeley)
- Technical paper summaries and insights
- LLM theory and application guides
- System design patterns for GenAI/ML systems
- Quantitative finance and trading notes
- Project implementations (LangChain, LangGraph, Cursor AI)

**Key distinction**: This is NOT a code repository with executable applications. It's a structured collection of markdown notes, learning materials, and technical documentation.

## Content Organization Pattern

### Directory Structure
```
/
├── AI LLM/              # LLM theory, RAG, agents, system design
├── Paper Reading/       # Latest research summaries (web agents, agentic AI)
├── System Design/       # GenAI/ML/Modern system design patterns
├── LangChain/          # LangGraph, Cursor AI project notes
├── DeepSeek/           # DeepSeek theory & implementation
├── NVIDIA GTC/         # Conference technical insights
├── Quant Job Interview Q&A/  # Cloud, quant, risk methodologies
└── [Domain-Specific]/  # Trading, finance, C++ patterns, etc.
```

### File Naming Convention
- Most files use descriptive names in English or Chinese
- Conference notes: `[Event Name] [Year].md`
- Theory notes: `[Topic] Theory.md`, `[Topic] Practice.md`
- Paper summaries: Direct paper/concept titles

## Content Style Guidelines

### Documentation Patterns

1. **Structured TOCs**: Every major document starts with auto-generated table of contents using `bitdowntoc`
2. **Bilingual Support**: Mix of English and Chinese content; Chinese technical terms often appear in English contexts
3. **Visual Documentation**: Heavy use of images stored via GitHub user attachments (`https://github.com/user-attachments/assets/...`)
4. **External References**: Links to books, courses, certificates, and external repos

### Technical Writing Style

- **Conference notes**: Bullet points, code snippets, architecture diagrams
- **Paper summaries**: Key takeaways, methodology breakdowns, comparative analysis
- **System design**: High-level architecture → detailed components → optimization strategies
- **Code examples**: Embedded in markdown with language-specific syntax highlighting

### Common Section Patterns
```markdown
# Contents (TOC)
# [Number]. Main Topic
## [Number].[Number] Subtopic
### [Concept/Method]
- Bullet points with implementation details
- Architecture diagrams
- Code snippets (Python/bash/SQL)
```

## Key Technical Domains

### 1. LLM & GenAI Systems
- **RAG architectures**: Vector DBs (FAISS, Pinecone), embedding strategies, reranking
- **Agent frameworks**: LangChain, LangGraph, CrewAI, multi-agent workflows
- **Optimization**: Quantization (INT4/GPTQ), LoRA/QLoRA, KV cache, batching
- **Deployment**: vLLM, Triton, BentoML, Ray Serve on Kubernetes

### 2. System Design Patterns
- **GenAI systems**: Prompt processing, vector embeddings, semantic caching, model hosting
- **ML pipelines**: Data preprocessing, model training, inference optimization
- **Distributed training**: AllReduce, Ring AllReduce, model parallelism
- **Monitoring**: Prometheus, Grafana, OpenTelemetry for latency tracking

### 3. Project Structure Examples
When documenting projects (see `LangChain/Projects.md`):
```
src/
├── [service]/
│   ├── tools/
│   ├── prompt/
│   ├── llm/
│   └── server.py
├── schemas/
├── database/
└── tests/
```

### 4. Cloud & Deployment
- **12-factor app principles**: Stateless services, config separation, port binding
- **Deployment patterns**: Blue-green, canary, rolling updates, A/B testing
- **IaC tools**: Docker, Kubernetes, Terraform references

## When Editing or Creating Content

### Adding New Notes
1. Place in appropriate domain folder
2. Start with TOC using `<!-- TOC start -->` markers
3. Include visual aids with GitHub attachment URLs
4. Add entry to main `README.md` with link and description
5. Use consistent header hierarchy (H1 for title, H2 for sections)

### Linking External Resources
- Books: `[Book Title](amazon-link) | [__Notes__](local-path)`
- Courses: `[Platform: Course Name](url) | [__Notes__](local-path)`
- Papers/Projects: Direct GitHub/arXiv links

### Code Documentation
```python
# Always include context comments
# Example from AI LLM/AI LLM Practice.md
def model_service():
    """Service layer with lazy loading pattern"""
    # FastAPI async pattern
    # Use ModelManager for lazy initialization
```

### System Design Diagrams
Document with:
- High-level architecture overview
- Component interactions (APIs, databases, queues)
- Data flow diagrams
- Optimization strategies with before/after comparisons

## Special Conventions

### Chinese Technical Terms
Preserve Chinese terminology when it appears in technical contexts:
- 大模型 (Large Language Model)
- 微调 (Fine-tuning)
- 检索增强生成 (RAG)

### Links and References
- External links use full URLs
- Internal links use relative paths
- Certificate/course completion links to Educative, Udemy, GeekBang

### Image Management
Images stored as GitHub attachments with format:
```markdown
<img src="https://github.com/user-attachments/assets/[hash]" width="X%" height="X%">
```

## Workflow Commands

This repository doesn't use traditional build/test commands. Common workflows:

```bash
# View structure
tree -L 2

# Search for topics
grep -r "RAG" --include="*.md"

# Update TOC (external tool)
# Visit: https://github.com/derlin/bitdowntoc
```

## Quality Standards

1. **Accuracy**: Technical content reflects current best practices
2. **Completeness**: Include context, examples, and visual aids
3. **Organization**: Logical grouping by domain, clear hierarchy
4. **Accessibility**: Both high-level overviews and deep dives
5. **Maintenance**: Update links, deprecate outdated patterns

## Repository Scope

**What this repo IS**:
- Curated learning materials and conference insights
- Technical pattern documentation with examples
- Paper reading summaries and key takeaways
- Implementation guidance and architecture patterns

**What this repo is NOT**:
- A production codebase with unit tests
- A library or package to install
- A project with CI/CD pipelines
- An executable application framework

Focus on enhancing documentation clarity, technical accuracy, and knowledge organization rather than code functionality.
