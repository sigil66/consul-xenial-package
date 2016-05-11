NAME='consul'
DESCRIPTION='HashiCorp Consul'
VERSION='0.6.4'
BUILD_DIR='./build'
OUT_DIR='./pkg'
PREFIX='/usr/bin'

# Setup RUBY ENV
ENV['PATH'] =  "#{Dir.pwd}/.gems/bin:#{ENV['PATH']}"
ENV['GEM_HOME'] = "#{Dir.pwd}/.gems"
ENV['GEM_PATH'] = '/var/lib/gems/2.3.0'

desc 'Install Dependencies'
task :install_deps do
  Dir.mkdir "#{Dir.pwd}/.gems" unless File.directory? "#{Dir.pwd}/.gems"
  sh %{ bundle update }
end

desc 'Clean'
task :clean do
  sh %{ rm -rf *.zip }
  sh %{ rm -rf ./build }
  sh %{ rm -rf ./pkg }
end

desc 'Fetch Binary'
task :fetch do
  sh %{ wget https://releases.hashicorp.com/consul/#{VERSION}/consul_#{VERSION}_linux_amd64.zip }
end

desc 'Build'
task :build do
  sh %{ mkdir -p #{BUILD_DIR}#{PREFIX} }
  sh %{ mkdir -p #{BUILD_DIR}/etc/consul/conf.d }
  sh %{ mkdir -p #{BUILD_DIR}/lib/systemd/system }
  sh %{ unzip consul_#{VERSION}_linux_amd64.zip }
  sh %{ mv ./consul #{BUILD_DIR}#{PREFIX} }
  sh %{ cp ./consul.service #{BUILD_DIR}/lib/systemd/system }
  sh %{ cp ./systemd.env #{BUILD_DIR}/etc/consul }
end

desc 'Package'
task :pkg do
  sh %{ mkdir ./pkg }
  sh %{
    fpm -t deb \
              -s dir \
              -n #{NAME} \
              -v #{VERSION} \
              --description "#{DESCRIPTION}" \
              -a amd64 \
              --deb-user root \
              --deb-group root \
              --after-install ./pkg-scripts/postinst \
              --before-install ./pkg-scripts/preinst \
              --after-remove ./pkg-scripts/postrm \
              --before-remove ./pkg-scripts/prerm \
              -p #{OUT_DIR} \
              -C #{BUILD_DIR} \
              .
  }
end

task :default => [:install_deps, :clean, :fetch, :build, :pkg]

