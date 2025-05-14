# Ansible

Various, unstructured ansible-related notes

## Paths, etc

```bash
export ANSIBLE_LOCAL_TEMP_HOME=$HOME/.ansible/tmp
export ANSIBLE_REMOTE_TEMP_HOME=$HOME/.ansible/tmp

export ANSIBLE_ROLES_PATH=/some/dir:/some/other/dir

$ ansible-playbook -i "localhost, " -c local /path/to/playbook.yml
```

## Lists

### First exercise

```yaml
---
- hosts: localhost
  vars:
    input: ['a', 'b', 'c']
    prefix: "--value " 
    result: "{{ [prefix] | product(input) | map('join') | list | join(' ') }}"
  tasks:
    - name: execute
      shell: |
        echo "a-tool {{ result }}"
      register: atool
    - debug: msg="{{ atool.stdout }}"
```

Output:

```
ok: [localhost] => {
    "msg": "a-tool --value a --value b --value c"
}
```

### Second exercise

```yaml
---
- hosts: localhost
  gather_facts: False
  vars:
    input:
      - a
      - b
      - c
      - d
    prefix: "-suffix"
    result: "{{ input | product([prefix]) | map('join') | list | join(' ') }}"
  tasks:
    - name: execute
      shell: |
        echo "a-tool {{ result }}"
      register: atool
    - debug: msg="{{ atool.stdout }}"
```

Output:

```
ok: [localhost] => {
    "msg": "a-tool a-suffix b-suffix c-suffix d-suffix"
}
```

### Third excercise

```yaml
---
- hosts: localhost
  gather_facts: False
  vars:
    input:
      - a
      - b
      - c
      - d
    prefix: "-suffix"
    result: "{{ input | product([prefix]) | map('join') | list  }}"
  tasks:
    - name: execute
      shell: |
        echo "a-tool {{ result }}"
      register: atool
    - debug: msg="{{ atool.stdout }}"
```

Output:

```
ok: [localhost] => {
    "msg": "a-tool ['a-suffix', 'b-suffix', 'c-suffix', 'd-suffix']"
}
```

### Fourth exercise

```yaml
---
- hosts: localhost
  gather_facts: False
  vars:
    input:
      - a
      - b
      - c
      - d
    prefix: "-suffix"
    result: "{{ input | product([prefix]) |  map('join') |  join(' ')}}"
  tasks:
    - name: execute
      shell: |
        echo "a-tool {{ result }}"
      register: atool
    - debug: msg="{{ atool.stdout }}"
```

Output:

```
ok: [localhost] => {
    "msg": "a-tool a-suffix b-suffix c-suffix d-suffix"
}
```

### Fifth exercise

```yaml
---
- hosts: localhost
  gather_facts: False
  vars:
    input:
      - a
      - b
      - c
      - d
    prefix: "-suffix"
    result: "{{ input | product([prefix]) |  map('join') |  join(' ')}}"
  tasks:
    - name: execute
      shell: |
        echo "a-tool {{ result }}"
      register: atool
    - debug: msg="{{ atool.stdout }}"
```

Output:

```
ok: [localhost] => {
    "msg": "a-tool a-suffix b-suffix c-suffix d-suffix"
}
```