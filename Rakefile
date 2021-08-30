<<<<<<< Updated upstream
require "tmpdir"

source_branch = "master"
production_branch = "gh-pages"

desc "Delete _site/"
task :delete do
  puts "\## Deleting _site/"
  status = system("rm -r _site")
  puts status ? "Success" : "Failed"
end

desc "Commit _site/"
task :commit, :message do |t, arg|
  puts "\n## Building _site files"
  status = system("jekyll build")
  puts status ? "Success" : "Failed"

  puts "\n## Staging modified files"
  status = system("git add -A :/")
  puts status ? "Success" : "Failed"
  puts "\n## Committing site build at #{Time.now.utc}"
  message = "Build site at #{Time.now.utc}"
  status = system("git commit -m \"#{arg[:message]||"Update"}. #{message}\"")
  puts status ? "Success" : "Failed"
  puts "\n## Pushing commits to remote #{source_branch}"
  status = system("git push origin #{source_branch}")
  puts status ? "Success" : "Failed"
end

desc "Deploy _site/ to #{production_branch} branch"
task :deploy do
  Dir.mktmpdir do |tmp|
    puts "\n## Moving #{source_branch} branch _site contents to tmp folder"
    status = system("mv _site/* #{tmp}")
    puts status ? "Success" : "Failed"
    puts "\n## Switching to #{production_branch} branch"
    status = system("git checkout #{production_branch}")
    puts status ? "Success" : "Failed"

    puts "\n## Pulling most recent #{production_branch} branch from remote"
    status = system("git pull")
    puts status ? "Success" : "Failed"
    puts "\n## Removing #{production_branch} branch contents"
    status = system("rm -rf *")
    puts status ? "Success" : "Failed"

    puts "\n## Moving contents in tmp folder to #{production_branch} branch"
    status = system("mv #{tmp}/* .")
    puts status ? "Success" : "Failed"
  end
  puts "\n## Adding #{production_branch} branch changes"
  status = system("git add -A")
  puts status ? "Success" : "Failed"
  puts "\n## Committing production site at #{Time.now.utc}"
  message = "Build production site at #{Time.now.utc}"
  status = system("git commit -m \"#{message}\"")
  puts status ? "Success" : "Failed"
  puts "\n## Pushing commits to remote #{production_branch}"
  status = system("git push origin #{production_branch}")
  puts status ? "Success" : "Failed"

  puts "\n## Switching back to #{source_branch} branch"
  status = system("git checkout #{source_branch}")
  puts status ? "Success" : "Failed"
end

desc "Commit and deploy _site/"
task :default => [:commit, :deploy] do
end
=======
require "rubygems"
require "tmpdir"

require "bundler/setup"
require "jekyll"


# Change your GitHub reponame
GITHUB_REPONAME = "tadeleTuli/tadeletuli.github.io"


desc "Generate blog files"
task :generate do
  Jekyll::Site.new(Jekyll.configuration({
    "source"      => ".",
    "destination" => "_site"
  })).process
end


desc "Generate and publish blog to gh-pages"
task :publish => [:generate] do
  Dir.mktmpdir do |tmp|
    cp_r "_site/.", tmp

    pwd = Dir.pwd
    Dir.chdir tmp

    system "git init"
    system "git add ."
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m #{message.inspect}"
    system "git remote add origin git@github.com:#{GITHUB_REPONAME}.git"
    system "git push origin master --force"

    Dir.chdir pwd
  end
end
>>>>>>> Stashed changes
