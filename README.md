# supervisior-setup-ubuntu-20.04-laravel

## Step 1

Install supervisor. For more information on Supervisor, consult the Supervisor documentation. http://supervisord.org/

```
    sudo apt-get install supervisor
```

## Step 2

The supervisor configuration can be located in "/etc/supervisor/conf.d" and from within this directory, you can create as many configurations as you like.

```
    cd /etc/supervisor/conf.d
```

## Step 3

You can create file with nano or vi. For example, let's create a laravel-worker.conf file that starts and monitors queue:work processes:

```
    sudo nano laravel-worker.conf
```

## Step 4

The content of the file should be as follows
* command=php /var/www/html/artisan queue:work --tries=3 -  here /var/www/html/ is my project root folder path
* numprocs=2 - for simple application it should be 1 or 2, and for heavy application you can increase the no. 
* stdout_logfile=/var/www/html/storage/logs/worker.log - log path

```
    [program:laravel-worker]
    process_name=%(program_name)s_%(process_num)02d
    command=php /var/www/html/artisan queue:work --tries=3
    autostart=true
    autorestart=true
    user=root
    numprocs=2
    redirect_stderr=true
    stdout_logfile=/var/www/html/storage/logs/worker.log
    startsecs=0
```

## Step 5

Add below line to .env

```
   SUPERVISE=enable
```


## Step 6

Once you have created the configuration, you can start the supervisor process. To start supervisor you can run the following commands.

```
    sudo supervisorctl reread
    sudo supervisorctl update
    sudo supervisorctl start laravel-worker:*
```

## Step 7

To see the running status of superviosor

```
    sudo supervisorctl status
```
It will show the result as
```
    laravel-worker:laravel-worker_00   RUNNING   pid 3374359, uptime 3 days, 9:29:13
    laravel-worker:laravel-worker_01   RUNNING   pid 3374360, uptime 3 days, 9:29:13
```
