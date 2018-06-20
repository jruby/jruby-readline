#-*- mode: ruby -*-

gemspec :jar => 'readline.jar', :include_jars => true

jruby_version = '9.1.16.0'

# only copy readline's dependencies (jline jar) :
plugin! :dependency do
  execute_goal 'copy-dependencies',
               :outputDirectory => '${basedir}/lib',
               :useRepositoryLayout => true,
               :includeGroupIds => 'jline'
end

# do not push gem to rubygems.org
jruby_plugin! :gem do
  execute_goals :id => 'default-push', :skip => true
end

# we need the JRuby API, the version is less important here
jar 'org.jruby:jruby-core', jruby_version, :scope => :provided

properties( 'jruby.plugins.version' => '1.0.10',
            'jruby.version' => jruby_version,
            'polyglot.dump.pom' => 'pom.xml',
            'polyglot.dump.readonly' => true,
            'tesla.dump.pom' => 'pom.xml',
            'tesla.dump.readonly' => true )

distribution_management do
  snapshot_repository :id => :ossrh, :url => 'https://oss.sonatype.org/content/repositories/snapshots'
  repository :id => :ossrh, :url => 'https://oss.sonatype.org/service/local/staging/deploy/maven2/'
end

plugin :compiler, '3.1', :source => '1.6', :target => '1.6',
       :encoding => 'UTF-8', :debug => true,
       :showWarnings => true, :showDeprecation => true

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
