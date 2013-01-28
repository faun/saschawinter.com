# A sample Guardfile
# More info at https://github.com/guard/guard#readme

guard 'less', :all_on_start => true, :all_after_change => true, :output => 'bootstrap/css' do
  watch(%r{^bootstrap/less/(.+\.less)$})
end
guard 'jekyll', :all_on_start => true, :all_after_change => true do
  watch(%r{^(.+\.html)|(.+\.css)|(.+\.markdown|md)|(_config\.yml)$})
end
