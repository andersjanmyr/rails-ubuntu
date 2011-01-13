require 'rake/remote_task'

set :domain, 'ec2-46-51-167-11.eu-west-1.compute.amazonaws.com'

set :app, domain

remote_task :install_ruby, :roles => :app do
    ruby_version = 'ruby-1.9.2-p136'
    run 'sudo apt-get install build-essential git-core curl wget'
    run "wget ftp://ftp.ruby-lang.org//pub/ruby/1.9/#{ruby_version}.gz"
    run "tar xvzf #{ruby_version}.tar.gz"
    run "cd #{ruby_version} && ./configure && make && sudo make install"
    run 'ruby -v'
end

