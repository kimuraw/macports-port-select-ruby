macports-port-select-ruby
=========================

enable `port select` for ruby such as python and php.

concept
-------

* add suffix "1.8" to commands from port:ruby
* install commands from port:[rb|rb19]-* under libexec/ruby[1.8|1.9], like perl5 ports
* add port:ruby_select

file hierarchy
--------------

files from port:ruby and port:ruby19:

    ${prefix}/bin
        ruby1.8 - port:ruby
        erb1.8
        irb1.8
        rdoc1.8
        ri1.8
        testrb1.8
        ruby1.9 - port:ruby19
        erb1.9
        gem1.9
        irb1.9
        rake1.9
        rdoc1.9
        ri1.9
        testrb1.9
    ${prefix}/libexec/
        ruby1.8/ - commands from port:rb-* without suffix
        ruby1.9/ - commands from port:rb19-* without suffix
    ${prefix}/etc/select/ruby
        base   - port:ruby_select
        none   - port:ruby_select
        ruby18 - port:ruby
        ruby19 - port:ruby19

example of rb-nanoc3 and rb19-nanoc3:

    ${prefix}/bin
        nanoc3-1.8
        nanoc3-1.9
    ${prefix}/libexec/
        ruby1.8/nanoc3
        ruby1.9/nanoc3

Step 1: change install destination of commands from rb|rb19-port
----------------------------------------------------------------

avoid conflicts of commands from rb-* and rb19-*.

* modify PortGroup ruby
* modify port:ruby and port:ruby19
    * generate ${prefix}/libexec/ruby[1.8|1.9] as destroot.keepdirs
* modify port:rb-* and rb19-*
    * change target of reinplace


Step 2: introduce `port select ruby`
------------------------------------


* modify port:ruby
    * pass --program-suffix to configure
    * add PortGroup select
* modify port:ruby19
    * remove variant "nosuffix"
    * add PortGroup select
* add port:select

Note
----

port samples of each style:

* basic_install - rb-archive-tar-minitar
* copy_install - rb-cvs
* extconf.rb - rb-password
* gem - rb-nanoc3, rb19-nanoc3
* gnu - rb-pdumpfs
* install.rb - rb-htaccess?
* setup.rb - rb-hikidoc

see https://gist.github.com/kimuraw/5068437 for numbers of ruby.setup styles.


