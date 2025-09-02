# Ansible quick deploy (Nginx config + reload)

- deploy/site.yml:
```
- hosts: prod
  become: true
  tasks:
    - name: Push nginx conf
      copy: src=nginx/conf.d/ dest=/srv/app/nginx/conf.d/ mode=0644
    - name: Test nginx
      command: docker exec nginx nginx -t
    - name: Reload nginx
      command: docker exec nginx nginx -s reload
```
- Inventory:
```
[prod]
vps ansible_host=1.2.3.4 ansible_user=root
```
- Run:
```
ansible-playbook -i deploy/inventory deploy/site.yml
```
