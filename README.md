# thimble-plugins

Plugin registry for [thimble](https://github.com/inovacc/thimble) MCP tools.

## Install a Plugin

```bash
# From the registry (by name)
thimble plugin install docker
thimble plugin install kubernetes
thimble plugin install terraform

# From a URL
thimble plugin install https://example.com/my-plugin.json

# From a GitHub repo
thimble plugin install github.com/user/repo/plugin.json
```

## Available Plugins

| Plugin | Description | Tools |
|--------|-------------|-------|
| **docker** | Docker container management | `ctx_docker_ps`, `ctx_docker_logs`, `ctx_docker_images`, `ctx_docker_stats` |
| **kubernetes** | Kubernetes cluster operations | `ctx_k8s_pods`, `ctx_k8s_logs`, `ctx_k8s_describe`, `ctx_k8s_events` |
| **terraform** | Terraform infrastructure | `ctx_tf_plan`, `ctx_tf_state`, `ctx_tf_output`, `ctx_tf_validate` |

## Create Your Own Plugin

Plugins are JSON files with this structure:

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "tools": [
    {
      "name": "ctx_my_tool",
      "description": "What the tool does",
      "command": "my-command {{.arg}}",
      "input_schema": {
        "arg": {
          "type": "string",
          "description": "argument description",
          "required": true
        }
      }
    }
  ]
}
```

### Rules

- Tool names **must** start with `ctx_`
- Commands use Go `text/template` syntax for argument substitution
- Plugins are validated on load — invalid files are skipped with a warning
- Commands execute through thimble's security policies

### Submit to Registry

1. Fork this repo
2. Add your plugin JSON to `plugins/`
3. Add an entry to `registry.json`
4. Open a PR

## Manage Plugins

```bash
thimble plugin list      # list installed plugins
thimble plugin search    # browse registry
thimble plugin remove <name>  # uninstall
thimble plugin dir       # show plugins directory
```
