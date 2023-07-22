## WARNING!

```
Mongodump and Mongorestore for backup/restore on larg databases are not a perfect way and we must use another method!
```

## Mongodump

### options

- `--uri` : connection string
- `--out` : the path of output dir
- `--archive` : the path of output archive file
- `-v` : verbose mode
- `--config` : the path of config file. (for sensitive values such as `--password`, `--uri` and `--sslPEMKeyPassword` it's better to create a YML file and set those values in that file)
- `--collection` : specifies a collectoin to backup.
- `--gzip` : compresses the output.
- `--oplog` : creates a oplog.bson and contains oplog entries that occur during the mongodump. this file provides an effective point-in-time snapshot of the state of a mongod instance. (use in `replica sets`)

### backup as a directory

```
mongodump --uri=mongodb://<host>:<port>/<db> --out=<path>
```

and to compressing it

```
mongodump --gzip --uri=mongodb://<host>:<port>/<db> --out=<path>
```

### backup as a archive file

```
mongodump --uri=mongodb://<host>:<port>/<db> --archive=<path>
```

and to compressing it

```
mongodump --gzip --uri=mongodb://<host>:<port>/<db> --archive=<path>.gz
```

### backup with `--oplog`

```
mongodump --uri=mongodb://<host>:<port>/ --oplog --out=./2023-22-01.backup.db
```

**note**: only for `replica sets` is used.

**note**: with `--oplog` you must dump whole database.

## Mongorestore

**Note**: The version of Mongodump you backed up a database with must be the same as the Mongorestore you want to restore.

**Note**: the version of the destination database shoud be the same with the database that was backed up.

### options

- `--preserveUUID` : doesn't create new \_id.
- `--nsFrom` : source database and collectoin. (format: <database\>:<collection\>)
- `--nsTo` : destination database and collection. (format: <database\>:<collection\>)
- `--oplogReplay` : After restoring the database dump, replays the oplog entries from a bson file.

### restore a directory

```
mongorestore --uri=mongodb://<host>:<port>/ --dir=<path> --nsFrom=<db>.<coll> --nsTo=<db>.<coll>
```

And if is commpressed

```
mongorestore --gzip --uri=mongodb://<host>:<port>/ --dir=<path> --nsFrom=<db>.<coll> --nsTo=<db>.<coll>
```

### restore a archive file

```
mongorestore --uri=mongodb://<host>:<port>/ --archive=<path> --nsFrom=<db>.<coll> --nsTo=<db>.<coll>
```

And if archive is commpressed

```
mongorestore --gzip --uri=mongodb://<host>:<port>/ --archive=<path> --nsFrom=<db>.<coll> --nsTo=<db>.<coll>
```

### backup with `--oplogReplay`

```
mongorestore --uri=mongodb://<host>:<port>/ --dir=<path> --oplogReplay
```
