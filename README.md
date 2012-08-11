###Setup
####Step 1.

Use Ruby 1.9

####Step 2

Bundle

    bundle install

####Step 3

Build with bootstrap

    git clone git://github.com/faun/bootstrap.git $HOME/src/bootstrap
    rake

####Step 4

View it in a browser

    rackup
    open http://0.0.0.0:9292

####Optional

Watch for changes with guard

    guard

