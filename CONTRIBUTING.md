We prefer using YAML rather than JSON to specify the grammar for readability.
Unfortunately, VS Code only supports the JSON format, so we need to convert YAML to JSON for it to be effective.
We use [js-yaml](https://github.com/nodeca/js-yaml) for that purpose.

```sh
brew install fswatch
npm i
```

```sh
# or use whatever that suits your workflow
fswatch ./syntaxes/*.yaml | while read -r changed; do
	filename=$(basename "${changed%.*}")
	npx js-yaml "./syntaxes/$filename.yaml" > "./syntaxes/$filename.json"
done
```

## Syntax

Order matters.
The first pattern that matches will be applied.
As a rule of thumb, more specific patterns go first.
Best to avoid pattern overlaps whenever possible to reduce bugs related to ordering.

Below is a reasonable layout of patterns.

```
comment		^(;|\(;)
constant
	int		^[0-9-]
	bv		^(#b|#x)
	bool	true | false
	const	^\$
keyword
function	^\([a-z]
type		^[A-Z]
variable
```
