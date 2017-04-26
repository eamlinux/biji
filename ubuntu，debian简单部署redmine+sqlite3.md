# ubuntu，debian简单部署redmine+sqlite3



安装Apache, mod-passenger, sqlite3

$ sudo apt-get install apache2 libapache2-mod-passenger sqlite3 libsqlite3-dev
$ sudo apt-get install redmine redmine-sqlite #选择“yes”后，再选择sqlite3
$ sudo gem update
$ sudo gem install bundler

修改 /etc/apache2/mods-available/passenger.conf 内容如下：

<IfModule mod_passenger.c>
  PassengerDefaultUser www-data
  PassengerRoot /usr/lib/ruby/vendor_ruby/phusion_passenger/locations.ini
  PassengerDefaultRuby /usr/bin/ruby
</IfModule>

连接redmine到web目录

$ sudo ln -s /usr/share/redmine/public /var/www/html/redmine

修改 /etc/apache2/sites-available/000-default.conf 添加内容：

<Directory /var/www/html/redmine>
    RailsBaseURI /redmine
    PassengerResolveSymlinksInDocumentRoot on
</Directory>

创建gemfile.lock并指定给www-data用户

$ sudo touch /usr/share/redmine/Gemfile.lock
$ sudo chown www-data:www-data /usr/share/redmine/Gemfile.lock

重启apche2服务

$ sudo service apache2 restart

就可以通过http://你的服务器IP/redmine访问！
