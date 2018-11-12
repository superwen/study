参考  
https://laravel-china.org/articles/13242/another-introduction-to-the-use-of-deployer


sudo usermod -aG nginx deployer  

我们需要将 depoloyer 用户加到 sudoers 中：  
```
$ vim /etc/sudoers
# 在最后加入
deployer ALL=(ALL) NOPASSWD: ALL
# 保存并退出
```

```
$ sudo chown deployer:nginx /var/www/html # 最后这里不要加斜线哦
$ sudo chmod g+s /var/www/html
```


laravel deploy的流程  
```
✔ Executing task deploy:prepare
✔ Executing task deploy:lock
✔ Executing task deploy:release
➤ Executing task deploy:update_code
✔ Executing task deploy:shared
✔ Executing task deploy:vendors
✔ Executing task deploy:writable
✔ Executing task artisan:storage:link
✔ Executing task artisan:view:clear
✔ Executing task artisan:cache:clear
✔ Executing task artisan:config:cache
✔ Executing task artisan:optimize
✔ Executing task deploy:symlink
✔ Executing task php-fpm:restart  --自定义的
✔ Executing task deploy:unlock
✔ Executing task cleanup
```


配置  
```
shared_dirs: 
shareD_files:
writable_dirs:
laravel_version:
```
命令  
```
artisan:up
artisan:down
artisan:migrate
artisan:migrate:fresh
artisan:migrate:rollback
artisan:migrate:status
artisan:db:seed
artisan:cache:clear
artisan:config:cache
artisan:route:cache
artisan:view:clear
artisan:optimize
artisan:queue:Restart
artisan:storage:link
deploy:public_disk
```
