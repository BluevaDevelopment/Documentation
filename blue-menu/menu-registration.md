# Menu Registration

Menu files are activated through `plugins/BlueMenu/config.yml`. Java and Bedrock menus each have a list of entries in the format `menu_key;file_name.yml`. The plugin reads those lists during startup and reloads to know which files to load.

## Where to register
The default config includes placeholders for both platforms:

```yaml
java_menus:
  - 'menu;chest_example.yml'
  - 'slots;slots_helper.yml'
  - 'conditions_example;conditions_example.yml'

bedrock_menus:
  - 'menu;modal_example.yml'
  - 'bedrock_menu;simple_example.yml'
  - 'customform;custom_example.yml'
  - 'simple_conditions;conditions_simple_example.yml'
  - 'custom_conditions;conditions_custom_example.yml'
```

- The text before the semicolon (`menu`, `slots`, etc.) is the name players use with `/bm open <platform> <menu>`.
- The filename after the semicolon must exist under `plugins/BlueMenu/menus/java/` or `/bedrock/`.

## Adding a new menu
1. Create the YAML file in the appropriate folder (Java or Bedrock).
2. Add a new entry to `java_menus` or `bedrock_menus` using `alias;file.yml`.
3. Run `/bm reload` or restart to load the new menu.

## Editing via the web editor
The web editor exposes `config.yml` as well. After opening `/bm editor` and confirming the session, you can edit the config list directly from the browser if you prefer not to use a text editor.