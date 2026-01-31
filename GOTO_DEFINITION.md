# Go to Definition Support for Gherkin Steps

This fork adds **Go to Definition** functionality for Gherkin steps in VS Code. When you Ctrl+Click (or F12) on a Gherkin step in a `.feature` file, the extension will now navigate to the corresponding keyword definition in your Robot Framework `.resource` files.

## Features Added

### 1. **Go to Definition** 
- **Ctrl+Click** or press **F12** on any Gherkin step to jump to its implementation
- Searches for keyword definitions in `.resource` files within `steps/` directories
- Supports both exact matches and pattern matching for steps with variables

### 2. **Hover Information**
- **Hover** over Gherkin steps to see:
  - Keyword signature
  - File location with clickable link
  - Arguments and their types
  - Tags (if any)
  - Documentation (if available)

### 3. **Variable Support**
- Handles Robot Framework variables like `${variable}` in step definitions
- Matches steps with quoted strings and variable placeholders
- Fuzzy matching for similar keywords

## How It Works

The extension:
1. **Caches keyword definitions** from `.resource` files in your workspace
2. **Parses Robot Framework syntax** to extract keyword names, arguments, and documentation
3. **Matches Gherkin steps** to keyword definitions using multiple strategies:
   - Exact text matching (case-insensitive)
   - Pattern matching for variables (e.g., `${name}` becomes a capture group)
   - Fuzzy matching for partial word matches

## Example

Given this Gherkin step:
```gherkin
Given a blog post named "My First Post" with Markdown body
```

And this Robot Framework keyword definition in a `.resource` file:
```robotframework
*** Keywords ***
a blog post named "${name}" with Markdown body
    [Tags]    gherkin:step
    [Arguments]    ${body}
    
    Log    ${name}
    Log    ${body}
```

You can now:
- **Ctrl+Click** on the step to navigate to the keyword definition
- **Hover** to see the keyword signature and documentation

## File Structure Support

The extension looks for keyword definitions in:
- `**/steps/**/*.resource` (primary location for step definitions)
- `**/*.resource` (fallback for any `.resource` files in the workspace)

## Installation

This enhanced version works as a drop-in replacement for the original RobotCode Gherkin extension. Simply:

1. Install the modified extension
2. Open a workspace with `.feature` files and corresponding `.resource` files
3. Start using Go to Definition and Hover features!

## Technical Details

### Architecture
- **Definition Provider**: Implements VS Code's `vscode.DefinitionProvider` interface
- **Hover Provider**: Implements VS Code's `vscode.HoverProvider` interface  
- **Caching**: Intelligent caching with 30-second timeout to balance performance and freshness
- **Pattern Matching**: Advanced regex patterns to handle Robot Framework variable substitution

### Performance
- **Lazy Loading**: Only parses files when needed
- **Cached Results**: Avoids re-parsing files on every request
- **Efficient Search**: Uses Maps for O(1) keyword lookups
- **Background Processing**: Non-blocking file parsing

This enhancement significantly improves the developer experience when working with Gherkin feature files in Robot Framework projects by providing the navigation features developers expect from modern IDEs.