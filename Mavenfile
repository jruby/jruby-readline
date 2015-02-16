#-*- mode: ruby -*-

gemspec :include_jars => true, :jar => 'readline.jar'

if model.version.to_s.match /[a-zA-Z]/
  # deploy snapshot versions on sonatype !!!
  model.version = model.version + '-SNAPSHOT'
end

parent 'org.jruby:jruby-parent', '9.0.0.0.pre1'

jruby_plugin! :gem do
  execute_goals :id => 'default-push', :skip => true
end

# we need the jruby API here, the version should be less important here
jar 'org.jruby:jruby:${project.parent.version}', :scope => :provided

properties( 'jruby.plugins.version' => '1.0.8',
            'tesla.dump.pom' => 'pom.xml',
            'tesla.dump.readonly' => true )

distribution_management do
  snapshot_repository :id => 'sonatype-nexus-snapshots', :url => 'https://oss.sonatype.org/content/repositories/snapshots'
  repository :id => 'sonatype-nexus-staging', :url => 'https://oss.sonatype.org/service/local/staging/deploy/maven2/'
end

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
