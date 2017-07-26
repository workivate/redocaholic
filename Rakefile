require 'erb'
require 'json'
require 'yaml'

apis = YAML.load(File.open('./data/apis.yml'))

task default: [:clone, :install_redoc, :build]

task :serve do
  port = (ENV['PORT'] || 8000)
  sh "ruby -run -ehttpd public/ -p#{port}"
end

task :clean do
  apis.each do |nickname, api|
    sh "rm -rf ./tmp/#{nickname}"
    sh "rm -rf ./public/specs/#{nickname}"
  end
end

task clone: [:clean] do
  apis.each do |nickname, api|
    raise "INVALID YAML!" unless [
      api['name'],
      api['repo'],
      api['branch'],
      api['spec_dir'],
      api['entry_file']
    ].all?

    tmp_dir = "./tmp/#{nickname}"
    tmp_spec_dir = "#{tmp_dir}/#{api['spec_dir']}"
    public_spec_dir = "./public/specs/#{nickname}"

    raise "INVALID PROPERTIES!" unless [tmp_dir, tmp_spec_dir, public_spec_dir].all?

    sh "git clone #{api['repo']} ./tmp/#{nickname} -b #{api['branch']} --single-branch --depth=1"
    sh "cp -R #{tmp_spec_dir} #{public_spec_dir}"
  end
end

task :install_redoc do
  # We put this module in the public dir so it can be loaded
  # It was either this or rely on their CDN. Phil
  sh "npm install redoc --save --prefix public"
end

task :build do
  @apis = apis

  renderer = ERB.new(File.read('./template/index.html'))
  File.write("./public/index.html", renderer.result)

  apis.each do |nickname, api|
    @name = api['name']
    @spec = "/specs/#{nickname}/#{api['entry_file']}"

    renderer = ERB.new(File.read('./template/api.html'))
    File.write("./public/#{nickname}.html", renderer.result)
  end

end
