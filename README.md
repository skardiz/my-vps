```markdown
# Памятка: раскат нового VPS

## 1. Обновить inventory

`inventory.ini`:

```
[vps_bootstrap]
NEW_IP ansible_user=root

[vps]
NEW_IP ansible_user=deploy
```

## 2. Bootstrap (создать deploy и базу)

С локальной машины:

```
cd ~/my-vps
ansible-playbook site-bootstrap.yml
```

## 3. Основной деплой сервисов

```
ansible-playbook site.yml
```

(ход в группу `vps`, всё ставится под пользователем `deploy`)

## 4. (Опционально) перенести VPN‑данные

Со старого сервера:

```
scp -r deploy@OLD_IP:/opt/amneziawg-data ./amnezia-backup
scp -r ./amnezia-backup deploy@NEW_IP:/opt/amneziawg-data
ansible-playbook site.yml
```

## 5. После переезда

- Web UI AmneziaWG: `http://NEW_IP/`
- Traefik dashboard: `http://NEW_IP:8080/`
- VPN endpoint в клиентах: `NEW_IP:51820`
```
