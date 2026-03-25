# agent-browser Command Reference

## Core Commands

```bash
agent-browser open <url>              # Navigate
agent-browser click <sel>             # Click element
agent-browser fill <sel> <text>       # Clear and fill input
agent-browser type <sel> <text>       # Type into element
agent-browser press <key>             # Press key (Enter, Tab, Escape)
agent-browser hover <sel>             # Hover
agent-browser select <sel> <val>      # Select dropdown option
agent-browser check/uncheck <sel>     # Checkbox
agent-browser scroll down [px]        # Scroll
agent-browser scrollintoview <sel>    # Scroll element into view
agent-browser screenshot [path]       # Screenshot (--full, --annotate)
agent-browser snapshot                # Accessibility tree with refs (best for AI)
agent-browser snapshot -i             # Interactive elements only
agent-browser snapshot -i -C          # Include custom clickable divs (SPAs)
agent-browser eval <js>               # Run JavaScript
agent-browser close                   # Close browser
```

## Get Info

```bash
agent-browser get url                 # Current URL
agent-browser get title               # Page title
agent-browser get text <sel>          # Element text content
agent-browser get value <sel>         # Input value
agent-browser get count <sel>         # Count matching elements
agent-browser get html <sel>          # innerHTML
agent-browser get attr <sel> <attr>   # Attribute value
```

## Check State (Assertions)

```bash
agent-browser is visible <sel>        # Returns true/false
agent-browser is enabled <sel>
agent-browser is checked <sel>
```

## Wait (Implicit Assertions — timeout = failure)

```bash
agent-browser wait --load networkidle        # Wait for network quiet
agent-browser wait --text "成功"             # Wait for text to appear
agent-browser wait --url "**/dashboard"      # Wait for URL pattern
agent-browser wait <selector>               # Wait for element to be visible
agent-browser wait <selector> --state hidden # Wait for element to disappear
agent-browser wait --fn "window.ready===true" # Wait for JS condition
agent-browser wait 2000                      # Wait fixed ms
```

## Find Elements (Semantic Locators)

```bash
agent-browser find role button click --name "Submit"
agent-browser find text "登录" click
agent-browser find label "Email" fill "test@test.com"
agent-browser find placeholder "搜索" fill "keyword"
agent-browser find testid "submit-btn" click
agent-browser find first ".item" click
agent-browser find nth 2 "a" text
```

## Session

```bash
# Persistent profile (full browser state)
agent-browser --profile ~/.kweaver-profile open <url>

# Auto save/restore cookies + localStorage (recommended)
agent-browser --session-name kweaver-staging open <url>
# or: export AGENT_BROWSER_SESSION_NAME=kweaver-staging

# Encryption at rest
export AGENT_BROWSER_ENCRYPTION_KEY=<64-char-hex>  # AES-256-GCM
```

## Diff

```bash
agent-browser diff screenshot --baseline before.png
agent-browser diff snapshot
```

## Tips

- Use `snapshot -i` refs (`@e1`, `@e2`) — never guess CSS selectors
- Use `-C` flag for SPAs with custom clickable divs
- Chain with `&&` when intermediate output not needed
- `--json` flag for programmatic output parsing
