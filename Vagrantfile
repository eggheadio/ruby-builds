# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provision "shell", inline: <<-SHELL
    RUBY_BUILD_VERSION=2.2.4
    
    echo "\n\nPrepping to build ruby version ${RUBY_BUILD_VERSION}\n\n"

    echo "\n\nUpdating apt lists or you'll get 404 errors from outdated package URLs\n\n"
    apt-get update
    apt-get autoremove

    echo "\n\nInstalling ruby-dev for mkmf\n\n"
    apt-get install -y ruby-dev

    echo "\n\nInstalling dependencies (these match the dependency list in the fpm command)\n\n"
    apt-get install -y \
    git \
    git-core \
    g++ \
    gcc \
    make \
    libc6-dev \
    patch \
    openssl \
    ca-certificates \
    libreadline6 \
    libreadline6-dev \
    curl \
    zlib1g \
    zlib1g-dev \
    libssl-dev \
    libyaml-dev \
    libsqlite3-dev \
    sqlite3 \
    autoconf \
    libgdbm-dev \
    libncurses5-dev \
    automake \
    libtool \
    bison \
    pkg-config \
    libffi-dev \
    imagemagick

    echo "\n\nInstalling the fpm gem so we can use it for the build\n\n"
    gem install fpm --no-rdoc --no-ri

    echo "\n\nDownloading ruby source\n\n"
    wget "http://cache.ruby-lang.org/pub/ruby/ruby-${RUBY_BUILD_VERSION}.tar.gz" --quiet
    
    echo "\n\nExtracting and compiling ruby source\n\n"
    tar -zxf ruby-${RUBY_BUILD_VERSION}.tar.gz
    cd ruby-${RUBY_BUILD_VERSION}/
    ./configure --prefix=/usr --disable-install-doc
    make clean
    make
    mkdir /tmp/installdir
    make install DESTDIR=/tmp/installdir

    echo "\n\nCreating the package from the built ruby\n\n"
    fpm \
    -s dir \
    -t deb \
    -n ruby-${RUBY_BUILD_VERSION} \
    -v 1 \
    -C /tmp/installdir \
    -d git \
    -d git-core \
    -d g++ \
    -d gcc \
    -d make \
    -d libc6-dev \
    -d patch \
    -d openssl \
    -d ca-certificates \
    -d libreadline6 \
    -d libreadline6-dev \
    -d curl \
    -d zlib1g \
    -d zlib1g-dev \
    -d libssl-dev \
    -d libyaml-dev \
    -d libsqlite3-dev \
    -d sqlite3 \
    -d autoconf \
    -d libgdbm-dev \
    -d libncurses5-dev \
    -d automake \
    -d libtool \
    -d bison \
    -d pkg-config \
    -d libffi-dev \
    -d imagemagick \
    .
    
    echo "\n\nSharing the built package with the host system (outside of VM via synced dir)\n\n"
    cp *.deb /vagrant/
  SHELL
end
