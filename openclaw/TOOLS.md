# Tools

## Primary Tool: agent-browser

Browser automation CLI. Every browser interaction uses this tool.

### Command Quick Reference

| Category | Commands |
|----------|----------|
| Navigate | `open <url>`, `close` |
| Snapshot | `snapshot -i`, `snapshot -i -C`, `snapshot -c` |
| Interact | `click @ref`, `fill @ref "text"`, `type @ref "text"`, `select @ref "val"` |
| Check | `check @ref`, `uncheck @ref` |
| Get Info | `get text @ref`, `get url`, `get title`, `get value @ref` |
| Assert | `is visible <sel>`, `is enabled <sel>`, `is checked <sel>` |
| Wait | `wait --load networkidle`, `wait --text "Welcome"`, `wait <sel>` |
| Capture | `screenshot [path]`, `screenshot --annotate`, `screenshot --full` |
| Scroll | `scroll down [px]`, `scroll up [px]`, `scrollintoview @ref` |
| Find | `find role button click --name "Submit"`, `find text "Sign In" click` |
| Diff | `diff snapshot`, `diff screenshot --baseline before.png` |
| Debug | `eval <js>`, `get html <sel>` |
| Keys | `press Enter`, `press Tab`, `press Escape` |

### Rules

1. Always run `snapshot -i` before interacting with elements
2. Use `-C` flag for modern SPAs with custom clickable divs
3. Use `--annotate` flag on screenshots for visual reference
4. Use `--json` flag when parsing output programmatically
5. Chain with `&&` for sequential commands that don't need intermediate parsing

## Secondary Tools

- **Bash**: For running test scripts, file operations, generating reports
- **Read/Grep/Glob**: For reading test plans, searching project files
- **Write/Edit**: For writing test reports and documenting findings
