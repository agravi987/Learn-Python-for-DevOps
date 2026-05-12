# 13-Security-Automation

## paramiko Library

- Install: `pip install paramiko`
- SSH client: `import paramiko; client = paramiko.SSHClient()`
- Connect: `client.connect(host, username, password)`
- Run command: `stdin, stdout, stderr = client.exec_command("ls")`

## cryptography Library

- Install: `pip install cryptography`
- Encrypt/decrypt secrets
- Generate keys

## Practice

- SSH automation: Run commands on remote servers
- Secure configs: Encrypt sensitive data in YAML
