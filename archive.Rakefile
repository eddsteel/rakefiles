# Archives the contents of leaf directories.
#
# DOCUMENT!

DIRS=FileList.new
`find . -links 2`.each do |d|
  DIRS << d.chop
end

DIRS.each do |dir|
  zip = dir.pathmap('%d%s%f.zip')
  tbz = dir.pathmap('%d%s%f.tar.bz2')

	if dir.pathmap('%f').length < 5
		zip = zip.sub(%r|^(.*)/([^/]+)$|, '\1.\2')
		tbz = tbz.sub(/^(.*)\/([^\/]+)$/, '\1.\2')
	end

  files = FileList["#{dir}/*"] - FileList["*.bz2"] - FileList["*.zip"]
  unless files.empty?
    file zip => files do
      sh %Q|zip -q "#{zip}" #{files.collect{|f| %Q~"#{f}"~}.to_s}|
    end
    file tbz => files do
      sh %Q|tar jcf "#{tbz}" #{files.collect{|f| %Q~"#{f}"~}.to_s}|
    end
    task :default => zip
    task :default => tbz
  end
end
