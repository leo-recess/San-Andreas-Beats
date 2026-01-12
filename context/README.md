# Context Directory

> Structured logging for AI agent session continuity

## Purpose

This directory preserves context across AI agent sessions, enabling:
- **Handoffs** between sessions without losing progress
- **Audit trail** of decisions and actions taken
- **Capability documentation** for testing and validation
- **Learning capture** for avoiding repeated mistakes

## File Structure

| File | Purpose | Update Frequency |
|------|---------|------------------|
| `SESSION_LOG.md` | Chronological history of all sessions | Every session |
| `CAPABILITIES_TESTED.md` | Documentation of tested capabilities | As capabilities are used |
| `LEARNINGS.md` | Gotchas, discoveries, and tips | When issues are encountered |
| `PROGRESS.md` | Quick progress snapshots | During long tasks |

## Log Format Standards

### SESSION_LOG.md

Each session entry includes:
```markdown
## Session: YYYY-MM-DD

**Start Time:** HH:MM UTC
**End Time:** HH:MM UTC or Ongoing
**Agent:** Model name
**Project:** Project name

### Goals Accomplished
| # | Task | Status | Notes |

### Key Decisions Made
1. Decision with rationale

### Files Created/Modified
Tree structure of changes

### External Integrations Used
| Service | Action | Result |

### Context for Next Session
Key points for continuity
```

### CAPABILITIES_TESTED.md

Documents each capability with:
- **Category** (Data, Integration, Planning, Code, DevOps, Context)
- **Status** (Tested, Partial, Not Tested)
- **Evidence** (What was done to test it)
- **Code samples** where applicable

### LEARNINGS.md

Captures:
- **Problem encountered**
- **Root cause**
- **Solution applied**
- **Prevention for future**

## Usage

### Starting a Session
1. Read `SESSION_LOG.md` for latest context
2. Check `LEARNINGS.md` for known issues
3. Continue from where previous session left off

### During a Session
1. Update `SESSION_LOG.md` as goals are accomplished
2. Add to `LEARNINGS.md` when issues are discovered
3. Document new capabilities in `CAPABILITIES_TESTED.md`

### Ending a Session
1. Summarize session in `SESSION_LOG.md`
2. Commit all context files to git
3. Push to remote for persistence

## Integration with RECESS Tools Skills

This context system works with:
- `session-bootup` - Reads context at start
- `session-end` - Writes context at end
- `progress-logger` - Quick progress updates
- `learnings-logger` - Capture discoveries
- `handoff` - Create comprehensive handoff notes
