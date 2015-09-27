Vagrant.configure(2) do |config|
  config.vm.box = "centos66x86"

  config.vm.hostname = "nikkei"
  config.vm.network :forwarded_port, host: 9000, guest: 9000
  config.vm.network :forwarded_port, host: 3000, guest: 3000
  config.vm.network "private_network", ip: "192.168.33.16"

  config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = false

     # Customize the amount of memory on the VM:
     vb.memory = "1024"
     vb.name   = "nikkei"
  end
  config.vm.provision "shell", inline: <<-EOT
    # timezone
    cp -p /usr/share/zoneinfo/Japan /etc/localtime

    # selinux
    sudo setenforce permissive

    # add user
    #sudo useradd kieaiaarh
    #sudo su - kieaiaarh

    # ssh
    #mkdir /home/kieaiaarh/.ssh
    #chmod 700 /home/kieaiaarh/.ssh
    #exit

    # git
    sudo yum install -y make bzip2-devel curl-devel gcc openssl-devel expat-devel cpan gettext  asciidoc xmlto
    cd /usr/local/src/
    sudo wget https://www.kernel.org/pub/software/scm/git/git-2.3.2.tar.gz
    sudo tar -zxf git-2.3.2.tar.gz
    cd git-2.3.2
    sudo make prefix=/usr/local all
    sudo make prefix=/usr/local install
    cd
    #git --version

    # vim
    sudo yum install -y mercurial ncurses-devel
    cd /usr/local/src
    sudo hg clone https://vim.googlecode.com/hg/ vim
    cd vim
    sudo hg pull
    sudo ./configure --with-features=huge --enable-multibyte --disable-selinux
    sudo make && sudo make install
    vim --version

    # postgresSQL
    sudo yum remove postgresql postgresql-devel postgresql-libs postgresql-server
    sudo yum install -y http://yum.postgresql.org/9.4/redhat/rhel-6-x86_64/pgdg-centos94-9.4-1.noarch.rpm
    sudo yum install -y postgresql94-server postgresql94-contrib
    sudo service postgresql-9.4 initdb
    sudo service postgresql-9.4 start
    sudo chkconfig postgresql-9.4 on

    # rbenv
    #su - kieaiaarh
    sudo yum install -y zlib zlib-devel openssl-devel sqlite-devel gcc-c++ glibc-headers libyaml-devel readline readline-devel zlib-devel libffi-devel libxml2 libxslt libxml2-devel libxslt-devel
    git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
    git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo "export PATH=$PATH:$HOME/.rbenv/bin:$HOME/.rbenv/shims" >> ~/.bash_profile
    exec $SHELL -1
    rbenv install 2.2.3
    rbenv global 2.2.3
    rbenv rehash

    # rails
    gem update --system
    gem install nokogiri -- --use-system-libraries
    gem install --no-ri --no-rdoc rails

    # fragmentation
    sudo dd if=/dev/zero of=/EMPTY bs=1M
    sudo rm -f /EMPTY
  EOT
end
