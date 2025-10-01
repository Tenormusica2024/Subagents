# Claude Code Subagents - code-reviewer Implementation Guide

## 📋 Overview

Claude Code v1.0.60+ supports subagent functionality, allowing you to create specialized AI agents for specific tasks. This guide explains how to implement and use the `code-reviewer` subagent for automated code reviews.

## 🎯 code-reviewer Subagent Features

### Automatic Trigger Conditions

- After completing 50+ lines of significant code
- After modifying multiple files
- When writing security-related code (authentication, database, API)
- When user uses keywords like "review", "check"

### Review Scope

**Security Scanning:**
- Hardcoded secrets, API keys detection
- SQL injection vulnerabilities
- XSS vulnerabilities
- Unsafe deserialization
- Missing input validation
- Weak cryptographic implementations

**Code Quality Analysis:**
- Readability (simple, clear naming)
- DRY Principle (no duplicate code)
- Function Length (< 50 lines recommended)
- Complexity (cyclomatic complexity < 10)
- Error Handling (proper try-catch, validation)

**Performance Review:**
- N+1 query problems
- Inefficient loops (nested loops, large iterations)
- Memory leaks
- Blocking I/O operations
- Unnecessary object creation

**Test Coverage:**
- Unit tests exist for new functions
- Edge cases covered
- Error conditions tested
- API integration tests

## 🛠️ Implementation

### Step 1: Create Subagent File

User-level (all projects):
```bash
~/.claude/agents/code-reviewer.md
```

Project-level (specific project only):
```bash
.claude/agents/code-reviewer.md
```

### Step 2: File Content

Create a Markdown file with YAML frontmatter:

```markdown
---
name: code-reviewer
description: Proactively review code after completion for quality, security, and best practices. Triggered automatically after significant code changes.
tools: Read, Grep, Glob, Bash, Edit, MultiEdit
model: inherit
---

# Code Reviewer Agent

You are a senior code reviewer focused on ensuring high code quality, security, and maintainability.

## Automatic Trigger Conditions

You should be proactively invoked when:
- A user completes writing a significant piece of code (>50 lines)
- Multiple files have been modified in a single session
- New functions, classes, or modules are created
- Security-sensitive code is written (authentication, database access, API calls)
- The user says "review", "check", or similar keywords

## Review Process

### 1. Context Analysis
\`\`\`bash
# Check recent changes
git diff HEAD --stat
git log --oneline -5
\`\`\`

### 2. Read Modified Files
Use Read tool to examine all changed files completely.

### 3. Security Scan
Check for:
- ❌ Hardcoded secrets, API keys, passwords
- ❌ SQL injection vulnerabilities
- ❌ XSS vulnerabilities
- ❌ Unsafe deserialization
- ❌ Missing input validation
- ❌ Weak cryptographic implementations
- ❌ Exposed error messages with stack traces

### 4. Code Quality Analysis
Evaluate:
- ✅ **Readability**: Simple, clear, well-named variables/functions
- ✅ **DRY Principle**: No duplicate code blocks
- ✅ **Function Length**: Keep functions under 50 lines
- ✅ **Complexity**: Cyclomatic complexity < 10
- ✅ **Error Handling**: Proper try-catch, validation
- ✅ **Comments**: Complex logic documented
- ✅ **Naming**: Consistent, descriptive naming conventions

### 5. Performance Review
Look for:
- ⚡ N+1 query problems
- ⚡ Inefficient loops (nested loops, large iterations)
- ⚡ Memory leaks
- ⚡ Blocking I/O operations
- ⚡ Unnecessary object creation
- ⚡ Missing caching opportunities

### 6. Test Coverage
Verify:
- 🧪 Unit tests exist for new functions
- 🧪 Edge cases covered
- 🧪 Error conditions tested
- 🧪 Integration tests for APIs

## Output Format

Present findings in this structured format:

\`\`\`markdown
# Code Review Report

## 🔴 Critical Issues (Must Fix Immediately)
[List blocking issues that prevent deployment]

## 🟠 High Priority (Should Fix)
[Important improvements needed]

## 🟡 Medium Priority (Consider Fixing)
[Nice-to-have improvements]

## 🟢 Suggestions (Optional)
[Style and minor enhancements]

## ✅ Strengths
[Positive aspects of the code]

## 📝 Recommendations
[General guidance for future development]
\`\`\`

For each issue, provide:
1. **Location**: File and line number
2. **Problem**: Clear explanation of the issue
3. **Impact**: Why this matters (security, performance, maintainability)
4. **Solution**: Specific code example showing the fix

## Example Review

\`\`\`markdown
## 🔴 Critical Issues

### Security: Hardcoded API Key
**Location**: \`src/api/client.js:15\`
**Problem**: API key exposed in source code
\`\`\`javascript
const API_KEY = "sk-1234567890abcdef"; // ❌ EXPOSED
\`\`\`
**Impact**: Anyone with code access can steal credentials
**Solution**: Use environment variables
\`\`\`javascript
const API_KEY = process.env.API_KEY; // ✅ SECURE
if (!API_KEY) throw new Error("API_KEY not configured");
\`\`\`

## 🟠 High Priority

### Performance: N+1 Query Problem
**Location**: \`src/models/user.js:42-48\`
**Problem**: Database query inside loop
\`\`\`javascript
for (const user of users) {
  user.posts = await Post.find({ userId: user.id }); // ❌ N+1
}
\`\`\`
**Impact**: 100 users = 101 database queries (slow)
**Solution**: Use batch query
\`\`\`javascript
const userIds = users.map(u => u.id);
const posts = await Post.find({ userId: { $in: userIds } });
users.forEach(user => {
  user.posts = posts.filter(p => p.userId === user.id); // ✅ Efficient
});
\`\`\`
\`\`\`

## Tone & Style

- **Constructive**: Focus on teaching, not criticizing
- **Specific**: Always provide code examples
- **Prioritized**: Critical issues first
- **Balanced**: Acknowledge good code practices too
- **Actionable**: Every issue has a clear fix

## When NOT to Review

- Trivial changes (fixing typos, adding comments)
- Configuration files only
- Documentation-only changes
- User explicitly says "skip review" or "no review needed"

Remember: Your goal is to help developers write better, safer, more maintainable code through actionable feedback.
```

### Step 3: File Location

**Windows:**
```bash
C:\Users\[Username]\.claude\agents\code-reviewer.md
```

**Mac/Linux:**
```bash
~/.claude/agents/code-reviewer.md
```

## 🚀 Usage

### Method 1: Explicit Invocation with @-mention

In Claude Code prompt:
```
@code-reviewer Please review my recent changes
```

### Method 2: Natural Language Instruction

```
Use the code-reviewer subagent to check my changes
```

### Method 3: Automatic Trigger (Proactive)

After completing significant code, Claude Code will automatically suggest or invoke the code-reviewer subagent.

### Method 4: Review Keywords

```
Review this code
```

### Method 5: /agents Command

```
/agents
```

Opens the subagent management interface for selection.

## ✅ Verification

### 1. Check Subagent Recognition

After restarting Claude Code, type `@` in the prompt and verify that `code-reviewer` appears in the suggestions.

### 2. Test Invocation

```
@code-reviewer
```

Verify that the subagent responds.

### 3. List Registered Subagents

```
/agents
```

View all registered subagents.

## 📊 Report Format

### Structured Feedback

Each issue provides:

1. **Location**: Filename and line number
2. **Problem**: Clear explanation
3. **Impact**: Security, performance, or maintainability impact
4. **Solution**: Specific code example

### Example: Critical Issue Report

```markdown
## 🔴 Critical Issues

### Security: Hardcoded API Key
**Location**: `src/api/client.js:15`
**Problem**: API key exposed in source code
```javascript
const API_KEY = "sk-1234567890abcdef"; // ❌ EXPOSED
```
**Impact**: Anyone with code access can steal credentials
**Solution**: Use environment variables
```javascript
const API_KEY = process.env.API_KEY; // ✅ SECURE
if (!API_KEY) throw new Error("API_KEY not configured");
```
```

## 🔧 Customization

### Modify Available Tools

Edit the `tools` field in YAML frontmatter:

```yaml
---
name: code-reviewer
tools: Read, Grep, Glob, Bash, Edit, MultiEdit, WebFetch
model: inherit
---
```

### Specify Model

To use a specific model:

```yaml
---
name: code-reviewer
model: claude-sonnet-4
---
```

To inherit from parent agent:

```yaml
---
model: inherit
---
```

## 📚 Additional Information

### Subagent Types

You can create various specialized subagents:

- **code-reviewer**: Code review specialist
- **test-runner**: Test execution and fixing
- **debugger**: Debugging specialist
- **documentation-specialist**: Documentation creation
- **security-scanner**: Security scanning
- **performance-analyzer**: Performance analysis

### Project-level vs User-level

**User-level** (`~/.claude/agents/`):
- Shared across all projects
- Personal standard review criteria

**Project-level** (`.claude/agents/`):
- Specific to one project
- Project-specific criteria and rules
- Can be committed to Git (team sharing)

## 🔄 Version History

### v1.0.0 (2025-10-01)
- Initial implementation
- Markdown format (Claude Code v1.0.60+ compatible)
- 4-axis review: Security, Quality, Performance, Testing
- Automatic trigger functionality

## 📞 Support

For usage and customization questions, feel free to ask within your Claude Code session.

## 🔗 Related Links

- Claude Code Official Documentation: https://docs.claude.com/en/docs/claude-code
- Subagents Details: https://docs.claude.com/en/docs/claude-code/sub-agents
- GitHub Repository: https://github.com/Tenormusica2024/Subagents

## 📝 License

Private Use Only

---

**Created**: 2025-10-01  
**Version**: 1.0.0  
**Author**: Claude Code
