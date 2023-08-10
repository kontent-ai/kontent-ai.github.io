# Kontent.ai GitHub developer resources

This is a source of Kontent.ai GitHub developer resources. Check out the [documentation site](https://kontent-ai.github.io) for more information.

## Spellcheck

This repository runs a Github Action for spellchecking the `.md` files. To configure the cSpell you can update the [cSpell.json](./cSpell.json) file.

You can run the spellcheck manually using the command:
```
cspell --config ./cSpell.json "**/*.md"
```
### Adding new words
The cSpell checks words against the [dictionaries](https://cspell.org/docs/dictionaries/), however some words might not be recognized (e.g. "CODEOWNERS"). To deal with errors caused by this you can update the configuration file by:

- adding the word into `words` array. Those words are considered correct and might be used for suggestions when misspelling a word.
- adding the word into `ignoreWords` array. Those words are just ignored.

### Ignoring patterns
By default, the entire document is checked for spelling. `ignoreRegExp` and `includeRegExp ` give us the ability to ignore or include patterns of text. If you find some patterns which should be skipped or included during spellcheck, add the regex to the [cSpell.json](./cSpell.json).

For more information about the cSpell you can head to the [documentation](https://cspell.org/).