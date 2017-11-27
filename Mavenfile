#-*- mode: ruby -*-

require File.dirname(__FILE__) + "/lib/readline/version.rb"

gemspec :jar => 'readline.jar'

parent 'org.jruby:jruby-parent', '9.1.14.0'

# do not push gem to rubygems.org
jruby_plugin! :gem do
  execute_goals :id => 'default-push', :skip => true
end

# we need the jruby API here, the version should be less important here
jar 'org.jruby:jruby:${project.parent.version}', :scope => :provided

jar 'jline:jline', Readline::Version::JLINE_VERSION

properties 'jruby.plugins.version' => '1.1.5'

distribution_management do
  snapshot_repository :id => 'sonatype-nexus-snapshots', :url => 'https://oss.sonatype.org/content/repositories/snapshots'
  repository :id => 'sonatype-nexus-staging', :url => 'https://oss.sonatype.org/service/local/staging/deploy/maven2/'
end

# NOTE there is remark about the `/Users/christian/projects/jruby/jruby-readline/pkg/jruby-readline-1.2.0-shaded.gem` which is actually the shaded jar !
plugin :shade, '3.1.0',
       relocations: [ { pattern: 'jline',
                        shadedPattern: 'org.jruby.readline.jline' },
                      { pattern: 'org.fusesource',
                        shadedPattern: 'org.jruby.readline.org.fusesource' } ] do
  execute_goal :shade, phase: 'prepare-package'
end

# do not do anything on deploy unless when using the release profile
plugin :deploy, '2.8.1' do
  execute_goals( :deploy, :skip => false )
end

profile :id => 'release' do
  activation do
    property :name => 'performRelease', :value => 'true'
  end
  build do
    default_goal :deploy
  end
  plugin :gpg, '1.5' do
    execute_goal 'sign', :id => 'sign-artifacts', :phase => 'verify'
  end
end

# vim: syntax=Ruby
