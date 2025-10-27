# Translating

## How to Contribute Translations

- Edit the relevant `.po` file and create a PR to submit changes.
- If adding a new language use `kiwimenu.pot` file as a template and save it to `po/xx.po` and then create a PR.
 

## Translation Status

| Language | Code | Status | Completion |
|----------|------|--------|------------|
| German | de | 🟢 Complete | 42/42 (100%) |
| Spanish | es | 🟢 Complete | 42/42 (100%) |
| Estonian | et | 🟢 Complete | 41/42 (97.6%) |
| Finnish | fi | 🟢 Complete | 41/42 (97.6%) |
| French | fr | 🟢 Complete | 42/42 (100%) |
| Italian | it | 🟢 Complete | 41/42 (97.6%) |
| Lithuanian | lt | 🟢 Complete | 41/42 (97.6%) |
| Latvian | lv | 🟢 Complete | 42/42 (100%) |
| Norwegian Bokmål | nb | 🟢 Complete | 41/42 (97.6%) |
| Dutch | nl | 🟢 Complete | 41/42 (97.6%) |
| Polish | pl | 🟢 Complete | 41/42 (97.6%) |
| Portuguese | pt | 🟢 Complete | 41/42 (97.6%) |
| Swedish | sv | 🟢 Complete | 41/42 (97.6%) |

*Note: The "1 untranslated message" in some languages is just the empty header (`msgid ""`), which is standard in PO files.*

## Note

> Current translations are machine-generated and may contain mistakes. Native speakers are welcome to review and improve them!


## Compiling translations for testing

The helper script compiles translations and produces a `locale/` folder for local testing.

Run the script to update `locale/`:

```bash
./compile-translations.sh
```

## Further Reading

- [GJS translations guide](https://gjs.guide/extensions/development/translations.html)


