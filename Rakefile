require 'rmagick'

# Default task is to build assets
task :default => [:build]

################################################################################
# File lists

SASS_FILES = FileList.new('assets/sass/**/*.scss') do |fl|
  fl.exclude('assets/sass/**/_*')
end
SASS_DEPENDENCIES = FileList.new('assets/sass/**/_*.scss') do |fl|
  fl.include('assets/stylesheets')
end
CSS_FILES = SASS_FILES.pathmap('assets/stylesheets/%n.css')

BANNER_IMAGES = FileList.new('assets/images/banner*') do |fl|
  fl.exclude('assets/images/banner*_small.*')
end
SMALL_BANNER_IMAGES = BANNER_IMAGES.pathmap('%X_small%x')

POST_IMAGES = FileList.new('assets/images/posts/**/*') do |fl|
  fl.exclude('assets/images/posts/**/*_small.*')
  fl.exclude('assets/images/posts/**/*_thumb.*')
end
SMALL_POST_IMAGES = POST_IMAGES.pathmap('%X_small%x')
POST_THUMBNAILS = POST_IMAGES.pathmap('%X_thumb%x')

################################################################################
# Build tasks

desc 'Compile SASS, etc...'
task :build => CSS_FILES + SMALL_BANNER_IMAGES + SMALL_POST_IMAGES + POST_THUMBNAILS

################################################################################
# Rules for file conversions, etc

# create stylesheets directory if it does not exist
directory 'assets/stylesheets'

# sass dependencies
CSS_FILES.each do |f|
  file f => [f.pathmap('assets/sass/%n.scss')] + SASS_DEPENDENCIES
end

# sass -> css rule
rule '.css' => [proc{|n| n.pathmap('assets/sass/%n.scss')}] do |t|
  puts "Converting #{t.source}..."
  sh "sass '#{t.source}' '#{t.name}'"
end

# banner dependencies
SMALL_BANNER_IMAGES.each do |f|
  file f => [f.sub(/_small(\.[^\/.]+)$/, '\1')]
end

# banner image -> small banner rule
rule(/banner[^\/]*_small.[^\/.]+$/ => [proc{|n| n.sub(/_small(\.[^\/.]+)$/, '\1')}]) do |t|
  puts "Creating small banner for #{t.source}..."
  img = Magick::Image.read t.source
  img = img[0] if img.is_a? Array
  thumb = img.resize_to_fill(640, 267)
  thumb.write t.name
end

# small post image dependencies
SMALL_POST_IMAGES.each do |f|
  file f => [f.sub(/_small(\.[^\/.]+)$/, '\1')]
end

# post image -> small image rule
rule(/_small.[^\/.]+$/ => [proc{|n| n.sub(/_small(\.[^\/.]+)$/, '\1')}]) do |t|
  puts "Creating small post image for #{t.source}..."
  img = Magick::Image.read t.source
  img = img[0] if img.is_a? Array
  thumb = img.resize_to_fill(640, 200)
  thumb.write t.name
end

# thumbnail dependencies
POST_THUMBNAILS.each do |f|
  file f => [f.sub(/_thumb(\.[^\/.]+)$/, '\1')]
end

# post image -> thumbnail rule
rule(/_thumb.[^\/.]+$/ => [proc{|n| n.sub(/_thumb(\.[^\/.]+)$/, '\1')}]) do |t|
  puts "Creating thumbnail for #{t.source}..."
  img = Magick::Image.read t.source
  img = img[0] if img.is_a? Array
  thumb = img.resize_to_fill(256, 256)
  thumb.write t.name
end