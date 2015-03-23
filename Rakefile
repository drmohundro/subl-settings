desc 'Install Sublime Text 3 settings'
task :install do
  Dir.glob('*').select { |f| File.directory? f }.each do |dir|
    link_user_preferences(dir)
  end
end

def link_user_preferences(dir)
  replace_all = false

  Dir.glob("#{dir}/*").each do |file|
    if File.exist?(File.join(root_path, file))
      if File.identical? file, File.join(root_path, file)
        puts "identical #{file}"
      elsif replace_all
        replace_file(file)
      else
        print "overwrite #{file}? [ynaq] "
        case $stdin.gets.chomp
        when 'a'
          replace_all = true
          replace_file(file)
        when 'y'
          replace_file(file)
        when 'q'
          exit
        else
          puts "skipping #{file}"
        end
      end
    else
      link_file(file)
    end
  end
end

def replace_file(file)
  FileUtils.rm_rf "#{root_path}/#{file}"
  link_file(file)
end

def link_file(file)
  puts "linking #{file}"

  link = "#{root_path}/#{file}"
  target = "#{Dir.pwd}/#{file}"

  ensure_link_directory_exists(link)

  if windows?
    link.gsub! '/', '\\'
    target.gsub! '/', '\\'

    system %(cmd /c mklink "#{link}" "#{target}")
  else
    system %(ln -s "#{target}" "#{link}")
  end
end

def ensure_link_directory_exists(link)
  link_dir = File.dirname(link)
  Dir.mkdir(link_dir) unless Dir.exist?(link_dir)
end

def windows?
  RUBY_PLATFORM =~ /(win32|mingw32)/
end

def root_path
  sublime_root =
    if windows?
      "#{ENV['AppData']}/Sublime Text 3"
    else
      File.expand_path('~/Library/Application Support/Sublime Text 3')
    end

  "#{sublime_root}/Packages"
end
