# Where our Bootstrap source is installed. Can be overridden by an environment variable.
BOOTSTRAP_SOURCE = ENV['BOOTSTRAP_SOURCE'] || File.expand_path("~/src/bootstrap")

# Where to find our custom LESS file.
BOOTSTRAP_CUSTOM_LESS = 'bootstrap/less/custom.less'

def different?(path1, path2)
  require 'digest/md5'
  different = false
  if File.exist?(path1) && File.exist?(path2)
    path1_md5 = Digest::MD5.hexdigest(File.read path1)
    path2_md5 = Digest::MD5.hexdigest(File.read path2)
    (path2_md5 != path1_md5)
  else
    true
  end
end

task :bootstrap => [:bootstrap_js, :bootstrap_css, :bootstrap_img]

task :bootstrap_js do
  require 'uglifier'
  require 'erb'

  template = ERB.new %q{
  <% paths.each do |path| %>
  <script type="text/javascript" src="/bootstrap/js/<%= path %>"></script><% end %>
  }

  paths = []
  minifier = Uglifier.new
  Dir.glob(File.join(BOOTSTRAP_SOURCE, 'js', '*.js')).each do |source|
    base = File.basename(source).sub(/^(.*)\.js$/, '\1.min.js')
    paths << base
    target = File.join('bootstrap/js', base)
    if different?(source, target)
      File.open(target, 'w') do |out|
        out.write minifier.compile(File.read(source))
      end
    end
  end

  File.open('_includes/bootstrap.js.html', 'w') do |f|
    f.write template.result(binding)
  end
end

task :bootstrap_css do |t|
  puts "Copying LESS files"
  Dir.glob(File.join(BOOTSTRAP_SOURCE, 'less', '*.less')).each do |source|
    target = File.join('bootstrap/less', File.basename(source))
    cp source, target if different?(source, target)
  end
end

task :bootstrap_img do |t|
  puts "Copying image files"
  Dir.glob(File.join(BOOTSTRAP_SOURCE, 'img', '*')).each do |source|
    target = File.join('bootstrap/img', File.basename(source))
    cp source, target if different?(source, target)
  end
end

task :default => :jekyll

task :jekyll => :bootstrap do
  system "jekyll --no-auto"
end

task :staging => :jekyll do
  # sh "s3cmd del s3://com.saschawinter.staging/ --recursive --force"
  sh "s3cmd sync --acl-public --force --progress _site/ s3://com.saschawinter.staging --exclude-from=.exclude --add-header='Cache-Control:max-age=1200, must-revalidate'"
end
