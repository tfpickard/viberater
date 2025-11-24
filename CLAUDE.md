# Claude AI Integration Guide

This guide explains how to use Claude AI (by Anthropic) as an agent within the viberater multi-agent development system.

## Overview

Claude is an AI assistant that can take on various agent roles defined in AGENTS.md. Claude excels at understanding context, writing code, analyzing systems, and providing detailed explanations.

## Prerequisites

- Access to Claude (via API or web interface)
- Familiarity with the viberater project architecture
- Understanding of the agent roles defined in AGENTS.md

## Setting Up Claude as an Agent

### 1. API Access

For programmatic access to Claude:

```bash
# Install the Anthropic Python SDK
pip install anthropic

# Set your API key
export ANTHROPIC_API_KEY='your-api-key-here'
```

**Python Example**:
```python
import anthropic

client = anthropic.Anthropic(
    api_key="your-api-key-here"
)

message = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    max_tokens=4096,
    messages=[
        {"role": "user", "content": "Analyze the viberater weather-mood correlation algorithm"}
    ]
)

print(message.content)
```

### 2. Web Interface

Access Claude through:
- Claude.ai (web interface)
- Claude for Slack
- Claude API playground

## Using Claude for Different Agent Roles

### Planner Agent

**Prompt Template**:
```
You are the Planner Agent for the viberater project - a weather-mood rating application.

Context: [Provide current project status, completed features, upcoming goals]

Task: Create a detailed project plan for [specific feature or milestone]

Please provide:
1. Broken down tasks with estimated effort
2. Dependencies between tasks
3. Risk assessment
4. Suggested timeline
5. Resource allocation recommendations
```

**Best Practices**:
- Provide Claude with current sprint status
- Share recent velocity metrics
- Include stakeholder requirements
- Ask for multiple planning scenarios (optimistic, realistic, pessimistic)

---

### Architect Agent

**Prompt Template**:
```
You are the Architect Agent for viberater, a weather-mood correlation application.

Current Architecture: [Describe existing system]

Challenge: [Specific architectural decision needed]

Please provide:
1. Multiple architectural approaches
2. Pros and cons of each approach
3. Recommendation with justification
4. Implementation considerations
5. Potential risks and mitigation strategies
```

**Example Use Cases**:
- Designing the weather API integration layer
- Database schema optimization
- Caching strategy for weather data
- Real-time vs batch processing decisions
- API design (REST vs GraphQL)

---

### Product Designer Agent

**Prompt Template**:
```
You are the Product Designer Agent for viberater.

User Need: [Describe the user problem or feature request]

Context: The app helps users track how weather affects their mood.

Please provide:
1. User journey map
2. Key user interactions
3. UI/UX recommendations
4. Accessibility considerations
5. Mobile-first design principles
6. Visual hierarchy suggestions
```

**Tips for Better Results**:
- Share user feedback or research findings
- Describe current UI pain points
- Request specific deliverables (wireframes description, user flows)
- Ask Claude to consider different user personas

---

### Backend Developer Agent

**Prompt Template**:
```
You are the Backend Developer Agent for viberater.

Task: [Specific backend development task]

Tech Stack: [List current technologies - e.g., Node.js, Python, etc.]

Requirements:
- [List functional requirements]
- [List non-functional requirements]

Please provide:
1. Implementation approach
2. Code structure and organization
3. API endpoint design (if applicable)
4. Error handling strategy
5. Testing approach
6. Performance considerations
```

**Common Backend Tasks**:
- Weather API integration (OpenWeatherMap, Weather.com, etc.)
- Mood rating CRUD operations
- Correlation calculation algorithms
- User authentication implementation
- Data caching and optimization

**Code Review with Claude**:
```
Review this backend code for the viberater project:

[Paste code here]

Please check for:
1. Code quality and best practices
2. Security vulnerabilities
3. Performance issues
4. Error handling completeness
5. Test coverage gaps
```

---

### Frontend Developer Agent

**Prompt Template**:
```
You are the Frontend Developer Agent for viberater.

Task: [Specific UI component or page]

Framework: [React, Vue, vanilla JS, etc.]

Requirements:
- [List features]
- [Design specifications]
- [Accessibility requirements]

Please provide:
1. Component structure
2. State management approach
3. API integration code
4. Responsive design implementation
5. Accessibility features
6. Testing strategy
```

**Example Tasks**:
- Mood rating input component
- Weather data visualization charts
- Responsive navigation
- Form validation
- Real-time data updates

---

### Database Developer Agent

**Prompt Template**:
```
You are the Database Developer Agent for viberater.

Database: [PostgreSQL, MongoDB, etc.]

Task: [Schema design, query optimization, etc.]

Requirements:
- [Data to store]
- [Query patterns]
- [Performance requirements]

Please provide:
1. Schema design or modifications
2. Index strategy
3. Query optimization suggestions
4. Migration plan (if changing existing schema)
5. Backup and recovery considerations
```

**Key Database Entities for Viberater**:
- Users (authentication, preferences)
- MoodRatings (timestamp, value, location, notes)
- WeatherData (cached weather information)
- Locations (user-saved locations)
- CorrelationMetrics (analyzed patterns)

---

### Test Developer Agent

**Prompt Template**:
```
You are the Test Developer Agent for viberater.

Component/Feature to Test: [Describe what needs testing]

Context: [Provide relevant code or functionality description]

Please provide:
1. Test strategy (unit, integration, e2e)
2. Test cases covering normal and edge cases
3. Test code examples
4. Mock/stub strategies for external dependencies
5. Performance/load testing approach (if applicable)
```

**Testing Priorities for Viberater**:
- Weather API integration reliability
- Mood rating data integrity
- Correlation calculation accuracy
- Cross-browser compatibility
- Mobile responsiveness
- Offline functionality (if applicable)

---

### Future Feature Developer Agent

**Prompt Template**:
```
You are the Future Feature Developer Agent for viberater.

Current State: [Describe current capabilities]

Goal: Explore [specific innovation area or general future enhancements]

Please provide:
1. Innovative feature ideas
2. Technology research and recommendations
3. Feasibility assessment
4. Prototype approach
5. Potential impact on users
6. Implementation complexity estimate
```

**Innovation Areas for Viberater**:
- Machine learning mood predictions
- Wearable device integration
- Social sharing features
- Advanced data visualization
- Predictive analytics

---

## Advanced Claude Usage

### Multi-Turn Conversations

Claude excels at multi-turn conversations. Use this for:

1. **Iterative Design**: Start with high-level design, then drill into specifics
2. **Code Refinement**: Share code, get feedback, iterate
3. **Problem Solving**: Break down complex problems step-by-step

**Example Flow**:
```
Turn 1: "Design a weather API integration layer for viberater"
[Claude provides high-level design]

Turn 2: "Great, now focus on the caching strategy. How should we handle cache invalidation?"
[Claude provides detailed caching approach]

Turn 3: "Show me code for implementing this caching layer in Python"
[Claude provides implementation]
```

### Context Management

Claude can handle long contexts. Provide:
- Relevant code files
- Architecture documentation
- Previous decisions and rationale
- User feedback or requirements

**Best Practice**: Use Projects feature (if available) to maintain persistent context about viberater.

### Chain of Thought Prompting

For complex tasks, ask Claude to think step-by-step:

```
Think through this architecture decision step by step:

1. First, identify the key requirements
2. Then, list possible approaches
3. Evaluate each approach
4. Make a recommendation

Task: Choose between REST and GraphQL for viberater's API
```

### Artifacts and Code Generation

Claude can generate complete code files as artifacts. Request:
- Complete component implementations
- Test suites
- Configuration files
- Documentation

## Integration with Development Workflow

### GitHub Integration

Use Claude to:
- Review pull requests
- Write commit messages
- Generate issue descriptions
- Create documentation

### CI/CD Integration

Integrate Claude API into pipelines for:
- Automated code review
- Documentation generation
- Test case generation
- Release notes creation

### Example GitHub Actions Workflow:

```yaml
name: Claude Code Review

on: [pull_request]

jobs:
  claude-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run Claude Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          # Script to send PR diff to Claude for review
          python scripts/claude_review.py
```

## Best Practices

### 1. Be Specific

❌ "Help me with the backend"
✅ "Design a caching strategy for weather API responses in viberater, considering data freshness requirements (1-hour max staleness) and rate limits (100 calls/hour)"

### 2. Provide Context

Always include:
- Project name and purpose
- Relevant code or architecture
- Constraints and requirements
- Previous decisions

### 3. Iterate

Don't expect perfect answers on first try:
- Ask follow-up questions
- Request clarifications
- Drill into specifics
- Ask for alternatives

### 4. Verify Output

Claude is powerful but can make mistakes:
- Review generated code carefully
- Test implementations thoroughly
- Verify technical recommendations
- Cross-reference with documentation

### 5. Use Appropriate Models

- **Claude 3.5 Sonnet**: Best balance of intelligence and speed (recommended for most tasks)
- **Claude 3 Opus**: Maximum capability for complex reasoning
- **Claude 3 Haiku**: Fast and cost-effective for simpler tasks

## Example Session: Complete Feature Development

```
Session: Implementing "Mood Pattern Analysis" Feature

[Turn 1 - Product Designer Role]
User: "You are the Product Designer for viberater. Design a mood pattern analysis feature that shows users how their mood correlates with weather over time."

Claude: [Provides user journey, key interactions, UI recommendations]

[Turn 2 - Architect Role]
User: "Now as the Architect, design the technical approach for this feature."

Claude: [Provides architecture design, data flow, component interactions]

[Turn 3 - Backend Developer Role]
User: "As the Backend Developer, implement the correlation calculation endpoint."

Claude: [Provides code implementation]

[Turn 4 - Frontend Developer Role]
User: "As the Frontend Developer, create the visualization component."

Claude: [Provides React component code]

[Turn 5 - Test Developer Role]
User: "As the Test Developer, write tests for the correlation calculation."

Claude: [Provides test suite]
```

## Troubleshooting

### Issue: Claude provides generic responses

**Solution**: Provide more specific context about viberater, include code samples, and be explicit about requirements.

### Issue: Code doesn't match our tech stack

**Solution**: Explicitly state your tech stack in the prompt (e.g., "Use TypeScript with React and Express.js").

### Issue: Responses are too verbose

**Solution**: Request concise answers: "Provide a brief, implementation-focused answer."

### Issue: Missing error handling or edge cases

**Solution**: Explicitly ask: "Include comprehensive error handling and edge cases in your solution."

## Resources

- [Anthropic Documentation](https://docs.anthropic.com/)
- [Claude API Reference](https://docs.anthropic.com/claude/reference/getting-started-with-the-api)
- [Prompt Engineering Guide](https://docs.anthropic.com/claude/docs/prompt-engineering)
- [Best Practices](https://docs.anthropic.com/claude/docs/best-practices)

## Conclusion

Claude can serve as a versatile agent across all roles in the viberater development process. The key to success is providing clear context, being specific about requirements, and iterating on responses to refine outputs.

For questions or improvements to this guide, please contribute to the viberater repository.
