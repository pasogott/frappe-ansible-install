# How to use
1. Ensure ansible is installed
2. change YOURHOST and YOURUSER in hosts.yml
3. Configure vars in vars.yml
4. `ansible-galaxy install -r requirements.yml`
5. `pip install -r requirements.txt` (use virtualenv or use --break-system-packages)
6. `ansible-playbook playbook.yml -i hosts.yml`
