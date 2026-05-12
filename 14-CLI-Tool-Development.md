# 14-CLI-Tool-Development

## argparse (Built-in)

- Parse args: `import argparse; parser = argparse.ArgumentParser()`
- Add arg: `parser.add_argument("--name")`
- Parse: `args = parser.parse_args()`

## click Library

- Install: `pip install click`
- Decorator: `@click.command() def cli(name): ...`
- Options: `@click.option("--name")`

## Practice

- Build CLI tool: Script to manage files with options
- Subcommands: Use click groups for complex tools
