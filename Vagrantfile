# Configuration
EXPOSE_PORT=8080

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/xenial64"
    config.vm.synced_folder "./ips", "/var/www/html", create: true, group: "www-data", owner: "www-data"
    config.vm.network "public_network"
    config.vm.network "forwarded_port", guest: 80, host: EXPOSE_PORT

    config.vm.provider "virtualbox" do |v|
        v.name = "Invision Community Development Server"
        v.customize ["modifyvm", :id, "--memory", "1024"]
    end

    $script = <<-SCRIPT
    echo "Updating packages..."
    apt-get -y update >/dev/null
  
    echo "Adding dependencies..."
    sudo apt-get -y install software-properties-common
    sudo add-apt-repository ppa:ondrej/php
    apt-get -y update >/dev/null

    echo "Installing Dependencies..."
    apt-get install -y php7.4-cli composer php7.4-zip unzip apache2 php7.4 libapache2-mod-php7.4 php7.4-mysql php7.4-mbstring mysql-client apache2-utils php-apcu php7.4-xml php7.4-gd >/dev/null

    echo "Installing MySQL..."
    sudo apt-get install debconf-utils -y > /dev/null
    debconf-set-selections <<< "mysql-server mysql-server/root_password password password" > /dev/null
    debconf-set-selections <<< "mysql-server mysql-server/root_password_again password password" > /dev/null
    sudo apt-get -y install mysql-server > /dev/null
    echo "Exposing MySql over local area network..."
    sed -i '43s!127.0.0.1!0.0.0.0!' /etc/mysql/mysql.conf.d/mysqld.cnf
    service mysql restart

    echo "Updating MySql Permissions..."
    mysql -u root -ppassword -e "USE mysql;UPDATE user SET host='%' WHERE User='root';GRANT ALL ON *.* TO 'root'@'%';FLUSH PRIVILEGES;" >/dev/null

    echo "Creating MySql Database 'community'..."
    mysql -u root -ppassword -e "CREATE DATABASE community;"

    echo "Enabling modrewrite..."
    a2enmod rewrite
    sed -i '155s!None!All!' /etc/apache2/apache2.conf
    sed -i '166s!None!All!' /etc/apache2/apache2.conf

    echo "Restarting Apache Service..."
    service apache2 restart

    echo "Done."
    echo ""
    echo "You may now open http://localhost:$1 in your web browser."
    echo ""
    echo "When the installation process asks you for for you license key, make sure you append it with '-TESTINSTALL'."
    echo "See https://invisioncommunity.com/4guides/welcome/install-and-upgrade-r259/ for more information"
    SCRIPT
  
    config.vm.provision "shell", inline: $script, args: EXPOSE_PORT
  end