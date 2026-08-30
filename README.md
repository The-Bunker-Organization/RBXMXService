# RBXMXService

> [!IMPORTANT]
> **RBXMXService is currently in Beta!**
> Please report bugs or issues on our [Discord Server](https://canary.discord.com/invite/MvVBbftUYm).
>
> For urgent questions, contact us at **[thebunkerproject@waifu.club](mailto:thebunkerproject@waifu.club)**.

## About

**RBXMXService** is a runtime **RBXMX loader** that allows you to load `.rbxmx` files directly inside a Roblox environment.

It is designed to work with a `loadstring`/compiler environment,allowing RBXMX content to be loaded without manually converting the file beforehand.

### Features

* Runtime `.rbxmx` loading
* Works through a Roblox `loadstring`/compiler environment
* Lua-based loader
* Currently in **Beta**

## Building

The project uses a `.lua` file that can be loaded directly through a `loadstring` environment directly without building it.

### Using HTTPService
If your environment supports `loadstring` or any bytecode compiler (a example being something like Vlua,VluaU or Sensation Loadstring) plus having HTTPService enabled, you can load the provided `.lua` file through the link.

```lua
local HttpService = game:GetService("HttpService")
local loader = loadstring(HttpService:GetAsync("https://raw.githubusercontent.com/The-Bunker-Organization/RBXMXService/refs/heads/main/src/Runtime.luau"))()
local link = HttpService:GetAsync("ur link to .rbxmx")
local instance = loader(link)

instance[1].Parent = workspace --put the first instance into the workspace
```

### Using Offline
If your environment supports `loadstring` or any bytecode compiler (a example being something like Vlua,VluaU or Sensation Loadstring) but it doesnt have HTTPService enabled, you can also load the code via the raw XML format

```lua
local HttpService = game:GetService("HttpService")
local loader = loadstring(HttpService:GetAsync("https://raw.githubusercontent.com/The-Bunker-Organization/RBXMXService/refs/heads/main/src/Runtime.luau"))()
local link = [====[
put the stuff inside ur rbxmx here
]====]
local instance = loader(link)

instance[1].Parent = workspace --put the first instance into the workspace
```

### Using RBXM
If you wanna just use the module,you can just [download the Releases](https://github.com/The-Bunker-Organization/RBXMXService/releases) or run Rojo on the "src" folder


## Status

| Feature         | Status            |
| --------------- | ----------------- |
| Lua loader      | 🟢 Available      |
| Rojo loader / RBXM Module      | 🟢 Available      |
| Argon build     | 🟡 Working onto it |

## Contributing

Found a bug or have an improvement?

* Open an issue on the repository.
* For urgent questions, email **[thebunkerproject@waifu.club](mailto:thebunkerproject@waifu.club)**.

## License
## License

RBXMXService is free and open-source software licensed under the **GNU General Public License v3.0 (GPL-3.0)**.
You are free to use, modify, and redistribute the software under the terms of the license.

See the [LICENSE](LICENSE) file for the full license text.
