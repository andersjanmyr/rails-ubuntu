begin
    require 'rake/remote_task'
rescue LoadError
    puts <<-EOT
        This file depends on rake-remote_task. Install with:
            gem install rake-remote_task
    EOT
end

set :domain, 'ec2-46-51-167-11.eu-west-1.compute.amazonaws.com'

set :app, domain

desc "Essential"
remote_task :essential, :roles => :app do
  run 'sudo apt-get install build-essential git-core curl wget'
end

desc 'Ruby'
remote_task :install_ruby => :essential, :roles => :app do
    ruby_version = 'ruby-1.9.2-p136'
    installed_version = run 'ruby -v'
    return if installed_version.include?(ruby_version)
    run "wget ftp://ftp.ruby-lang.org//pub/ruby/1.9/#{ruby_version}.gz"
    run "tar xvzf #{ruby_version}.tar.gz"
    run "cd #{ruby_version} && ./configure && make && sudo make install"
    run 'sudo gem update --system'
    run 'sudo gem install bundler'
end

desc "Sqlite"
remote_task :sqlite3, :roles => :app do
  run 'sudo apt-get sqlite3 libsqlite3-dev'
end

desc "Java"
task :java, :roles => :app do
  run 'sudo apt-get openjdk-6-jre default-jre icedtea6-plugin'
end

desc 'Jenkins'
remote_task :jenkins => :java, :roles => :app do
  run 'wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -'
  run 'sudo echo "deb http://pkg.jenkins-ci.org/debian binary/" > /etc/apt/sources.list.d/jenkins.list'
  run 'sudo aptitude update'
  run 'sudo aptitude install jenkins'

end

task :default do
    puts 'There is no default task, available tasks are:'
    system 'rake -T'
end
