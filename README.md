# ansible-debian_helper

Маленькая baseline-роль для свежих Debian/Ubuntu серверов.

Делает две вещи:

1. Ставит набор «обязательных» утилит: `grc`, `sudo`, `tmux`, `tcpdump`, `mc`, `bash-completion`.
2. Включает grc shell-aliases — `GRC_ALIASES=true` в `/etc/default/grc`, чтобы пакетный `/etc/profile.d/grc.sh` на login активировал цветные алиасы (`df`, `ip`, `ss`, `journalctl`, etc).

## Defaults

```yaml
debian_helper_packages:
  - grc
  - sudo
  - tmux
  - tcpdump
  - mc
  - bash-completion

debian_helper_grc_aliases: true
```

Если на каком-то хосте нужен другой набор пакетов или хочется отключить grc — переопредели в `host_vars`/`group_vars`:

```yaml
debian_helper_packages:
  - tmux
  - mc
debian_helper_grc_aliases: false
```

## Usage

В playbook:

```yaml
- name: Baseline Debian server
  hosts: debian_baseline
  become: true
  remote_user: napaster
  roles:
    - debian_helper
```

Inventory:

```ini
[debian_baseline]
mx.napaster.ru ansible_host=176.208.122.166
mx.mail-ok.ru  ansible_host=176.208.122.165
```

## Что НЕ делает

- Не трогает `/etc/profile.d/grc.sh` — это пакетный файл, ставится `apt install grc`.
- Не настраивает tmux/mc — если нужны кастомные конфиги, держи в отдельной роли.
- Не делает motd/profile/hostname/hosts/sudoers — для этого используется [k0ste/ansible-role-linux_helper](https://github.com/k0ste/ansible-role-linux_helper).

## Совместимость

- Debian 12 (bookworm), 13 (trixie)
- Ubuntu 22.04 (jammy), 24.04 (noble)

## License

MIT
