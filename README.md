# ADK Course - Agent Development Kit Patterns

A comprehensive course covering various agent implementation patterns using Google's Agent Development Kit (ADK).

## Project Setup

### System Requirements
- **Python:** 3.12 or higher
- **OS:** Windows, macOS, or Linux
- **Package Manager:** uv (recommended) or pip

### Python Dependencies

Install dependencies using one of these methods:

**Using uv (recommended):**
```bash
uv sync
```

**Using pip:**
```bash
pip install -r requirements.txt
```

**Or install manually:**
```bash
pip install google-adk>=1.12.0 litellm>=1.76.0 streamlit>=1.49.1 python-dotenv
```

**Key Packages:**
- **google-adk** (>=1.12.0) - Google Agent Development Kit for building agents
- **litellm** (>=1.76.0) - LLM interface supporting multiple model providers
- **streamlit** (>=1.49.1) - Web UI framework for interactive applications
- **python-dotenv** - Environment variable management for API keys

### Environment Variables Setup

Create a `.env` file in the project root to configure API credentials:

```bash
# Google API Keys
GOOGLE_API_KEY=your_google_api_key_here
GOOGLE_SEARCH_API_KEY=your_google_search_api_key_here
GOOGLE_SEARCH_ENGINE_ID=your_search_engine_id_here

# LiteLLM Configuration (for alternative model providers)
LITELLM_API_KEY=your_litellm_api_key_here

# Model Configuration (for non-Gemini models)
OPENAI_API_KEY=your_openai_api_key_here  # if using OpenAI
# For other providers, refer to LiteLLM documentation
```

**Getting API Keys:**
1. **Google API Key** - Get from [Google Cloud Console](https://console.cloud.google.com/)
   - Enable: Generative Language API, Custom Search API
2. **Gemini Model Access** - Available through Google AI Studio (free tier available)
3. **Search API** - Configure Custom Search with [Google Search Console](https://programmablesearchengine.google.com/)

### Virtual Environment Setup (Optional but Recommended)

```bash
# Create virtual environment
python -m venv .venv

# Activate (Windows)
.\.venv\Scripts\Activate.ps1

# Activate (macOS/Linux)
source .venv/bin/activate

# Install dependencies
uv sync
```

---

## Module Overview

### 1. **agent-callback/** — Agent Callback Hooks
**Real-World Use Case:** 📚 **Math Tutor Agent**
- A friendly AI tutor that helps students with math problems
- Demonstrates callback hooks for monitoring interactions and tracking performance

**Purpose:** Demonstrates how to use before/after callbacks to monitor and intercept agent interactions.

**Key Concepts:**
- `before_agent_callback()` - Triggered BEFORE agent processes user message
  - Use for: input validation, authentication, logging, request tracking
- `after_agent_callback()` - Triggered AFTER agent generates response
  - Use for: logging responses, analytics, performance metrics, response modification

**Example Use Cases:**
- Track request/response timing
- Validate user input before processing
- Log conversations for audit trails
- Implement rate limiting or permission checks

---

### 2. **model-callback/** — Model Request/Response Interceptor
**Real-World Use Case:** 🤖 **Filtered Chatbot**
- A content-aware chatbot that blocks certain topics (e.g., math questions)
- Demonstrates request/response filtering and response modification with emojis

**Purpose:** Shows how to intercept and filter/modify requests going TO and responses coming FROM the AI model.

**Key Concepts:**
- `before_model_callback()` - Intercepts request before sending to AI model
- `after_model_callback()` - Intercepts response from AI model
- Content filtering and validation at model interface level

**Example Use Cases:**
- Block inappropriate content before model receives it
- Content filtering (e.g., no math questions, no PII)
- Request logging and monitoring
- Custom response generation without calling the model
- Cost optimization through selective model calls

---

### 3. **multi-agents/** — Multi-Agent Orchestration
**Real-World Use Case:** ✈️ **Travel Planning System**
- **Destination Research Agent** - Researches locations, attractions, weather, culture, and safety
- **Itinerary Builder Agent** - Creates detailed day-by-day schedules with activities and dining
- **Travel Optimizer Agent** - Adds money-saving tips, packing lists, and backup plans

**Purpose:** Demonstrates multiple specialized agents working together using `SequentialAgent` for coordinated workflows.

**Key Components:**
- Three specialized agents working sequentially
- Each agent enhances output from previous agent
- Comprehensive travel planning from research → planning → optimization
- Real-time research using google_search tool

**Example Use Cases:**
- Complex travel planning workflows
- Multi-step business processes requiring expert coordination
- Content generation pipelines (research → create → optimize)
- System that requires information flow between agents

---

### 4. **parallel-agent/** — Parallel Agent Execution
**Real-World Use Case:** 📰 **Content Marketing Platform**
- **Blog Writer Agent** - Creates trend-aware blog content (runs in parallel)
- **SEO Specialist Agent** - Develops SEO strategies (runs in parallel)
- **Visual Creator Agent** - Designs visual content strategies (runs in parallel)
- All agents research trends simultaneously, then outputs are aggregated

**Purpose:** Executes multiple agents in parallel for performance optimization and concurrent task handling.

**Key Concepts:**
- `ParallelAgent` - Run multiple agents simultaneously
- Independent agents working concurrently on related tasks
- Aggregate results for complete content strategy
- 3x faster than sequential execution

**Example Use Cases:**
- Content creation across multiple dimensions (blog + SEO + visuals)
- Concurrent market research and analysis
- Parallel report generation from multiple sources
- Speed up complex workflows with independent analysis streams

---

### 5. **persistent_agent/** — Persistent State with Database
**Real-World Use Case:** 🍳 **Personal Recipe Assistant**
- Maintains a personal recipe collection in SQLite database
- Users can add, retrieve, and manage recipes across sessions
- Stores full recipe details (ingredients, instructions, cook time)

**Purpose:** Demonstrates persistent storage using `DatabaseSessionService` for maintaining data across sessions.

**Key Features:**
- SQLite database backend (`recipe_data.db`)
- Recipe management (add, retrieve, update recipes)
- Persistent conversation state across sessions
- Tool context integration with database

**Tools Included:**
- `add_recipe()` - Add new recipes to database with metadata
- Recipe retrieval and management functions
- Persistent storage survives application restarts

**Example Use Cases:**
- Personal digital assistants with memory
- Production database-backed agents
- Long-term user data retention and recall
- CRM systems with persistent customer data

---

### 6. **session-state-runner/** — Session State Management
**Real-World Use Case:** 💬 **Customer Support Agent**
- AI support agent for TechStore with customer profile tracking
- Maintains customer preferences (favorite category, loyalty points, order history)
- Provides personalized support based on customer state

**Purpose:** Demonstrates in-memory session management with `InMemorySessionService` for maintaining conversational state.

**Key Features:**
- `InMemorySessionService()` - Fast in-memory session backend for current conversation
- `CustomerStateOutput` - Pydantic schema for structured customer data
- `Runner` - Orchestration layer for agent execution and session management
- Multi-turn conversation with state tracking

**Components:**
- **session_object.py** - Session and state definitions
- **tester.py** - Test utilities and examples
- Output schema validation with Pydantic BaseModel
- Customer profile: name, favorite category, recent orders, loyalty points

**Example Use Cases:**
- Real-time customer support chatbots
- Multi-turn conversational agents
- Session-based interactions with state tracking
- Personalized agent responses based on user profile

---

## Running Examples

### Session State Runner (In-Memory)
```bash
# Run the customer support agent with session state
uv run session-state-runner/agent.py
```

### Persistent Agent (Database)
```bash
# Run the recipe assistant with persistent storage
uv run persistent_agent/agent.py
```

### Multi-Agents
```bash
# Run coordinated blog writer and SEO agents
uv run multi-agents/agent.py
```

---

## Key Concepts Summary

| Pattern | State Storage | Use Case | Execution |
|---------|--------------|----------|-----------|
| Agent Callback | Session | Monitoring & logging | Sequential |
| Model Callback | Session | Content filtering | Sequential |
| Multi-Agents | Session | Coordinated workflows | Sequential |
| Parallel Agent | Session | Concurrent tasks | Parallel |
| Persistent Agent | Database | Long-term memory | Sequential |
| Session State | Memory | Real-time conversation | Sequential |

---

## Learning Path

1. Start with **agent-callback** - Understand basic monitoring
2. Progress to **model-callback** - Learn request/response interception
3. Explore **session-state-runner** - Try in-memory session management
4. Study **persistent_agent** - Add database persistence
5. Advance to **multi-agents** - Coordinate multiple agents
6. Experiment with **parallel-agent** - Optimize with parallelization
 
 