# Система мониторинга на Flask + Prometheus + Grafana + Ansible

Этот проект демонстрирует настройку системы мониторинга для простого Flask-приложения с использованием Prometheus для сбора метрик, Grafana для визуализации и Ansible для автоматизации развертывания.

---

## Зависимости

- Docker (19.03+)
- Docker Compose (1.27+)
- Python 3.8+
- Ansible 2.9+ (для автоматизации)

---

## Структура проекта

```
monitoring-project/
├── app/                    # Flask-приложение
│   ├── app.py              # Исходный код приложения
│   ├── Dockerfile          # Dockerfile для сборки приложения
│   └── requirements.txt    # Зависимости приложения
├── prometheus/             # Конфигурация Prometheus
│   ├── prometheus.yml      # Основной конфиг Prometheus
│   └── alert_rules.yml     # Правила оповещений
├── grafana/                # Конфигурация Grafana
│   └── provisioning/       # Автоматическая настройка
│       └── datasources/    # Источники данных
├── alertmanager/           # Конфигурация AlertManager
│   └── alertmanager.yml    # Основной конфиг AlertManager
├── ansible/                # Ansible-плейбуки и конфигурация
│   ├── deploy.yml          # Плейбук для развертывания
│   ├── inventory.ini       # Инвентарь хостов
│   └── group_vars/         # Переменные
│       └── all.yml         # Общие переменные
├── docker-compose.yml      # Конфигурация Docker Compose
└── README.md               # Документация проекта
```

---

## Быстрый старт

### Запуск через Docker Compose

```bash
git clone https://github.com/Valera1488-s/devops-monitoring-demo.git
cd devops-monitoring-demo
docker compose up -d
```

### Запуск через Ansible

```bash
cd ansible
ansible-playbook -i inventory.ini deploy.yml
```

---

## Доступ к сервисам

- Flask-приложение: http://localhost:5000
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000 (логин: admin, пароль: admin)
- AlertManager: http://localhost:9093

---

## Описание компонентов

### Flask-приложение
- Демонстрационное приложение с метриками для Prometheus.
- Метрики доступны по `/metrics`.
- Эндпоинты для тестирования: `/`, `/slow`, `/error`.

### Prometheus
- Система сбора метрик.
- Конфиг: `prometheus/prometheus.yml`.
- Правила алертов: `prometheus/alert_rules.yml`.

### Grafana
- Визуализация метрик.
- Источник данных Prometheus настраивается автоматически.
- Можно создавать дашборды для анализа метрик.

### AlertManager
- Управление оповещениями.
- Конфиг: `alertmanager/alertmanager.yml`.
- Можно интегрировать с email, Slack и др.

### Ansible
- Автоматизация развертывания всей инфраструктуры.
- Плейбук: `ansible/deploy.yml`.
- Инвентарь: `ansible/inventory.ini`.
- Переменные: `ansible/group_vars/all.yml`.

---

## Пример правила алерта (prometheus/alert_rules.yml)
```yaml
groups:
  - name: example
    rules:
      - alert: InstanceDown
        expr: up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Инстанс {{ $labels.instance }} недоступен"
          description: "Экземпляр {{ $labels.instance }} не отвечает более 1 минуты."
```

---

## Лучшие практики DevOps/AIOps
- **Инфраструктура как код (IaC):** Вся инфраструктура описана в виде кода, что обеспечивает воспроизводимость и масштабируемость.
- **Автоматизация:** Использование Ansible и Docker Compose для быстрого развертывания.
- **Безопасность:** Не храните секреты в репозитории, используйте `.gitignore` и секрет-хранилища.
- **Документирование:** README содержит всю необходимую информацию для быстрого старта и поддержки.
- **Масштабируемость:** Легко добавить новые сервисы или метрики.

---

## Контакты и поддержка
- Вопросы и предложения: через Issues на GitHub
- Документация по каждому компоненту — в соответствующих папках

---

**Этот проект можно использовать как шаблон для быстрого старта мониторинга в любых DevOps/AIOps-проектах.** 
