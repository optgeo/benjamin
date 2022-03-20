require './constants.rb'
require 'zip'
require 'json'

task :download do
  sh <<-EOS
curl -O #{SRC_URL}
  EOS
end

task :geojsons do
  require 'zip'
  fn = SRC_URL.split('/')[-1]
  Zip::File.open(fn) {|zip|
    p zip.entries
  }
end

task :tiles do
  sh <<-EOS
rake geojsons | 
tippecanoe -o tiles.mbtiles 
  EOS
end

task :style do
  sh <<-EOS
charites build style.yml docs/style.json 
  EOS
end

task :host do
  sh <<-EOS
budo -d docs
  EOS
end

