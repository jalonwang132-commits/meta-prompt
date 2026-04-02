# Tool Restriction Sets Reference
## Source: `src/constants/tools.ts`

## Three-Layer Tool Architecture

```typescript
// Tools DISALLOWED for ALL agents (including built-in):
ALL_AGENT_DISALLOWED_TOOLS = {
  TASK_OUTPUT_TOOL_NAME,
  EXIT_PLAN_MODE_V2_TOOL_NAME,
  ENTER_PLAN_MODE_TOOL_NAME,
  AGENT_TOOL_NAME (except for ant users),
  ASK_USER_QUESTION_TOOL_NAME,
  TASK_STOP_TOOL_NAME,
  WORKFLOW_TOOL_NAME
}

// Additional tools disallowed for CUSTOM agents:
CUSTOM_AGENT_DISALLOWED_TOOLS = { ...ALL_AGENT_DISALLOWED_TOOLS }

// Tools allowed for ASYNC agents (background subagents):
ASYNC_AGENT_ALLOWED_TOOLS = {
  FILE_READ_TOOL_NAME,
  WEB_SEARCH_TOOL_NAME,
  TODO_WRITE_TOOL_NAME,
  GREP_TOOL_NAME,
  WEB_FETCH_TOOL_NAME,
  GLOB_TOOL_NAME,
  SHELL_TOOL_NAMES,
  FILE_EDIT_TOOL_NAME,
  FILE_WRITE_TOOL_NAME,
  NOTEBOOK_EDIT_TOOL_NAME,
  SKILL_TOOL_NAME,
  SYNTHETIC_OUTPUT_TOOL_NAME,
  TOOL_SEARCH_TOOL_NAME,
  ENTER_WORKTREE_TOOL_NAME,
  EXIT_WORKTREE_TOOL_NAME
}

// Tools allowed in COORDINATOR mode:
COORDINATOR_MODE_ALLOWED_TOOLS = {
  AGENT_TOOL_NAME,
  TASK_STOP_TOOL_NAME,
  SEND_MESSAGE_TOOL_NAME,
  SYNTHETIC_OUTPUT_TOOL_NAME
}
```

## Tool Filtering Logic

```typescript
function filterToolsForAgent({ tools, isBuiltIn, isAsync, permissionMode }) {
  return tools.filter(tool => {
    // Allow MCP tools for all agents
    if (tool.name.startsWith('mcp__')) return true

    // ExitPlanMode allowed in plan mode
    if (tool === EXIT_PLAN_MODE_V2_TOOL_NAME && permissionMode === 'plan') return true

    // Remove ALL_AGENT_DISALLOWED_TOOLS
    if (ALL_AGENT_DISALLOWED_TOOLS.has(tool.name)) return false

    // Remove CUSTOM_AGENT_DISALLOWED_TOOLS for non-built-in agents
    if (!isBuiltIn && CUSTOM_AGENT_DISALLOWED_TOOLS.has(tool.name)) return false

    // For async agents, check ASYNC_AGENT_ALLOWED_TOOLS
    if (isAsync && !ASYNC_AGENT_ALLOWED_TOOLS.has(tool.name)) return false

    return true
  })
}
```
