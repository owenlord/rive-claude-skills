# Rive Embed Skill

A Claude Code skill for embedding Rive animations in web and React applications.

## What it does

This skill provides quick reference and code examples for:
- Vanilla JavaScript Rive embedding
- React component integration with `@rive-app/react-canvas`
- State machine control and inputs
- Layout and responsive sizing
- Troubleshooting common issues
- Performance best practices

## Usage

In Claude Code, this skill will automatically activate when you ask questions like:
- "How do I embed a Rive animation in React?"
- "How can I control a state machine input?"
- "My Rive animation isn't showing, what should I check?"
- "How do I fire a trigger in Rive?"

## Skill Location

```
/home/claude/rive-embed/
├── SKILL.md           # Main skill documentation
└── evals/
    └── evals.json     # Test cases for validation
```

## Testing the Skill

To test this skill with the skill-creator:

```bash
# From Claude Code, you can run evaluations
# Ask: "Can you run the evals for the rive-embed skill?"
```

## What's Covered

### Web (Vanilla JS)
- Basic setup with CDN or NPM
- Configuration options
- Playback control
- State machine inputs
- Layout and sizing
- Cleanup

### React
- `useRive` hook
- `useStateMachineInput` hook
- Component patterns
- Responsive sizing
- Lifecycle management

### Common Scenarios
- Loading from URL or file
- Controlling boolean, number, and trigger inputs
- Multiple instances on one page
- Responsive canvas sizing
- Troubleshooting

## Next Steps

You can now:
1. **Use the skill** - Ask Claude Code questions about Rive embedding
2. **Test it** - Run the evals to see how well it performs
3. **Improve it** - Add more examples or update based on new Rive releases
4. **Extend it** - Add sections for Vue, Svelte, or other frameworks

## Installing in Claude Code

To make this skill available in Claude Code:

1. Copy this directory to your skills folder:
   ```bash
   cp -r /home/claude/rive-embed /path/to/your/skills/folder/
   ```

2. Or reference it directly in your projects

## Keeping it Updated

Since Rive APIs can change, the skill includes a note to use web search for the latest documentation when needed. The core APIs are stable, but always verify against:
- https://rive.app/community/doc/web-js/docvS86uz10s
- https://github.com/rive-app/rive-react
