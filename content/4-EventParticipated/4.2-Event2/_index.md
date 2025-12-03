---
title: "Event 2 - AWS GenAI Builder Club: AI-Driven Development Life Cycle"
weight: 2
chapter: false
---

## AWS GenAI Builder Club: AI-Driven Development Life Cycle - Reimagining Software Engineering

**Date & Time:** Friday, October 3, 2025 | 14:00 (2:00 PM)

**Location:** AWS Event Hall, L26 Bitexco Tower, Ho Chi Minh City

**Instructors:** Toan Huynh & My Nguyen

**Coordinators:** Diem My, Dai Truong, Dinh Nguyen

---

## Event Overview

This AWS GenAI Builder Club session explored the AI-Driven Development Lifecycle (AI-DLC), a transformative approach to software engineering that integrates AI as a central collaborator throughout the entire development process. The session featured hands-on demonstrations of Amazon Q Developer and Kiro, showcasing practical applications of AI in modern software development.

---

## Agenda

| Time | Session | Instructor |
|---|---|---|
| 14:00 - 14:15 | Welcoming | - |
| 14:15 - 15:30 | AI-Driven Development Life Cycle Overview & Amazon Q Developer Demonstration | Toan Huynh |
| 15:30 - 15:45 | Break | - |
| 15:45 - 16:30 | Kiro Demonstration | My Nguyen |

---

## Key Concepts & Learnings

### 1. AI-Driven Development Lifecycle (AI-DLC) Overview

#### Core Philosophy

The AI-Driven Development Lifecycle represents a fundamental shift in how software is built. Rather than treating AI as an afterthought or simple code completion tool, AI-DLC embeds AI as an intelligent partner throughout the entire development process.

**Key Principles:**

- **You Are in Control** - AI is your assistant, not your manager. You must maintain decision-making authority over the project direction and implementation details.

- **AI as Collaborator, Not Replacement** - AI should ask critical questions about your project requirements, architecture, and goals. The collaboration should be bidirectional, with you guiding the AI's suggestions.

- **Plan Before Implementation** - Always create a comprehensive plan before diving into code. AI can help generate this plan, but you must review, validate, and refine it.

#### The Development Workflow

**Step 1: Create a Project Plan**

- Define clear project requirements and scope
- Ask AI to generate a plan based on your specifications
- Review the plan critically and request modifications
- Ensure the plan is detailed and unambiguous

**Step 2: Break Down into User Stories**

- Convert the plan into user stories with clear acceptance criteria
- Divide large scope into smaller, manageable units
- Each unit becomes a mini-project that can be assigned to team members
- Estimate timelines for each unit (though be cautious of over-estimation)

**Step 3: Define Technology Stack**

- Clearly specify the technologies, frameworks, and tools to be used
- Instead of telling AI "don't implement this," tell it "implement this way"
- Positive direction yields higher success rates than negative constraints

**Step 4: Detailed Requirements & Design**

- Write requirements with precision and clarity
- Collaborate with AI to create detailed specifications
- Define data models, API contracts, and system architecture
- Create design documents before implementation begins

**Step 5: Implementation & Verification**

- Implement features according to the plan
- Use mob development approach (team works together on code)
- Verify all output code as a team
- Conduct code reviews and quality checks

**Step 6: Testing & Deployment**

- Move through environments: Development (Dev) → Testing (QA) → User Acceptance Testing (UAT) → Production (Prod)
- Ensure quality gates at each stage
- Validate functionality before production release

#### Critical Success Factors

- **Create a Plan First** - Don't expect AI to handle everything. Always start with a clear plan.
- **Review Regularly** - Continuously review AI suggestions and outputs. High error rates are possible.
- **You Are the Manager** - Your value lies in code validation and project management, not in writing every line of code.
- **Ask Clarifying Questions** - Ensure AI understands your project context by asking it critical questions about requirements, architecture, and goals.
- **Use Prompt Templates** - Create structured prompts that include user context, user stories, and specific requirements to get clearer AI responses.
- **Export Plans to Files** - Have AI generate plans as files you can save, review, and modify. This creates a living document for future reference.
- **Be Polite to AI** - Maintain respectful communication with AI tools. Good rapport may help in future interactions (and it's just good practice!).

---

### 2. Amazon Q Developer Demonstration

#### What is Amazon Q Developer?

Amazon Q Developer is an AI-powered assistant that transforms the software development lifecycle (SDLC) through agentic capabilities across multiple platforms:

- **AWS Console** - Helps with infrastructure and service configuration
- **IDE (Integrated Development Environment)** - Provides code generation and optimization suggestions
- **CLI (Command Line Interface)** - Assists with command generation and automation
- **DevSecOps Platforms** - Integrates security practices into the development workflow

#### Key Capabilities

**Code Generation & Quality**

- Accelerates code generation with AI-powered suggestions
- Improves code quality through intelligent recommendations
- Maintains seamless integration with existing workflows
- Understands complex codebases and suggests optimizations

**Documentation & Testing**

- Automatically generates comprehensive documentation
- Creates unit tests with minimal manual effort
- Significantly improves code maintainability and reliability
- Reduces boilerplate and repetitive coding tasks

**Intelligent Collaboration**

- Acts as an intelligent collaborator leveraging large language models
- Combines deep AWS service knowledge with coding expertise
- Helps developers accelerate development cycles
- Enhances code quality and strengthens security posture

**Automation Across Development Lifecycle**

- Automates routine tasks across the entire development lifecycle
- Reduces manual, repetitive work
- Allows developers to focus on higher-value, creative tasks
- Improves overall productivity and efficiency

#### Best Practices for Using Amazon Q Developer

1. **Provide Clear Context** - Give Q detailed information about your project, architecture, and requirements
2. **Use Specific Prompts** - Instead of vague requests, provide specific, detailed prompts with examples
3. **Review Suggestions** - Always review Q's suggestions before implementing them
4. **Iterate and Refine** - If the first suggestion isn't perfect, refine your prompt and try again
5. **Leverage AWS Knowledge** - Take advantage of Q's deep understanding of AWS services and best practices

---

### 3. Kiro Demonstration

#### What is Kiro?

Kiro is an agentic IDE (Integrated Development Environment) developed by Amazon Web Services that bridges the gap between rapid AI-powered prototyping and production-ready software development. It's currently in public preview.

#### Core Philosophy

Kiro embodies the principle that AI should enhance developer productivity while maintaining professional standards, clear structure, comprehensive testing, documentation, and long-term maintainability.

#### Key Features

**Spec-Driven Development**

- When you submit a requirement (e.g., "add a product rating system"), Kiro converts it into:
  - User stories with clear acceptance criteria
  - Design documentation
  - Task lists and implementation plans
  - Structured specifications before code generation

**Agent Hooks & Automation**

- Automatically triggers tasks based on events:
  - File saves trigger documentation updates
  - Commits trigger test generation
  - Specific actions trigger performance optimization
  - Reduces manual, repetitive work

**Steering & Project Context**

- Create steering files (markdown) to describe:
  - Project structure and organization
  - Coding standards and conventions
  - Desired architecture patterns
  - Team guidelines and best practices
- Helps Kiro understand your project context deeply

**Multi-File Analysis & Intent Understanding**

- Analyzes multiple files simultaneously
- Understands functional goals across the codebase
- Makes changes aligned with overall project objectives
- Goes beyond simple code completion

**VS Code Integration**

- Built on VS Code's open-source foundation
- Import settings, themes, and extensions from VS Code
- Familiar interface for existing VS Code users
- Seamless transition for developers

**Flexible AI Model Selection**

- Currently uses Claude Sonnet 4 as default
- "Auto" mode combines multiple models based on context
- Balance between quality and cost
- Flexibility to choose different models for different tasks

#### Advantages of Using Kiro

**Increased Transparency & Control**

- Start with specifications before code generation
- Review and validate specs before implementation
- Reduce hallucinated code or misaligned implementations
- Maintain clear traceability from requirements to code

**Reduced Boilerplate & Repetitive Tasks**

- Agent hooks automate documentation generation
- Automatic unit test creation
- Automatic information updates
- Frees developers for higher-value work

**Security & Privacy**

- Most code operations happen locally
- Data only sent externally with explicit permission
- Maintains control over sensitive information

**Extensibility & Flexibility**

- Integrates external tools via MCP (Model Context Protocol)
- Supports multiple AI models
- Not locked into a single AI environment
- Adaptable to different team workflows

#### Limitations & Considerations

- **Preview Status** - Still in public preview; stability and features may change
- **Complex Projects** - May struggle with deep contextual understanding in highly complex projects
- **Supervision Required** - Users still need to oversee and validate AI decisions
- **Future Pricing** - Expected pricing tiers:
  - Free: ~50 tasks/month
  - Pro: ~1,000 tasks/month
  - Pro+: ~3,000 tasks/month

#### When to Use Kiro

- You want an AI + programming workflow that maintains professionalism and clear structure
- Building rapid prototypes but concerned about production sustainability
- Exploring how AI can become a true programming colleague, not just a code suggestion tool
- You need spec-driven development with automated documentation and testing

---

## Common Pitfalls When Using AI in Development

### 1. Expecting AI to Handle Everything

**Problem:** Many developers expect AI to complete entire projects autonomously.

**Solution:** Always create a plan first and review regularly. AI is a tool to enhance productivity, not replace developer judgment.

### 2. High Error Rates

**Problem:** AI can make mistakes, especially in complex scenarios.

**Solution:** Implement regular review cycles. Validate all AI-generated code before deployment.

### 3. Lack of Clear Requirements

**Problem:** Vague or unclear requirements lead to vague AI outputs.

**Solution:** Write requirements with precision. Collaborate with AI to create detailed specifications before implementation.

### 4. Negative Constraints Instead of Positive Direction

**Problem:** Telling AI "don't do this" is less effective than "do this."

**Solution:** Use positive, specific instructions. Higher success rates come from clear positive direction.

### 5. Insufficient Project Context

**Problem:** AI doesn't understand your project's unique requirements and constraints.

**Solution:** Create steering files, provide detailed context, and ask AI critical questions about your project.

### 6. Treating AI as a Manager

**Problem:** Letting AI make all decisions about project direction and architecture.

**Solution:** Remember: **You are the manager.** Your value lies in code validation and project oversight, not in writing every line of code.

---

## Key Takeaways

1. **AI is Your Assistant** - Maintain control over project decisions and implementation direction

2. **Plan First, Code Second** - Always create a comprehensive plan before implementation

3. **Collaboration Over Automation** - AI should ask questions and collaborate, not just execute commands

4. **Clear Requirements Matter** - Precision in requirements leads to better AI outputs

5. **Regular Review is Essential** - Don't expect AI to be perfect; review and validate continuously

6. **You Are the Code Manager** - Your value is in validation and oversight, not in writing every line

7. **Use Structured Prompts** - Templates with context, user stories, and requirements yield better results

8. **Export Plans to Files** - Create living documents you can reference and modify

9. **Positive Direction Works Better** - Tell AI what to do, not what to avoid

10. **Experience Matters** - Use these tools hands-on to understand their capabilities and limitations

---

## Recommended Tools & Resources

- **Amazon Q Developer** - AI-powered development assistant integrated with AWS services
- **Kiro IDE** - Spec-driven development environment with AI collaboration
- **AWS CodeWhisperer** - Code generation and optimization tool
- **MCP (Model Context Protocol)** - Framework for integrating external tools and services

---

## Conclusion

The AI-Driven Development Lifecycle represents a new paradigm in software engineering where AI and humans collaborate as equals. Success requires clear planning, regular review, precise requirements, and maintaining developer control over project direction. Tools like Amazon Q Developer and Kiro are enabling this new workflow, but they work best when developers understand their capabilities and limitations, and maintain their role as project managers and code validators.
