# Hexlet Ansible Profiling Demo

## Пререквизиты

- Установленный Ansible
- Установленный [Vagrant](https://www.vagrantup.com/)
- Установленный [Ara](https://github.com/ansible-community/ara)

## Как запустить

Подготавливаем виртуалку, об которую будем тестировать плейбуки:

```bash
vagrant init
vagrant up
```

Подготавливаем Ara для сбора статистики о выполнении плейбуков

```bash
export ANSIBLE_CALLBACK_PLUGINS="$(python3 -m ara.setup.callback_plugins)"
ara-manage migrate
ara-manage runserver
```

Ставим роли и коллекции из Ansible Galaxy

```bash
cd ansible
ansible-galaxy install -r roles/requirements.yml
```

Запускаем плейбук

```bash
ansible-playbook -i inventory/local.ini playbook.yml
```

При необходимости сбросить состояние виртуалки - перезапускаем Vagrant

```bash
vagrant destroy -f && vagrant up
```

## Как переключиться между шагами оптимизации плейбуков

Разные шаги имеют разные гитовые теги:

- step_0 - исходное состояние плейбуков, плагины для профилирования подключены, но никаких оптимизаций не сделано
- step_1 - оптимизировано SSH-соединение
- step_2 - оптимизировано получение и кэширование фактов
- step_3 - оптимизировано получение пакетов, добавлен async, добавлены теги для тасок

Переключиться между тегами можно командой `git checkout STEP_NAME`
