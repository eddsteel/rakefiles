require 'musicinfo'

SRC="../Music"
TRG="."

MP3=FileList["#{SRC}/**/*.mp3"]
FLC=FileList["#{SRC}/**/*.flac"]
OGG=FileList["#{SRC}/**/*.ogg"]
M4A=FileList["#{SRC}/**/*.m4a"]

TYPES=[:mp3s, :flacs, :oggs, :m4as]
TYPES.each do |t|
  task t
  task :default => t
end

(OGG + FLC + MP3 + M4A).each do |file|
  ext = file.pathmap "%{.,}x"
  outdir = file.pathmap("#{TRG}/#{artist file} - #{album file}/#{ext}").gsub("\000", " ")
  outfile = file.pathmap "#{outdir}/%f"

  directory outdir
  file outfile => [file, outdir] do
    cp file, outfile
  end

  FileList["#{file.pathmap("%d")}/*.jpg"].each do |cover|
    file cover => [file, outdir] do
      cp cover, outdir
    end
  end

  task :"#{ext}s" => outfile
end



task :default => [:flacs, :oggs, :mp3s, :m4as]


