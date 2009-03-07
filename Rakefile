require 'net/ftp'

desc 'Upload the site to the website.'
task :upload do
  puts ' - Uploading...'
  Net::FTP.open('eighteightohnine.com','mbleigh','hotmail') do |ftp|
    ftp.chdir('eighteightohnine/current')
    puts ftp.list('*')
    upload(ftp, '_site')
  end
end

desc 'Build the site.'
task :build do
  puts ' - Building...'
  puts `jekyll`
end

desc 'Build and upload the site to Dreamhost.'
task :deploy => [:build, :upload]

def upload(ftp, file, prefix = '')
  stats = File::Stat.new(file)
  if stats.directory?
    puts " - Changing to directory #{file}"
    Dir.chdir(file) do
      ftp.chdir(file) unless file == '_site'
      Dir.foreach('.') do |ifile|
        next if ifile.match(/\A\./)
        upload(ftp, ifile, "#{file}/")
      end
      ftp.chdir('..') unless file == '_site'
    end
  elsif ['jpg','png','gif'].include? File.extname(file)
    ftp.putbinaryfile(file)
    puts " - Uploaded #{file}"
  else
    ftp.puttextfile(file)
    puts " - Uploaded #{file}"
  end
end