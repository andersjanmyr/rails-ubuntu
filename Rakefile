begin
    require 'rake/remote_task'
rescue LoadError
    puts <<-EOT
        This file depends on rake-remote_task. Install with:
            gem install rake-remote_task
    EOT
end

set :domain, 'ec2-79-125-61-254.eu-west-1.compute.amazonaws.com'
# set :domain, 'ec2-46-137-16-12.eu-west-1.compute.amazonaws.com'

set :app, domain

def install args
  run "sudo aptitude -y install #{args}"
end

desc "Essential"
remote_task :essential, :roles => :app do
  run 'sudo aptitude -y update'
  install 'build-essential libc6-dev-i386 git-core curl wget'
  install 'zlib1g zlib1g-dev libxml2 libxml2-dev libxslt-dev libssl-dev openssl'
  install 'libreadline5 libreadline5-dev libncurses5 libncurses5-dev'
end

desc 'Ruby'
remote_task :ruby, :roles => :app do
    ruby_version = 'ruby-1.9.2-p136'
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
  install 'sqlite3 libsqlite3-dev'
end

desc "Java"
remote_task :java, :roles => :app do
  install 'openjdk-6-jre default-jre icedtea6-plugin'
end

desc 'Jenkins'
remote_task :jenkins, :roles => :app do
  run 'wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -'
  run 'echo "deb http://pkg.jenkins-ci.org/debian binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list'
  run 'sudo aptitude -y update'
  install ' jenkins'
end

desc 'Sendmail'
remote_task :sendmail, :roles => :app do
  install 'sendmail'
end

task :all => [:essential, :ruby, :sqlite, :java, :jenkins, :sendmail]

task :default do
    puts 'There is no default task, available tasks are:'
    system 'rake -T'
end
