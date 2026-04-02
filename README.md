# ASC Registry

Plugin registry for [ASC CLI](https://github.com/tddworks/asc-cli). Browse and install plugins with:

```bash
asc plugins market list
asc plugins install --name asc-pro
```

## Available Plugins

| Plugin | Description | Author |
|--------|-------------|--------|
| [ASC Pro](https://github.com/tddworks/asc-registry) | Simulator streaming, interaction & tunnel sharing | tddworks |

## Submit Your Plugin

1. Build your plugin as a `.plugin` bundle (dylib + `manifest.json` + optional `ui/` scripts)
2. Package it as `<Name>.plugin.zip`
3. Fork this repo
4. Add your plugin entry to `registry.json`
5. Open a PR

### registry.json format

```json
{
  "plugins": [
    {
      "id": "my-plugin",
      "name": "My Plugin",
      "version": "1.0",
      "description": "What your plugin does",
      "author": "your-name",
      "repositoryURL": "https://github.com/you/my-plugin",
      "downloadURL": "https://github.com/tddworks/asc-registry/releases/download/v1.0/MyPlugin.plugin.zip",
      "categories": ["category1", "category2"]
    }
  ]
}
```

### Plugin bundle structure

Your `.plugin.zip` must extract to a `<Name>.plugin/` directory:

```
MyPlugin.plugin/
├── manifest.json    # {"name": "My Plugin", "version": "1.0", "server": "MyPlugin.dylib", "ui": [...]}
├── MyPlugin.dylib   # compiled dynamic library
└── ui/              # optional web UI scripts
```

### Plugin protocol

```swift
@_cdecl("ascPlugin")
public func ascPlugin() -> UnsafeMutableRawPointer {
    Unmanaged.passRetained(MyPlugin()).toOpaque()
}

public final class MyPlugin: NSObject, ASCPluginBase {
    public let name = "My Plugin"
    public var commands: [Any] { [] }
    public func configureRoutes(_ router: Any) { /* HTTP/WebSocket routes */ }
}
```

See [ASC Plugin Architecture](https://github.com/tddworks/asc-cli/blob/main/docs/features/plugins.md) for full documentation.