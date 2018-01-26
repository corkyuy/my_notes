# Ideas

## Enable searching in BASH history

```
stty -ixon
(CTRL-r)
```

## Horizontally scalable data

```
digraph node {
  refData;
  server1;
  server2;
  server3;
  server4;

  refData -> server1;
  refData -> server2;
  refData -> server3;
  refData -> server4;
}
```

### Questions

* How much data will it need?
* Syncing? Should it start from the beginning?
* 

### Technology that we can use

* redis clustering
* etcd
* cockroachDB
* TiDB

