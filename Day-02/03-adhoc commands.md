### Adhoc commands 

It is usefull when we want to perform a simple and quick operations on managed node

```
ansible -i inventory.ini -m shell -a "touch test.txt" all
```