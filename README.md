# Gogh Themes for Windows Terminal

**1,250 terminal colour schemes from [Gogh](https://github.com/Gogh-Co/Gogh), converted into Windows Terminal colour schemes and ready to import.**

![themes](https://img.shields.io/badge/themes-1%2C250-8DDC20?style=flat-square)
![Windows Terminal](https://img.shields.io/badge/Windows%20Terminal-settings.json-0C0C0C?style=flat-square)
![conversion license](https://img.shields.io/badge/conversion-MIT-blue?style=flat-square)

Gogh's themes are primarily maintained for terminal emulators such as GNOME Terminal, iTerm2, and Kitty. This project converts the Gogh palette data into Windows Terminal colour-scheme JSON, so you can use these themes on Windows without running Gogh's installation scripts.

## Contents

- [What's included](#whats-included)
- [Quick start](#quick-start)
- [Where is `settings.json`?](#where-is-settingsjson)
- [Applying a theme to a profile](#applying-a-theme-to-a-profile)
- [Performance considerations](#performance-considerations)
- [Windows Terminal built-in scheme names](#windows-terminal-built-in-scheme-names)
- [Readability and low-contrast themes](#readability-and-low-contrast-themes)
- [Conversion and validation](#conversion-and-validation)
- [Credits and licensing](#credits-and-licensing)

## What's included

```text
README.md     # Project documentation
converted/    # 1,250 colour schemes, one JSON file per theme
```

Each file in `converted/` contains one Windows Terminal colour scheme named after the theme. Filenames use slugified names, for example `dracula.json`, `tokyo-night.json`, and `catppuccin-mocha.json`.

A scheme contains the 16 ANSI colours plus `background`, `foreground`, `cursorColor`, and `selectionBackground`. For example:

```json
{
  "name": "Grape",
  "black": "#2D283F",
  "red": "#ED2261",
  "green": "#1FA91B",
  "yellow": "#8DDC20",
  "blue": "#487DF4",
  "purple": "#8D35C9",
  "cyan": "#3BDEED",
  "white": "#9E9EA0",
  "brightBlack": "#59516A",
  "brightRed": "#F0729A",
  "brightGreen": "#53AA5E",
  "brightYellow": "#B2DC87",
  "brightBlue": "#A9BCEC",
  "brightPurple": "#AD81C2",
  "brightCyan": "#9DE3EB",
  "brightWhite": "#A288F7",
  "background": "#171423",
  "foreground": "#9F9FA1",
  "cursorColor": "#A288F7",
  "selectionBackground": "#493D70"
}
```

## Quick start

Open your Windows Terminal `settings.json`. See [Where is `settings.json`?](#where-is-settingsjson) if you need help locating it. Find the top-level `"schemes"` array.

```jsonc
{
    // ...
    "schemes": [
        // Add colour schemes here
    ]
}
```

### Add one theme

1. Browse `converted/` and open the JSON file for the theme you want.
2. Copy the JSON object and paste it inside the `"schemes"` array.
3. Ensure commas are valid JSONC separators, then save the file.

### Add a hand-picked set

Repeat the steps above for each theme you want to keep. This is the recommended approach for most users: it keeps the configuration manageable while allowing you to add or remove schemes later.

The scheme names match Gogh's theme names. You can preview themes in the [Gogh repository](https://github.com/Gogh-Co/Gogh) and then import the matching file.

### Import every theme

You can combine the individual files into a single JSON object containing a `"schemes"` array:

```bash
python3 - <<'PY'
import glob
import json
from pathlib import Path

files = sorted(glob.glob("converted/*.json"))
schemes = [json.loads(Path(filename).read_text()) for filename in files]

print(json.dumps({"schemes": schemes}, indent=2))
PY
```

Run the command from the repository root and redirect the output to a file if desired:

```bash
python3 - <<'PY' > all-schemes.json
import glob
import json
from pathlib import Path

files = sorted(glob.glob("converted/*.json"))
schemes = [json.loads(Path(filename).read_text()) for filename in files]
print(json.dumps({"schemes": schemes}, indent=2))
PY
```

The resulting file wraps the schemes in a `"schemes"` property. Copy the value of that property—the array of scheme objects—into the existing `"schemes"` array in `settings.json`. Do not replace the whole settings file with `all-schemes.json`, because it contains only the schemes wrapper, not your profiles and other settings.

Review [Performance considerations](#performance-considerations) and [Windows Terminal built-in scheme names](#windows-terminal-built-in-scheme-names) before importing all schemes.

After saving `settings.json`, Windows Terminal normally reloads the configuration automatically. If a new scheme does not appear, reopen Settings or start a new tab.

## Where is `settings.json`?

| Installation | Typical path |
|---|---|
| Windows Terminal (Microsoft Store) | `%LOCALAPPDATA%\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json` |
| Windows Terminal Preview | `%LOCALAPPDATA%\Packages\Microsoft.WindowsTerminalPreview_8wekyb3d8bbwe\LocalState\settings.json` |
| Unpackaged installation | `%LOCALAPPDATA%\Microsoft\Windows Terminal\settings.json` |

The simplest method is to open Windows Terminal and press **Ctrl + ,**, then select **Open JSON file**. You can also open the Settings menu while holding **Alt** and select **Settings**.

Paths can vary by installation method and version; opening the JSON file from Settings avoids guessing the location.

## Applying a theme to a profile

Set `"colorScheme"` on an individual profile, or under `"profiles.defaults"` to apply it to profiles by default:

```jsonc
{
    "profiles": {
        "defaults": {
            "colorScheme": "Dracula"
        },
        "list": [
            {
                "name": "Ubuntu (WSL 2)",
                "colorScheme": "Tokyo Night",
                "guid": "{...}"
            }
        ]
    }
}
```

The value of `"colorScheme"` must match the scheme's `"name"` exactly. For example, `Grape`, `Dracula`, `Catppuccin Mocha`, `Tokyo Night`, and `Nord`. Once imported, a scheme can also be selected through the Windows Terminal Settings UI.

## Performance considerations

Importing all 1,250 schemes makes `settings.json` larger. In this repository, the combined schemes block is approximately **940 KB**, compared with a few kilobytes for a configuration containing only a handful of schemes.

A larger configuration requires more data to be read and parsed, but the practical effect on startup time and Settings responsiveness depends on the system and Windows Terminal version. This project has not established a universal performance impact through benchmarking.

Most users only need a small selection of themes. Keeping the schemes you actually use makes the configuration easier to manage and avoids adding data you do not need. Since `converted/` contains one file per theme, you can import additional schemes later.

## Windows Terminal built-in scheme names

Windows Terminal includes built-in colour schemes. Names that overlap with built-in schemes can behave differently from ordinary user-defined names; the exact result can depend on the scheme contents and Windows Terminal version. Do not assume that a same-named custom scheme will always replace the built-in scheme.

The following Gogh theme names overlap with names in the Windows Terminal built-in scheme list used when this README was prepared:

| Gogh theme name | Built-in name |
|---|---|
| `Campbell` | `Campbell` |
| `CGA` | `CGA` |
| `Dark+` | `Dark+` |
| `Dimidium` | `Dimidium` |
| `One Half Dark` | `One Half Dark` |
| `One Half Light` | `One Half Light` |
| `Solarized Dark` | `Solarized Dark` |
| `Solarized Light` | `Solarized Light` |

If you want to avoid ambiguity, rename the imported scheme by changing its `"name"` property—for example, `Campbell` to `Campbell (Gogh)`—and use that new name in `"colorScheme"`. Test the result in your installed Windows Terminal version, especially when importing schemes with names that already exist.

The built-in scheme list can change between Windows Terminal releases. For the version you use, consult the [`defaults.json`](https://github.com/microsoft/terminal/blob/main/src/cascadia/TerminalApp/defaults.json) file in the Microsoft Terminal repository.

## Readability and low-contrast themes

The palette colours are copied from Gogh without colour correction. As a result, some themes may have poor contrast or be difficult to read. For example, a dark foreground on a nearly black background may be hard to distinguish. These cases originate in the source palette rather than from colour conversion.

Windows Terminal provides `adjustIndistinguishableColors`, which can adjust foreground colours that are too close to the background:

```jsonc
{
    "profiles": {
        "defaults": {
            "adjustIndistinguishableColors": "always"
        }
    }
}
```

Supported values are `never`, `indexed`, and `always`. `indexed` applies the adjustment to indexed ANSI colours; `always` also allows adjustment of the default foreground colour. This can improve legibility, but it may change the rendered appearance of a theme. For palette evaluation, compare the theme with this setting disabled and enabled.

## Conversion and validation

The source theme definitions are maintained in the [`Gogh-Co/Gogh`](https://github.com/Gogh-Co/Gogh) repository, with one YAML file per theme. The installer command published on Gogh's website is a bootstrapper; the palette data used for this conversion comes from the repository.

The conversion maps Gogh's palette fields to Windows Terminal's scheme fields:

| Windows Terminal property | Gogh source |
|---|---|
| `name` | `name` |
| `black` through `white` | `color_01` through `color_08` |
| `brightBlack` through `brightWhite` | `color_09` through `color_16` |
| `background` | `background`, falling back to `color_01` |
| `foreground` | `foreground`, falling back to `color_08` |
| `cursorColor` | `cursor`, falling back to `color_08` |
| `selectionBackground` | Derived; see below |

Gogh labels `color_06` as magenta; Windows Terminal's corresponding scheme property is named `purple`. Colour values are normalised to uppercase `#RRGGBB` notation without rescaling or blending the source palette. Gogh's `variant` metadata is not carried into the Windows Terminal scheme.

### Selection background

Gogh does not provide a `selectionBackground` value for every theme. For themes that match an entry in [iTerm2-Color-Schemes](https://github.com/mbadolato/iTerm2-Color-Schemes) by their 19 palette values, the corresponding selection colour from that project is reused. For the remaining themes, a selection colour is derived by blending the theme's background 25% of the way toward its foreground.

A contrast guard is applied to avoid selection colours that are too close to the foreground. This is a practical fallback rather than a guarantee that every selection colour will meet every user's contrast preferences; review the result in your terminal if selection visibility is important.

### Validation

The conversion output was checked against the source YAML files: all 19 palette values across the 1,250 themes matched, with **0 reported mismatches** and **0 duplicate theme names** in the generated set.

You can check that every generated JSON file parses successfully from the repository root:

```bash
python3 - <<'PY'
import json
from pathlib import Path

files = sorted(Path("converted").glob("*.json"))
errors = []

for path in files:
    try:
        scheme = json.loads(path.read_text(encoding="utf-8"))
        if not isinstance(scheme, dict):
            errors.append(f"{path}: expected a JSON object")
        elif not isinstance(scheme.get("name"), str) or not scheme["name"]:
            errors.append(f"{path}: missing or invalid scheme name")
    except (OSError, json.JSONDecodeError) as exc:
        errors.append(f"{path}: {exc}")

if errors:
    print("\n".join(errors))
    raise SystemExit(1)

print(f"Validated {len(files)} JSON scheme files.")
PY
```

This checks JSON syntax and the presence of a non-empty scheme name. It does not independently reproduce the source-palette comparison or verify the visual contrast of each theme.

## Credits and licensing

- **Theme palettes:** [Gogh](https://github.com/Gogh-Co/Gogh) (`Gogh-Co/Gogh`), dual-licensed under the [MIT License](https://opensource.org/licenses/MIT) and [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
- **Some selection colours:** [iTerm2-Color-Schemes](https://github.com/mbadolato/iTerm2-Color-Schemes) by Mark Badolato, MIT-licensed.
- **Conversion and project files:** MIT-licensed. See the repository's `LICENSE` file.

Theme colour values originate from their respective authors. This project is not affiliated with or endorsed by Microsoft, Gogh, or the Windows Terminal team.
