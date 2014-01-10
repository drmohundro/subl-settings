desc "Install Sublime Text 3 settings"
task :install do
  link_subl_settings
end

def link_subl_settings
  link = "#{root_path}"
  target = "#{Dir.pwd}"

  puts "linking #{file} to #{target}"

  if is_windows?
    system %Q{cmd /c mklink /D "#{link}" "#{target}"}
  else
    system %Q{ln -s "#{target}" "#{link}"}
  end
end

def is_windows?
  RUBY_PLATFORM =~ /(win32|mingw32)/
end

def root_path
  sublime_root = is_windows? ?
    "#{ENV['AppData']}/Sublime Text 3" :
    File.expand_path('~/Library/Application Support/Sublime Text 3')

  "#{sublime_root}/Packages/User"
end
