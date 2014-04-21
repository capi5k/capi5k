require 'bundler/setup'
require 'rubygems'
require 'capistrano'
require 'xp5k'
require 'erb'
require 'colored'


load "config/deploy.rb"
load "config.rb"
load "config/deploy/xp5k.rb"

desc 'Automatic deployment'
task :automatic do
 puts "Welcome to automatic deployment".bold.blue
end

after "automatic", "xp5k", "rabbitmq"
