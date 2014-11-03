desc 'Install all'
task :install do
  Dir.chdir File.dirname(__FILE__) do
    current_dir = Dir.pwd.sub(ENV['HOME'] + '/', '')
    install_executables(current_dir)
  end

  Dir.chdir "dotfiles" do
    current_dir = Dir.pwd
    install_dotfiles(current_dir)
    setup_git(current_dir)
  end

  puts_y "Run \e[1;33msource ~/.bash_profile && source ~/.osx\e[0;36;32m to finish installation."
end

desc 'Install Sublime Text 3 settings'
task :sublime_text3 do
  current_dir = Dir.pwd.sub(ENV['HOME'] + '/', '')

  root_path   = File.expand_path('~/Library/Application Support/Sublime Text 3')
  source_path = File.join(ENV['HOME'], current_dir, 'sublime_text3', 'Preferences.sublime-settings')
  target_path = File.join(root_path, 'Packages/User/Preferences.sublime-settings')

  if File.exists?(target_path)
    system %[unlink \"#{target_path}\"]
  end

  Dir.chdir File.dirname(__FILE__) do
    system %[ln -vsf #{source_path} \"#{target_path}\"]
  end
end

def install_executables(dir)
  puts_c "Installing executables"
  Dir['bin/*'].each do |file|
    target_file = "/usr/#{file}"
    if File.exists?(target_file)
      system %[unlink #{target_file}]
    end
    system %[ln -vs #{File.join(ENV['HOME'], dir, file)} /usr/bin && chmod 755 #{target_file}]
  end
end

def install_dotfiles(dir)
  puts_c "Installing dotfiles"
  Dir['*'].each do |file|
    next if %w(gitconfig).include?(file)

    source_file = File.join(dir, file)

    target_file = File.join(ENV['HOME'], ".#{file}")
    if File.exists?(target_file)
      system %[unlink #{target_file}]
    end

    system %[ln -vs #{source_file} #{target_file}]
  end
  unless File.exist?(File.join(ENV['HOME'], ".bash_extended"))
    File.open(File.join(ENV['HOME'], ".bash_extended"), "w+") { |file| file.write("# Add bash extensions here") }
  end
end

def setup_git(dir)
  puts_c "Configuring git"
  gitconfig_file = File.join(dir, "gitconfig")
  system %[unlink $HOME/.gitconfig && cp -v #{gitconfig_file} $HOME/.gitconfig]
  system %[git config --global core.excludesfile ~/.global_ignore]
end

# Colored output :3
#
def puts_c(string)
  puts "\n\e[0;36;49m#{string}\e[0;0m"
end

def puts_y(string)
  puts "\n\e[0;36;32m#{string}\e[0;0m"
end