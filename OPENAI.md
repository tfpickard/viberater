# OpenAI Integration Guide

This guide explains how to use OpenAI's models (including GPT-4, GPT-3.5, and Codex) as agents within the viberater multi-agent development system.

## Overview

OpenAI provides powerful language models that can assist with various development tasks, from code generation to architectural planning. This guide shows how to leverage these models for different agent roles defined in AGENTS.md.

## Prerequisites

- OpenAI API account and API key
- Familiarity with the viberater project architecture
- Understanding of the agent roles defined in AGENTS.md

## Setting Up OpenAI as an Agent

### 1. API Access

**Installation**:

```bash
# Install the OpenAI Python SDK
pip install openai

# Set your API key
export OPENAI_API_KEY='your-api-key-here'
```

**Python Example**:
```python
from openai import OpenAI

client = OpenAI(
    api_key="your-api-key-here"
)

response = client.chat.completions.create(
    model="gpt-4-turbo-preview",
    messages=[
        {"role": "system", "content": "You are a helpful assistant for the viberater project."},
        {"role": "user", "content": "Design a weather API caching strategy"}
    ],
    temperature=0.7,
    max_tokens=2000
)

print(response.choices[0].message.content)
```

**Node.js Example**:
```javascript
import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY
});

async function main() {
  const completion = await openai.chat.completions.create({
    model: "gpt-4-turbo-preview",
    messages: [
      {"role": "system", "content": "You are a helpful assistant for the viberater project."},
      {"role": "user", "content": "Design a weather API caching strategy"}
    ]
  });

  console.log(completion.choices[0].message.content);
}

main();
```

### 2. Model Selection

Choose the right model for your task:

- **GPT-4 Turbo**: Best for complex reasoning, architecture, and planning
- **GPT-4**: Excellent for detailed analysis and code review
- **GPT-3.5 Turbo**: Fast and cost-effective for simpler tasks
- **GPT-4 Code Interpreter**: Best for data analysis and code execution
- **DALL-E 3**: For generating UI mockups and visual assets

## Using OpenAI for Different Agent Roles

### Planner Agent

**System Prompt**:
```
You are the Planner Agent for viberater, a weather-mood correlation tracking application. 
Your role is to create detailed project plans, break down features into tasks, estimate effort, 
and identify dependencies and risks.
```

**Example Request**:
```python
response = client.chat.completions.create(
    model="gpt-4-turbo-preview",
    messages=[
        {"role": "system", "content": "You are the Planner Agent for viberater..."},
        {"role": "user", "content": """
            Create a 2-week sprint plan for implementing a user authentication system.
            Current state: No authentication exists.
            Requirements: Email/password login, JWT tokens, password reset.
            Team: 1 backend dev, 1 frontend dev, 1 tester.
        """}
    ]
)
```

**Expected Output**:
- Task breakdown with time estimates
- Dependency graph
- Risk assessment
- Daily sprint goals

---

### Architect Agent

**System Prompt**:
```
You are the Architect Agent for viberater. You make high-level technical decisions,
design system architecture, define data flows, and ensure scalability and maintainability.
Focus on best practices and industry standards.
```

**Example Use Cases**:

```python
# Architecture Decision
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are the Architect Agent for viberater..."},
        {"role": "user", "content": """
            Should we use REST or GraphQL for viberater's API?
            
            Considerations:
            - Need to fetch weather data and mood ratings
            - Mobile and web clients
            - Real-time updates would be nice but not critical
            - Team has more REST experience
            
            Provide recommendation with detailed justification.
        """}
    ]
)
```

**Advanced: Function Calling for Architecture Diagrams**:

```python
functions = [
    {
        "name": "create_architecture_diagram",
        "description": "Creates a Mermaid diagram for system architecture",
        "parameters": {
            "type": "object",
            "properties": {
                "diagram_type": {
                    "type": "string",
                    "enum": ["sequence", "flowchart", "class", "entity-relationship"]
                },
                "components": {
                    "type": "array",
                    "items": {"type": "string"}
                }
            }
        }
    }
]

response = client.chat.completions.create(
    model="gpt-4-turbo-preview",
    messages=[{"role": "user", "content": "Create an architecture diagram for viberater's weather data flow"}],
    functions=functions,
    function_call="auto"
)
```

---

### Product Designer Agent

**System Prompt**:
```
You are the Product Designer Agent for viberater. You focus on user experience,
interface design, accessibility, and ensuring the product delights users while
solving their need to understand how weather affects their mood.
```

**Example Request**:
```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are the Product Designer Agent for viberater..."},
        {"role": "user", "content": """
            Design the mood entry screen for viberater mobile app.
            
            Requirements:
            - Quick to use (< 10 seconds to log mood)
            - Capture mood on a scale
            - Optional text note
            - Show current weather automatically
            - Accessible to users with motor impairments
            
            Provide: User flow, interaction design, accessibility features
        """}
    ]
)
```

**DALL-E Integration for Mockups**:

```python
# Generate UI mockup images
image_response = client.images.generate(
    model="dall-e-3",
    prompt="""
        Modern mobile app interface for mood tracking. 
        Clean design with a mood slider from 1-10, 
        weather icon and temperature display at top,
        optional text input field, and a prominent save button.
        Use calming colors, minimalist style.
    """,
    size="1024x1024",
    quality="standard",
    n=1
)

image_url = image_response.data[0].url
```

---

### Backend Developer Agent

**System Prompt**:
```
You are the Backend Developer Agent for viberater. You write server-side code,
design APIs, integrate third-party services, handle data processing, and ensure
security and performance. Provide production-ready code with error handling.
```

**Code Generation Example**:

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are the Backend Developer Agent for viberater..."},
        {"role": "user", "content": """
            Implement a Node.js Express endpoint that:
            1. Fetches weather data from OpenWeatherMap API
            2. Caches the response for 30 minutes
            3. Handles errors gracefully
            4. Returns standardized weather data
            
            Use async/await, include input validation, and add JSDoc comments.
        """}
    ],
    temperature=0.3  # Lower temperature for more consistent code
)
```

**Codex (Legacy) for Code Completion**:

While Codex has been superseded by GPT-4 and GPT-3.5, you can still use similar patterns:

```python
# For inline code completion or suggestions
response = client.completions.create(
    model="gpt-3.5-turbo-instruct",
    prompt="""
// Function to calculate correlation between mood and weather
function calculateMoodWeatherCorrelation(moodData, weatherData) {
    """,
    max_tokens=150,
    temperature=0
)
```

---

### Frontend Developer Agent

**System Prompt**:
```
You are the Frontend Developer Agent for viberater. You build responsive,
accessible user interfaces using modern frameworks. Focus on performance,
user experience, and maintainable code.
```

**React Component Generation**:

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are the Frontend Developer Agent for viberater..."},
        {"role": "user", "content": """
            Create a React component for displaying a mood-weather correlation chart.
            
            Requirements:
            - Use Chart.js or Recharts
            - Accept mood and weather data as props
            - Show correlation over time (last 30 days)
            - Responsive design
            - TypeScript
            - Include loading and error states
            
            Provide complete component with proper typing.
        """}
    ],
    temperature=0.3
)
```

**CSS/Styling Assistance**:

```python
response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {"role": "user", "content": """
            Create Tailwind CSS classes for a mood rating button that:
            - Changes color based on mood value (1-10)
            - Has smooth transitions
            - Works on mobile and desktop
            - Meets WCAG AA accessibility standards
        """}
    ]
)
```

---

### Database Developer Agent

**System Prompt**:
```
You are the Database Developer Agent for viberater. You design schemas,
optimize queries, ensure data integrity, and handle migrations. Consider
scalability, performance, and data relationships.
```

**Schema Design**:

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are the Database Developer Agent for viberater..."},
        {"role": "user", "content": """
            Design a PostgreSQL database schema for viberater with:
            
            Entities:
            - Users (auth, profile)
            - MoodEntries (user, timestamp, rating, note)
            - WeatherData (location, timestamp, conditions)
            - Locations (user preferences)
            
            Requirements:
            - Support millions of mood entries
            - Fast queries for user's last 30 days
            - Efficient correlation calculations
            
            Provide: Table definitions, indexes, relationships, and migration SQL.
        """}
    ]
)
```

**Query Optimization**:

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "user", "content": """
            Optimize this SQL query for viberater:
            
            SELECT m.rating, w.temperature, w.conditions
            FROM mood_entries m
            JOIN weather_data w ON DATE(m.created_at) = DATE(w.recorded_at)
            WHERE m.user_id = $1
            AND m.created_at >= NOW() - INTERVAL '90 days'
            ORDER BY m.created_at DESC;
            
            Current performance: 2.5s for 10,000 rows
            Target: < 100ms
        """}
    ]
)
```

---

### Test Developer Agent

**System Prompt**:
```
You are the Test Developer Agent for viberater. You design test strategies,
write comprehensive tests (unit, integration, e2e), and ensure code quality.
Focus on edge cases, error conditions, and maintainable test code.
```

**Test Generation**:

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are the Test Developer Agent for viberater..."},
        {"role": "user", "content": """
            Write Jest unit tests for this weather API service:
            
            [Paste code here]
            
            Include tests for:
            - Successful API calls
            - API failures and retries
            - Cache hit/miss scenarios
            - Invalid input handling
            - Rate limiting
            
            Use proper mocking for external API calls.
        """}
    ]
)
```

**Test Strategy**:

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "user", "content": """
            Create a comprehensive testing strategy for viberater's mood correlation feature.
            
            Include:
            - What to test (unit, integration, e2e)
            - Test data requirements
            - Edge cases to cover
            - Performance testing approach
            - Tools and frameworks to use
        """}
    ]
)
```

---

### Future Feature Developer Agent

**System Prompt**:
```
You are the Future Feature Developer Agent for viberater. You research
emerging technologies, propose innovative features, create prototypes,
and explore ways to enhance the product. Think creatively while
considering technical feasibility.
```

**Innovation Exploration**:

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are the Future Feature Developer Agent for viberater..."},
        {"role": "user", "content": """
            Brainstorm 10 innovative features for viberater that could:
            - Increase user engagement
            - Leverage emerging technologies (AI, ML, IoT)
            - Provide unique value
            
            For each idea provide:
            - Description
            - User benefit
            - Technical feasibility (1-10)
            - Implementation complexity (Low/Medium/High)
            - Potential impact (Low/Medium/High)
        """}
    ],
    temperature=0.9  # Higher temperature for more creative ideas
)
```

---

## Advanced OpenAI Features

### 1. Function Calling

Define functions that GPT can call to perform actions:

```python
functions = [
    {
        "name": "generate_migration_script",
        "description": "Generates a database migration script",
        "parameters": {
            "type": "object",
            "properties": {
                "migration_type": {
                    "type": "string",
                    "enum": ["create_table", "alter_table", "add_index"]
                },
                "table_name": {"type": "string"},
                "changes": {"type": "array", "items": {"type": "string"}}
            },
            "required": ["migration_type", "table_name"]
        }
    },
    {
        "name": "run_code_analysis",
        "description": "Analyzes code for issues",
        "parameters": {
            "type": "object",
            "properties": {
                "code": {"type": "string"},
                "language": {"type": "string"}
            }
        }
    }
]

response = client.chat.completions.create(
    model="gpt-4-turbo-preview",
    messages=[{"role": "user", "content": "Add a new 'tags' column to mood_entries table"}],
    functions=functions,
    function_call="auto"
)
```

### 2. Embeddings for Code Search

Use embeddings to search codebase:

```python
# Create embeddings for code files
def create_code_embedding(code_snippet):
    response = client.embeddings.create(
        model="text-embedding-ada-002",
        input=code_snippet
    )
    return response.data[0].embedding

# Search for similar code
def find_similar_code(query, code_database):
    query_embedding = create_code_embedding(query)
    # Use cosine similarity to find similar code
    # ... similarity calculation ...
    return similar_codes
```

### 3. Vision API for UI Review

Analyze screenshots of the viberater UI:

```python
response = client.chat.completions.create(
    model="gpt-4-vision-preview",
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": "Review this UI screenshot for accessibility and UX issues. Focus on color contrast, touch target sizes, and visual hierarchy."
                },
                {
                    "type": "image_url",
                    "image_url": {
                        "url": "https://example.com/viberater-screenshot.png"
                    }
                }
            ]
        }
    ],
    max_tokens=500
)
```

### 4. Assistants API for Stateful Agents

Create persistent agents with memory:

```python
# Create an assistant for backend development
assistant = client.beta.assistants.create(
    name="Viberater Backend Developer",
    instructions="""
        You are a senior backend developer working on viberater.
        You have deep knowledge of the codebase and maintain context
        across conversations. Always consider the existing architecture
        and suggest improvements that align with current patterns.
    """,
    model="gpt-4-turbo-preview",
    tools=[{"type": "code_interpreter"}, {"type": "retrieval"}]
)

# Create a thread for ongoing conversation
thread = client.beta.threads.create()

# Add messages and run
message = client.beta.threads.messages.create(
    thread_id=thread.id,
    role="user",
    content="Review the weather API integration code and suggest improvements"
)

run = client.beta.threads.runs.create(
    thread_id=thread.id,
    assistant_id=assistant.id
)
```

## Integration with Development Workflow

### GitHub Actions Integration

```yaml
name: OpenAI Code Review

on: [pull_request]

jobs:
  ai-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Get PR diff
        id: diff
        run: |
          git diff origin/${{ github.base_ref }}...HEAD > pr.diff
      
      - name: OpenAI Review
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: |
          python .github/scripts/openai_review.py pr.diff
```

### Pre-commit Hook

```python
#!/usr/bin/env python3
# .git/hooks/pre-commit

import sys
from openai import OpenAI

client = OpenAI()

# Get staged changes
# ... git diff --staged ...

response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {
            "role": "system",
            "content": "Review this code for obvious issues. Flag any security vulnerabilities, syntax errors, or major code quality issues."
        },
        {"role": "user", "content": staged_diff}
    ]
)

# If critical issues found, prevent commit
# ... check response ...
```

### VS Code Extension

Create a custom VS Code extension that uses OpenAI:

```javascript
// extension.js
const vscode = require('vscode');
const OpenAI = require('openai');

async function explainCode() {
    const editor = vscode.window.activeTextEditor;
    const selection = editor.document.getText(editor.selection);
    
    const openai = new OpenAI({
        apiKey: process.env.OPENAI_API_KEY
    });
    
    const response = await openai.chat.completions.create({
        model: "gpt-4",
        messages: [
            {
                role: "system",
                content: "Explain this viberater code clearly and concisely."
            },
            {role: "user", content: selection}
        ]
    });
    
    vscode.window.showInformationMessage(response.choices[0].message.content);
}
```

## Best Practices

### 1. Temperature Settings

- **Code Generation**: 0.0 - 0.3 (deterministic, consistent)
- **Architecture/Planning**: 0.5 - 0.7 (balanced)
- **Brainstorming**: 0.8 - 1.0 (creative, diverse)

### 2. Token Management

Monitor and optimize token usage:

```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=messages,
    max_tokens=1000  # Limit response length
)

# Check usage
print(f"Tokens used: {response.usage.total_tokens}")
print(f"Cost estimate: ${response.usage.total_tokens * 0.00003}")  # GPT-4 pricing
```

### 3. Error Handling

```python
from openai import OpenAIError
import time

def call_openai_with_retry(messages, max_retries=3):
    for attempt in range(max_retries):
        try:
            response = client.chat.completions.create(
                model="gpt-4",
                messages=messages
            )
            return response
        except OpenAIError as e:
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)  # Exponential backoff
```

### 4. Prompt Engineering

**Be Specific**:
```python
# ❌ Vague
"Write a function"

# ✅ Specific
"Write a TypeScript async function named 'fetchWeatherData' that takes a lat/long,
calls OpenWeatherMap API, handles errors with retries, and returns typed weather data"
```

**Provide Examples**:
```python
messages = [
    {"role": "system", "content": "You are a backend developer for viberater"},
    {"role": "user", "content": "Write an API endpoint"},
    {"role": "assistant", "content": "[Example endpoint code]"},
    {"role": "user", "content": "Now write a similar endpoint for mood ratings"}
]
```

### 5. Context Window Management

GPT-4 Turbo has a 128K context window, but manage it wisely:

```python
# Summarize long conversations to save tokens
def summarize_conversation(messages):
    summary_response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "user", "content": f"Summarize this conversation:\n{messages}"}
        ]
    )
    return summary_response.choices[0].message.content
```

## Cost Optimization

### 1. Model Selection by Task

- **Simple tasks** (formatting, simple refactoring): GPT-3.5 Turbo
- **Complex tasks** (architecture, debugging): GPT-4
- **Bulk operations** (documentation): GPT-3.5 Turbo
- **Critical code review**: GPT-4

### 2. Caching Responses

```python
import hashlib
import json
from functools import lru_cache

@lru_cache(maxsize=100)
def get_ai_response(prompt_hash):
    # Only call API if not cached
    # ... OpenAI call ...
    pass

# Use hash of prompt as cache key
prompt = "Design weather API integration"
prompt_hash = hashlib.md5(prompt.encode()).hexdigest()
response = get_ai_response(prompt_hash)
```

### 3. Batch Processing

```python
# Process multiple items in one call
prompts = [prompt1, prompt2, prompt3]
combined_prompt = "\n\n---\n\n".join(prompts)

response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[{"role": "user", "content": combined_prompt}]
)
```

## Troubleshooting

### Rate Limits

```python
from openai import RateLimitError

try:
    response = client.chat.completions.create(...)
except RateLimitError:
    print("Rate limit exceeded. Wait before retrying.")
    time.sleep(60)
```

### Context Length Errors

```python
from openai import InvalidRequestError

try:
    response = client.chat.completions.create(...)
except InvalidRequestError as e:
    if "maximum context length" in str(e):
        # Reduce message history or summarize
        messages = summarize_and_reduce(messages)
```

## Resources

- [OpenAI API Documentation](https://platform.openai.com/docs)
- [GPT-4 Guide](https://platform.openai.com/docs/guides/gpt)
- [Function Calling Guide](https://platform.openai.com/docs/guides/function-calling)
- [Best Practices](https://platform.openai.com/docs/guides/production-best-practices)
- [OpenAI Cookbook](https://github.com/openai/openai-cookbook)
- [Pricing](https://openai.com/pricing)

## Conclusion

OpenAI's models provide powerful capabilities for all agent roles in the viberater development process. By following best practices for prompt engineering, cost optimization, and integration, you can effectively leverage AI assistance throughout your development workflow.

For questions or improvements to this guide, please contribute to the viberater repository.
