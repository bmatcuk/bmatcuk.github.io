require 'rmagick'

# Default task is to build assets
task :default => [:build]

################################################################################
# Helper methods

# List of css files, based on existing scss files
def css_files
  Dir['assets/sass/**/*'].
    select{|f| !File.directory?(f) && !File.basename(f).start_with?('_')}.
    map{|f| f.sub(/\.scss$/, '.css').sub('assets/sass', 'assets/stylesheets')}
end

# List of scss includes and other sass dependencies
def css_dependencies
  ['assets/stylesheets'] + Dir['assets/sass/**/*'].select{|f| !File.directory?(f) && File.basename(f).start_with?('_')}
end

# Images for blog posts - thumbnails will be generated
def post_images
  Dir['assets/images/posts/**/*'].
    select{|f| !File.directory?(f) && !(f =~ /_thumb\.[^\/.]+$/)}.
    map{|f| f.sub(/(\.[^\/.]+)$/, '_thumb\1')}
end

################################################################################
# Build tasks

desc 'Compile SASS, etc...'
task :build => css_files + post_images

################################################################################
# Rules for file conversions, etc

# create stylesheets directory if it does not exist
directory 'assets/stylesheets'

# sass dependencies
css_files.each do |f|
  task f => css_dependencies
end

# sass rule
rule '.css' => [proc{|n| n.sub(/\.css$/, '.scss').sub('assets/stylesheets', 'assets/sass')}] do |t|
  puts "Converting #{t.source}..."
  sh "sass '#{t.source}' '#{t.name}'"
end

# Create thumbnails
rule(/_thumb.[^\/.]+$/ => [proc{|n| n.sub(/_thumb(\.[^\/.]+)$/, '\1')}]) do |t|
  puts "Creating thumbnail for #{t.source}..."
  img = Magick::Image.read t.source
  img = img[0] if img.is_a? Array
  thumb = img.resize_to_fill(256, 256)
  thumb.write t.name
end