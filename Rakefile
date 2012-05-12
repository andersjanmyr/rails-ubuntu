begin
    require 'rake/remote_task'
rescue LoadError
    puts <<-EOT
        This file depends on rake-remote_task. Install with:
            gem install rake-remote_task
    EOT
end

set :domain, 'lexicon' 
# set :domain, 'ec2-46-137-16-12.eu-west-1.compute.amazonaws.com'

set :app, domain

def install_pkg args
  run "sudo aptitude -y install #{args}"
end

desc "Essential"
remote_task :essential, :roles => :app do
  run 'sudo aptitude -y update'
  install_pkg 'build-essential libc6-dev-i386 git-core curl wget'
  install_pkg 'zlib1g zlib1g-dev libxml2 libxml2-dev libxslt-dev libssl-dev openssl'
  install_pkg 'libreadline5 libreadline5-dev libncurses5 libncurses5-dev'
end

desc 'Ruby'
remote_task :ruby, :roles => :app do
    ruby_version = 'ruby-1.9.3-p125'
    begin
      installed_version = run 'ruby -v'
    rescue
      installed_version = 'none'
    end
    return if installed_version.include?(ruby_version)
    run "wget ftp://ftp.ruby-lang.org//pub/ruby/1.9/#{ruby_version}.tar.gz"
    run "tar xvzf #{ruby_version}.tar.gz"
    run "cd #{ruby_version} && ./configure && make && sudo make install"
    run 'sudo gem update --system'
    run 'sudo gem install bundler'
    run "cd #{ruby_version}/ext/openssl && ruby extconf.rb && make && sudo make install"
end

desc "Sqlite"
remote_task :sqlite, :roles => :app do
  install_pkg 'sqlite3 libsqlite3-dev'
end

desc "Java"
remote_task :java, :roles => :app do
  install_pkg 'openjdk-6-jre default-jre icedtea6-plugin'
end

desc 'Jenkins'
remote_task :jenkins, :roles => :app do
  run 'wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -'
  run 'echo "deb http://pkg.jenkins-ci.org/debian binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list'
  run 'sudo aptitude -y update'
  install_pkg ' jenkins'
end

desc 'Sendmail'
remote_task :sendmail, :roles => :app do
  install_pkg 'sendmail'
end

desc 'XVFB'
remote_task :xvfb, :roles => :app do
  install_pkg 'xvfb'
end

desc 'Install all the other tasks'
task :all => [:essential, :ruby, :sqlite, :java, :jenkins, :sendmail]

task :default do
    puts 'There is no default task, available tasks are:'
    system 'rake -T'
end
