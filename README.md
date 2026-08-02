# paprika-3-mcp

A [Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction) server for Paprika 3. It lets an LLM like Claude create, update, list, and look up your recipes - including nutritional info - so you can use natural language for meal planning and building grocery lists.

## Example

<p align="center">
  <img src="docs/example.png" alt="MCP server running with Claude" />
</p>

## Features

### Resources

- Recipes, including nutritional info when present in Paprika
- Recipe photos (not yet implemented)

### Tools

- `create_paprika_recipe` - save a new recipe to your Paprika app
- `update_paprika_recipe` - modify an existing recipe
- `list_paprika_recipes` - list recipes by name, uid, and categories, with an optional text filter. Recipes with nutritional info in Paprika also show a calories/protein/carbs/fat summary.
- `get_paprika_recipe` - get full details for a single recipe (ingredients, directions, servings, prep/cook time, notes, nutritional info) by uid or by name

Open an issue on this repo to request a feature.

## Prerequisites

- A Mac, Linux, or Windows system
- [Paprika 3](https://www.paprikaapp.com/) installed with cloud sync enabled
- Your Paprika 3 username and password
- Claude or any LLM client with MCP tool support

## Installation

Prebuilt binaries are available on the [Releases](https://github.com/soggycactus/paprika-3-mcp/releases) page.

### macOS (Homebrew)

```bash
brew tap soggycactus/tap
brew install paprika-3-mcp
```

### Linux / Windows

1. Go to the [latest release](https://github.com/soggycactus/paprika-3-mcp/releases).
2. Download the archive for your OS and architecture:
   - `paprika-3-mcp_<version>_linux_amd64.zip` for Linux
   - `paprika-3-mcp_<version>_windows_amd64.zip` for Windows
3. Extract it:
   - Linux: `unzip paprika-3-mcp_<version>_<os>_<arch>.zip`
   - Windows: right-click the `.zip` file and select "Extract All", or use a tool like 7-Zip
4. Move the binary into a directory on your `$PATH`:
   - Linux: `sudo mv paprika-3-mcp /usr/local/bin/`
   - Windows: move `paprika-3-mcp.exe` to any folder on your `PATH` (e.g. `%USERPROFILE%\bin`)

### Verify the installation

```bash
paprika-3-mcp --version
```

Expected output:

```bash
paprika-3-mcp version v0.1.0
```

## Setting up Claude

If you haven't set up MCP before, read the [Claude Desktop quickstart](https://modelcontextprotocol.io/quickstart/user) first.

Add an entry to the `mcpServers` section of your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "paprika-3": {
      "command": "paprika-3-mcp",
      "args": [
        "--username",
        "<your paprika 3 username (usually email)>",
        "--password",
        "<your paprika 3 password>"
      ]
    }
  }
}
```

Restart Claude. The paprika-3-mcp tools should appear under the MCP tools icon:

![MCP server running with Claude](docs/install.png)

## Logs

The server writes structured logs (via Go's `slog`, rotated with `lumberjack`):

| OS            | Log file                                  |
| ------------- | ------------------------------------------ |
| macOS         | `~/Library/Logs/paprika-3-mcp/server.log` |
| Linux         | `/var/log/paprika-3-mcp/server.log`       |
| Windows       | `%APPDATA%\paprika-3-mcp\server.log`      |
| Other/unknown | `/tmp/paprika-3-mcp/server.log`           |

Logs rotate at 100MB, keep 5 backups, and are deleted after 10 days.

## License

MIT. See [LICENSE](./LICENSE). © 2025 [Lucas Stephens](https://github.com/soggycactus).
