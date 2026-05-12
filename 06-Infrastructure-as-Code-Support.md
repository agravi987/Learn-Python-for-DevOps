# 06-Infrastructure-as-Code-Support

## Ansible + Python

- Create dynamic inventories, custom modules, automation scripts

## Jinja2 Templates

- Install: `pip install jinja2`
- Render: `from jinja2 import Template; t = Template("Hello {{name}}"); t.render(name="World")`

## YAML Parsing

- Use PyYAML for Ansible playbooks

## Practice

- Generate inventory: Use Python to query cloud APIs and output YAML
- Custom module: Write Python script for Ansible module
