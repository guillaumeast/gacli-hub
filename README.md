# 🧩 GACLI Hub

**Declarative module registry for `GACLI`**  
→ Designed to index and share custom `CLI extensions`

---

## ✨ What is it?

`gacli-hub` is a lightweight registry of **[GACLI](https://github.com/guillaumeast/gacli)** `modules`.

- It does **NOT** contain the actual code of `modules`.
- It only contains **`YAML` descriptors** that allow `GACLI` to download and install `modules` via `curl | tar`.

This design makes it:
- 🧼 Clean (no nested git repos)
- 📦 Scalable (modules live in their own repositories)
- 🪄 Simple to parse and automate

---

## 🧠 How does it work?

Each module is described by a single **YAML** file: `modules/<module>.yaml`.

```yaml
name: "module_name"
version: "1.0.0"
stable: true
description: "Useful tool for..."
repo_url: "https://github.com/<user>/<module>"
module_url: "https://github.com/<user>/<module>/archive/refs/heads/main.tar.gz"
```

Then `GACLI` can install it via:

```bash
gacli add <module>
```

`GACLI` will:
- Fetch and parse the `my_module.yaml` from this repo
- Download the compressed archive from your repo `module_url`
- Extract it into `~/.gacli/modules/user_modules/<module>/`
- Install `module` dependencies declared in `module/tools.yaml`
- Source `main.zsh` script
- Make `module` commands available (exposed by `get_commands`)

---

## 📤 How to publish a `module`?

1. Create a new repo for your `module` (ex: `my_module`)
2. Make sure it has a valid structure:

```bash
my_module/
├── main.zsh          # Module entry point (can expose commands via get_commands)
├── tools.yaml        # Dependencies (can be empty but file must exist)
└── ...
```

3. Generate a `.tar.gz` download URL (`GitHub` does it by default)
4. Add a descriptor in this repo under `modules/my_module.yaml`

Example:

```yaml
name: "my_module"
version: "0.1.0"
stable: false
description: "A custom dev helper"
repo_url: "https://github.com/my_username/my_module"
module_url: "https://github.com/my_username/my_module/archive/refs/heads/main.tar.gz"
```

5. Commit and push a PR.

---

## 📎 Related

- 🔧 [`GACLI`](https://github.com/guillaumeast/gacli): Modular CLI to easily manage your dev setup and tools

---

Made with ❤️ by [guillaumeast](https://github.com/guillaumeast)
