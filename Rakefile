require 'rubygems' unless defined? Gem # rubygems is only needed in 1.8

require 'yaml'
require 'plist'

config_file = 'config.yml'

workflow_home=File.expand_path("~/Library/Application Support/Alfred 2/Alfred.alfredpreferences/workflows")

task :config do
  $config = YAML.load_file(config_file)
  $config["bundleid"] = "#{$config["domain"]}.#{$config["id"]}"
  $config["plist"] = File.join($config["path"], "info.plist")

  info = Plist::parse_xml($config["plist"])
  unless info['bundleid'].eql?($config["bundleid"])
    info['bundleid'] = $config["bundleid"]
    File.open($config["plist"], "wb") { |file| file.write(info.to_plist) }
  end
end

desc "Install to Alfred"
task :install => [:config] do
  FileUtils.rm File.join(workflow_home, $config["bundleid"])
  ln_sf File.expand_path($config["path"]), File.join(workflow_home, $config["bundleid"])
end

desc "Unlink from Alfred"
task :uninstall => [:config] do
  rm File.join(workflow_home, $config["bundleid"])
end
