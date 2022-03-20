require './constants.rb'

task :download do
  sh <<-EOS
curl -O #{SRC_URL}
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

