
```bash
curl -fsSL <https://code-server.dev/install.sh> | sh -s -- --dry-run
```

```bash
curl -fsSL <https://code-server.dev/install.sh> | sh
```

```python
sudo iptables -I INPUT 5 -i ens3 -p tcp --dport 80 -m state --state NEW,ESTABLISHED -j ACCEPT
sudo iptables -I INPUT 5 -i ens3 -p tcp --dport 443 -m state --state NEW,ESTABLISHED -j ACCEPT
sudo iptables -I INPUT 5 -i ens3 -p tcp --dport 8080 -m state --state NEW,ESTABLISHED -j ACCEPT
```

```bash
vim ~/.config/code-server/config.yaml
```

bind-addr: 0.0.0.0:8080
auth: password
password: <비밀번호>
cert: false