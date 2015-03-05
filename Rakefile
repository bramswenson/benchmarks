require 'benchmark'
require 'benchmark/ips'
require 'ffaker'
require 'erb'
require 'yajl'
require 'oj'

desc 'benchmark erb vs json'
task :erb_vs_json do

  Benchmark.ips do |b|

    @users = []
    1000.times { |i| @users << { id: i, name: ::Faker::Lorem.word, email: ::Faker::Internet.free_email } }

    erb_template = ERB.new(<<-EOT)
<% @users.each do |user| %>
  <h1><%= user[:id] %> - <%= user[:name] %><h1>
  <h2><%= user[:email] %><h2>
<% end %>
EOT

    b.config(warmup: 2, time: 8)

    b.report('erb') do
      erb_template.result
    end

    b.report('yajl') do
      Yajl::Encoder.encode(@users)
    end

    b.report('oj') do
      Oj.dump(@users)
    end

    b.compare!
  end

end
