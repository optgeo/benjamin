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
  n = 0
  fn = SRC_URL.split('/')[-1]
  Zip::File.open(fn) {|zip|
    zip.entries.each {|entry|
      if entry.name == PATH
        entry.get_input_stream.each_line {|line|
          r = line.strip.split(',')
          lng = r[INDEX_LONG].to_f
          lat = r[INDEX_LAT].to_f
          next if lng == 0.0 or lat == 0.0
          f = {
            :type => 'Feature',
            :geometry => {
              :type => 'Point',
              :coordinates => [lng, lat]
            },
            :properties => {},
            :tippecanoe => {
              :layer => 'nad'
            }
          }
          print "\x1e#{JSON.dump(f)}\n"
          n += 1
          break if n == MAX_FEATURES
        }
      end
    }
  }
end

task :plaingeojsons do
  n = 0
  File.foreach(PATH) {|line|
    r = line.strip.split(',')
    lng = r[INDEX_LONG].to_f
    lat = r[INDEX_LAT].to_f
    next if lng == 0.0 or lat == 0.0
    print <<-EOS
\x1e{"type":"Feature","geometry":{"type":"Point","coordinates":[#{lng},#{lat}]},"properties":{},"tippecanoe":{"layer":"nad"}}
    EOS
    n += 1
    break if n == MAX_FEATURES
  }
end

task :tiles do
  sh <<-EOS
rake geojsons | 
tippecanoe \
--no-tile-compression \
--output-to-directory=docs/zxy2 \
--temporary-directory=#{TEMPORARY_DIRECTORY} \
--force \
--minimum-zoom=#{TILE_MINZOOM} \
--maximum-zoom=#{TILE_MAXZOOM}
  EOS
end

task :style do
  sh <<-EOS
charites --provider=mapbox build style.yml docs/style.json 
  EOS
end

task :host do
  sh <<-EOS
budo -d docs
  EOS
end

