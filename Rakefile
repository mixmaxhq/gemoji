require 'rake/testtask'

task :default => :test

Rake::TestTask.new do |t|
  t.libs << "test"
  t.test_files = FileList["test/*_test.rb"]
end

namespace :db do
  desc %(Generate Emoji data files needed for development)
  task :generate => ['db/Category-Emoji.json', 'db/NamesList.txt']

  desc %(Dump a list of supported Emoji with Unicode descriptions and aliases)
  task :dump => :generate do
    system 'ruby', '-Ilib', 'db/dump.rb'
  end
end

emoji_plist = '/System/Library/Input Methods/CharacterPalette.app/Contents/Resources/Category-Emoji.plist'
nameslist_url = 'http://www.unicode.org/Public/6.3.0/ucd/NamesList.txt'

task 'db/Category-Emoji.json' do |t|
  system "plutil -convert json -r '#{emoji_plist}' -o '#{t.name}'"
end

file 'db/NamesList.txt' do |t|
  system "curl -fsSL '#{nameslist_url}' -o '#{t.name}'"
end

namespace :images do
  desc %(Extract PNG images from Apple's "Apple Color Emoji.ttf" font)
  task :extract, [:size, :images_dir] do |t, args|
    require 'emoji/extractor'
    gem_dir = File.dirname(File.realpath(__FILE__))
    args.with_defaults(:size => "64", :images_dir => "#{gem_dir}/images/emoji/unicode")
    Emoji::Extractor.new(args[:size].to_i, args[:images_dir]).extract!
  end
end
