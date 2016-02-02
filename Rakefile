require 'bundler'
require 'yaml'
require 'english'
Bundler::GemHelper.install_tasks

CONFIG = YAML.load_file(
  File.expand_path('spec/support/database.yml', File.dirname(__FILE__))
)

def test_database_exists?
  system "psql -l | grep -q #{CONFIG['test'][:database]}"
  $CHILD_STATUS.success?
end

def create_test_database
  system "createdb #{CONFIG['test'][:database]}"
end

namespace :db do
  task :create do
    create_test_database unless test_database_exists?
  end
end

require 'rubocop/rake_task'
RuboCop::RakeTask.new

desc 'Run all tests'
task spec: 'db:create' do
  exec 'rspec'
end

task default: [:spec, :rubocop]

