desc "Install Sublime Text 2 settings"
task :install do
  replace_all = false
  Dir['*'].each do |file|
    next if %w[Rakefile README.md LICENSE].include? file

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

  if is_windows?
    mklink_opts = File.directory?(target) ? "/D" : ""

    link.gsub! '/', "\\"
    target.gsub! '/', "\\"

    system %Q{cmd /c mklink #{mklink_opts} "#{link}" "#{target}"}
  else
    system %Q{ln -s "#{target}" "#{link}"}
  end
end

def is_windows?
  RUBY_PLATFORM =~ /(win32|mingw32)/
end

def root_path
  # TODO: MO> get the path for windows...
  sublime_settings = is_windows? ?
    "#{root_path}/AppData/Sublime Text 2" :
    File.expand_path('~/Library/Application Support/Sublime Text 2')

  "#{sublime_settings}/Packages/User"
end