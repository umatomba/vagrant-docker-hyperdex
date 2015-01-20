# -*- mode: ruby -*-
# vi: set ft=ruby : 

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

#cluster configuration
CLUSTER_SIZE = 3
START_CLUSTER_ID = 1
FROM_IP = "192.168.100.1"
ALL_NODES_IN_CLUSTER = ["192.168.100.1","192.168.100.2","192.168.100.3"]

#spetial NW settings
INTERFACE_PREFFIX = "eth"
START_INTERFACE_ID = 1
HOSTNAME_PREF = 'h'

def get_nodes (count, from_ip, hostname_pref)
    nodes = []
    ip_arr = from_ip.split('.')
    first_ip_part = "#{ip_arr[0]}.#{ip_arr[1]}.#{ip_arr[2]}"
    count.times do |i|
        hostname = "%s%01d" % [hostname_pref, (i+START_INTERFACE_ID)]
        nodes.push([i+START_CLUSTER_ID, hostname, "#{first_ip_part}.#{ip_arr.last.to_i+i}", "#{INTERFACE_PREFFIX}#{i+START_INTERFACE_ID}"])
    end
    nodes
end

Vagrant.configure("2") do |config|

    cluster_nodes = get_nodes(CLUSTER_SIZE, FROM_IP, HOSTNAME_PREF)

    cluster_nodes.each do |in_cluster_position, hostname, hostaddr, interface|
        config.vm.define "coordinator#{in_cluster_position}" do |coordinator|
            coordinator.vm.provider 'docker' do |d|
                d.image = "umatomba/docker-hyperdex:1.6"
                d.name   = "coordinator#{in_cluster_position}"
                d.create_args = ['-i', '-t', '--net=host']
                if in_cluster_position == 1
                    d.cmd = ['hyperdex', 'coordinator', '--foreground', "--listen=#{hostaddr}", "--data=/hyperdex/coord"]
                else
		    d.cmd    = ['hyperdex', 'coordinator', '--foreground', "--listen=#{hostaddr}" ,"--connect-string=#{ALL_NODES_IN_CLUSTER.join(':1982,')}", '--data=/hyperdex/coord']
                end
            end
        end

        config.vm.define "daemon#{in_cluster_position}" do |daemon|
            daemon.vm.provider 'docker' do |d|
                d.image = "umatomba/docker-hyperdex:1.6"
                d.name   = "daemon#{in_cluster_position}"
                d.create_args = ['-i', '-t', '--net=host']
                d.cmd    = ['hyperdex', 'daemon', '--foreground', "--listen=#{hostaddr}", "--coordinator=#{hostaddr}", '--data=/hyperdex/daemon']
            end
        end
    end
end
