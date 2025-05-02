# Автоматизация развертывания Kubernetes-кластера с помощью Ansible

## Описание

Этот репозиторий содержит набор Ansible playbook-ов и конфигурационных файлов для автоматизированного развертывания Kubernetes-кластера на базе Debian 12 (bookworm). Поддерживается установка различных контейнерных движков (CRI-O, containerd, Docker+cri-dockerd), настройка HA (high availability) control-plane с помощью keepalived и haproxy, а также деплой тестового nginx-приложения.

## Структура репозитория

- `playbook.yml` — основной Ansible playbook, управляющий всем процессом установки.
- `inventory.ini` — инвентарный файл с описанием всех узлов кластера.
- `branch_var.yml` — переменная выбора контейнерного движка (`branch_mode`: 0 — cri-o, 1 — containerd, 2 — Docker+cri-dockerd).
- `network_tasks.yml`, `add_repositories.yml`, `install_dependencies.yml`, `k8s_prereqs.yml` — вспомогательные задачи для подготовки системы.
- `cri-o.yml`, `containerd.yml`, `cri-dockerd.yml` — роли для установки соответствующих контейнерных движков.
- `cluster.yml` — задачи для развертывания HA-кластера Kubernetes.
- `all_in_one.yml` — задачи для развертывания одновузлового кластера.
- `nginx.yml` — манифест для деплоя тестового nginx.
- `inventory.ini` — пример структуры инвентаря.

## Требования

- Ansible 2.10+
- Доступ по SSH к каждому узлу с правами sudo
- Debian 12 (bookworm) на всех узлах

## Быстрый старт

1. Отредактируйте `inventory.ini` под вашу инфраструктуру.
2. Выберите нужный контейнерный движок в `branch_var.yml` (`branch_mode`).
3. Запустите основной playbook:
   ```bash
   ansible-playbook -i inventory.ini playbook.yml
   ```
4. После завершения деплоя, nginx будет доступен на NodePort 30080 на node1.

## k8s_cluster_role

Роль можно установить с помощью `ansible-galaxy`
```bash
ansible-galaxy role install Alvald1.k8s_cluster_role
```
Репозиторий с ролью [k8s_cluster_role](https://github.com/Alvald1/k8s_cluster_role)

## Примечания

- Для HA-кластера используйте `branch_mode: 2` (Docker+cri-dockerd).
- Для одновузлового кластера используйте `branch_mode: 0` или `1`.
- Все действия выполняются с помощью Ansible, ручное вмешательство не требуется.

## Авторы

- alvald1

