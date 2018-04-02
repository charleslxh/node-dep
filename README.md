# What this

Excute command in target server via ssh2 protocol.

## How to install

```bash
$ npm install node-dep --save-dev
```

## Configurations

### Task

Task is the special linux command to excute.

- **name**: task description.
- **command**: the command to excute.
- **priority**: the priority of this task, tasks will be sorted by their priority before excution.
- **stage**: only run this task in some stages server.
- **workDir**: where the command to excute.

### Proxy

Specify the proxy server when your server is behind a forward agent server (ssh hopping).

see [node-ssh2 client connect configuration](https://github.com/mscdex/ssh2#client-methods).

### Server

- **useProxy**: if true, it will connect via proxy server, `Default: false`.
- **stage**: the server stage.
- **releasePath**: your application release path.
- **connectOptions**: see [node-ssh2 client connect configuration](https://github.com/mscdex/ssh2#client-methods).

## Events

- **ready**: when all clents connected, this event will be fire.
- **done**: All task completed.

## Methods

- **setLogger(< object >logger)**: replace logger instance.
- **setProxy(< object >options)**: replace proxy instance.
- **addServer(< object >options, < function >callback)**: add a remote server.
- **removeServer(< string >name, < function >callback)**: remove a remote server.
- **addTask(< object >options)**: add a task.
- **removeTask(< string >name)**: remove a task.
- **sort(< function >callback)**: sort all tasks, default sort by priority.
- **start()**: start to excute all tasks.

# Examples

```js
const dep = require('gulp-dep');

const options = {
    "tasks": [
        {
            "name": "list all files in /var/www",
            "command": "ls -la",
            "priority": 0,
            "stages": ["prod", "test"],
            "workDir": "/var/www"
        }
    ,
        {
            "name": "show the absolute path of application release path",
            "command": "pwd",
            "priority": 0,
            "stages": ["prod", "test"]
        }
    ],
    "proxy": {
        "host": "192.168.51.222",
        "port": 22,
        "username": "proxy_user_name",
        "privateKey": fs.readFileSync(path.resolve(os.homedir(), ".ssh/id_rsa"))
    },
    "servers": [
        {
            "useProxy: false",
            "stage": "test",
            "releasePath": "/var/www/app",
            "connectOptions": {
                "name": "s1",
                "host": "192.168.51.223",
                "port": 22,
                "username": "user",
                "password": "123456"
            }
        }
    ]
}

const dep = new Dep(options);

dep.on('ready', dep.start);
dep.on('done', () => console.log('tasks completed'));
```
