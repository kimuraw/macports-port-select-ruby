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
* add port:ruby_select

ports might be affected from this changes
-----------------------------------------

uses ${prefix}/bin/ruby:

    % pwd
    /opt/local/var/macports/sources/rsync.macports.org/release/tarballs/ports
    % ack -a -G Portfile 'bin\/ruby' --ignore-dir=ruby
    databases/pqa/Portfile
    23:	reinplace "s|/usr/local/bin/ruby|/usr/bin/env ruby|g" \
    
    devel/subversion-rubybindings/Portfile
    40:configure.env		RUBY=${prefix}/bin/ruby
    
    editors/MacVim/Portfile
    162:    configure.args-append   --with-ruby-command=${prefix}/bin/ruby
    167:    configure.args-append   --with-ruby-command=${prefix}/bin/ruby1.9
    
    editors/vim/Portfile
    968:    configure.args-append   --with-ruby-command=${prefix}/bin/ruby
    973:    configure.args-append   --with-ruby-command=${prefix}/bin/ruby1.9
    
    editors/vim-app/Portfile
    987:    configure.args-append   --with-ruby-command=${prefix}/bin/ruby
    992:    configure.args-append   --with-ruby-command=${prefix}/bin/ruby1.9
    
    sysutils/facter/Portfile
    36:destroot.cmd        ${prefix}/bin/ruby ${worksrcpath}/install.rb \
    
    sysutils/puppet/Portfile
    35:destroot.cmd        ${prefix}/bin/ruby ${worksrcpath}/install.rb \

these ports need to change ${prefix}/bin/ruby -> ${ruby.bin} or ${prefix}/bin/ruby1.8.

depends_lib for port:ruby*:

    % ack -a -G Portfile -l 'port:ruby' --ignore-dir=ruby | cut -d/ -f1-2
    audio/liblastfm
    audio/xmms2
    databases/pqa
    devel/subversion-rubybindings
    devel/swig
    devel/thrift
    devel/thrift-devel
    devel/xapian-bindings
    editors/MacVim
    editors/vim
    editors/vim-app
    graphics/graphviz
    graphics/graphviz-devel
    irc/weechat
    kde/koffice
    kde/qtruby
    multimedia/mkvtoolnix
    print/pdflib
    science/root
    security/metasploit3
    sysutils/facter
    sysutils/puppet
    textproc/ohcount
    www/elinks-devel
    www/eruby
    www/mod_ruby
    www/redland-bindings

* linking libruby: not affected.
* auto detect `ruby': might specify path to ${ruby.bin}.

Note
----

added options:

* ruby.branch - decide ${ruby.bin} without "ruby.setup". "1.8" or "1.9".
* ruby.link_binaries - whether generate suffixed symlink under ${prefix}/bin or not. "yes" (default) or "no".

port samples of each style:

* basic_install - rb-archive-tar-minitar
* copy_install - rb-cvs
* extconf.rb - rb-password
* gem - rb-nanoc3, rb19-nanoc3
* gnu - rb-pdumpfs
* install.rb - rb-htaccess?
* setup.rb - rb-hikidoc

see https://gist.github.com/kimuraw/5068437 for numbers of ruby.setup styles.


