macports-port-select-ruby
=========================

enable `port select` for ruby such as python and php.

concept
-------

* add suffix "1.8" to commands from port:ruby
* install commands from port:[rb|rb19]-* under libexec/ruby-[1.8|1.9] (like perl5 ports)

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
        ruby-1.8/ - commands from port:rb-* without suffix
        ruby-1.9/ - commands from port:rb19-* without suffix
    ${prefix}/etc/select/ruby
        base
        none
        ruby18
        ruby19

example of rb-nanoc3 and rb19-nanoc3:

    ${prefix}/bin
        nanoc3-1.8
        nanoc3-1.9
    ${prefix}/libexec/
        ruby-1.8/nanoc3
        ruby-1.9/nanoc3

Step 1: change install destination of commands from rb|rb19-port
----------------------------------------------------------------

avoid conflicts of commands from rb-* and rb19-*.

* modify PortGroup ruby
* modify port:ruby and port:ruby19
    * generate ${prefix}/libexec/ruby-[1.8|1.9] as destroot.keepdirs


Step 2: introduce `port select ruby`
------------------------------------


* modify port:ruby
    * pass --program-suffix to configure
    * add PortGroup select
* modify port:ruby19
    * remove variant "nosuffix"
    * add PortGroup select

