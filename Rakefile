#!/usr/bin/rake -T

require 'yaml'
require 'find'

desc 'Validate the contents of the repository'
task :validate do
  Rake::Task['yaml_lint'].execute
end

desc 'Lint all YAML files'
task :yaml_lint do
  yaml_files = []
  Find.find('.') do |path|
    # Ignore dot directories and files
    if path.split(File::SEPARATOR)[1][0] == '.'
      Find.prune
      next
    end

    if path =~ /\.ya?ml$/
      yaml_files << path
    end
  end

  yaml_dirs = yaml_files.map do |x|
    x = x.split(File::SEPARATOR)[0..1].join(File::SEPARATOR)
  end.sort.uniq

  sh %(yaml-lint -i #{yaml_dirs.join(' ')})

  invalid_yaml = []
  yaml_files.each do |yaml_file|
    yaml = YAML.load_file(yaml_file)

    unless yaml.is_a?(Hash)
      invalid_yaml << invalid_yaml
    end
  end

  unless invalid_yaml.empty?
    $stderr.puts("Error: The following files do not contain Hashes:")
    $stderr.puts(invalid_yaml.map{|x| x = "  * #{x}"}.join("\n"))

    exit 1
  end
end

task :default => :validate
