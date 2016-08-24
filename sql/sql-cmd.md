###启动服务###
- 打开cmd 进入mysql的bin目录，输入net start mysql;同理关闭为 net stop mysql

###如果密码错误###
必须重新设置密码

- 打开cmd 进入mysql的bin目录,mysqld --skip-grant-tables;(其实为还原默认账号密码:root,mysql)
- 在另开一个cmd，进入bin目录，输入mysql -u root mysql
- 然后输入update user set password=password('123456') where user='root';
- 然后刷新权限，FLUSH PRIVILEGES;
