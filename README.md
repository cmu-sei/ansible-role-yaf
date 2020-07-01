yaf
=========

A role for installing, configuring, and managing the YAF service.  YAF is Yet Another Flowmeter. It processes packet data from pcap dumpfiles as generated by tcpdump or via live capture from an interface using pcap into bidirectional flows, then exports those flows to IPFIX Collecting Processes or in an IPFIX-based file format. YAF's output can be used with the SiLK flow analysis tools, super_mediator, Pipeline, and any other IPFIX compliant toolchain. See yaf [documentation](https://tools.netsa.cert.org/yaf/docs.html) for more information.

Role Variables
--------------

Available variables are listed below, along with default values (see [defaults/main.yml](defaults/main.yml)):
    
    yaf_version

The version of yaf to install.  The master branch will always point to the latest available version.

    netsa_url: "http://tools.netsa.cert.org/releases/"
    yaf_name: "yaf-{{ yaf_version }}"
    yaf_tgz: "{{ yaf_name }}.tar.gz"
    yaf_url: "{{ netsa_url }}{{ yaf_tgz }}"
    yaf_timeout: 10
    yaf_checksums:
      '2.11.0': sha256:5e2523eeeaa5ac7e08f73b38c599f321ba93f239011efec9c39cfcbc30489dca
      '2.10.0': sha256:ed13a5d9f4cbbe6e82e2ee894cf3c324b2bb209df7eb95f2be10619bbf13d805    
    yaf_checksum: '{{ yaf_checksums[yaf_version] }}'

Helper variables used to download the yaf release from the [netsa tools site](https://tools/netsa.cert.org).

    yaf_myname: "yaf"

The name of the yaf process.

    yaf_conf_template: "yaf.conf.j2"
    yaf_conf_file_loc: "/usr/local/etc"
    yaf_conf_file_path: "{{ yaf_conf_file_loc }}/{{ yaf_myname }}.conf"
    yaf_init_template: "yaf.j2"
    yaf_init_file_path: "/etc/init.d/{{ yaf_myname }}"

Template sources to use and their destinations.

    yaf_prefix: "/usr/local"

Folder prefix for autoconf stuff.

    yaf_service: False

Whether to start the yaf service.

| Variable  | Explanation |
| ------------- | ------------- |
| yaf_cap_type: "pcap" | Live capture type. Must be pcap, or dag for Endace DAG if YAF was built with libdag, napatech if YAF was built with libnapatech, or netronome with Netronome support |
| yaf_cap_if: "eth0" | Live capture interface name. |
| yaf_ipfix_proto: "tcp" | IPFIX transport protocol to use for export. Must be one of tcp or udp, or sctp if fixbuf was built with SCTP support or spread if fixbuf was built with Spread support.  If using spread, --groups must be added to extra flags |
| yaf_ipfix_host: "localhost" | Hostname or IP address of IPFIX collector to export flows to. |
| yaf_ipfix_port: "" | If present, connect to the IPFIX collector on the specified port. Defaults to port 4739, the IANA-assigned port for IPFIX |
| yaf_rotate_location: "" | If present, and YAF_IPFIX_PROTO is not present, write IPFIX files to the given file directory |
| yaf_rotate_time: 120 | Rotate time. If present, and YAF_ROTATE_LOCATION is present, rotate files every YAF_ROTATE_TIME seconds. |
| yaf_statedir: "/var/log/yaf" | Path to state location directory; contains the log and pidfiles unless modified by the following configuration parameters. |
| yaf_pidfile: "{{ yaf_statedir }}/yaf.pid" | Path to PID file for YAF |
| yaf_log_folder: "{{ yaf_statedir }}/log" | Folder to create to hold yaf logs. |
| yaf_log: "{{ yaf_log_folder }}/yaf.log" | File or syslog facility name for YAF logging. If file, must be an absolute path to a logfile. Directory must exist. |
| yaf_user: "" | If present, become the specified user after starting YAF |
| yaf_extraflags: "--silk --ip4-only" | Additional flags to pass to the YAF process. Use --silk --ip4-only for export to SiLK v2 rwflowpack or SiLK v2 flowcap. |

Dependencies
------------

- cmusei.silk

Example Playbook
----------------

    - hosts: servers
      vars:
        yaf_service: True
      roles:
         - role: cmusei.yaf
           tags: ['yaf']

License
-------

Copyright 2020 Carnegie Mellon University.
NO WARRANTY. THIS CARNEGIE MELLON UNIVERSITY AND SOFTWARE ENGINEERING INSTITUTE MATERIAL IS FURNISHED ON AN "AS-IS" BASIS. CARNEGIE MELLON UNIVERSITY MAKES NO WARRANTIES OF ANY KIND, EITHER EXPRESSED OR IMPLIED, AS TO ANY MATTER INCLUDING, BUT NOT LIMITED TO, WARRANTY OF FITNESS FOR PURPOSE OR MERCHANTABILITY, EXCLUSIVITY, OR RESULTS OBTAINED FROM USE OF THE MATERIAL. CARNEGIE MELLON UNIVERSITY DOES NOT MAKE ANY WARRANTY OF ANY KIND WITH RESPECT TO FREEDOM FROM PATENT, TRADEMARK, OR COPYRIGHT INFRINGEMENT.
Released under a MIT (SEI)-style license, please see license.txt or contact permission@sei.cmu.edu for full terms.
[DISTRIBUTION STATEMENT A] This material has been approved for public release and unlimited distribution.  Please see Copyright notice for non-US Government use and distribution.
CERT® is registered in the U.S. Patent and Trademark Office by Carnegie Mellon University.
This Software includes and/or makes use of the following Third-Party Software subject to its own license:
1. ansible (https://github.com/ansible/ansible/tree/devel/licenses) Copyright 2019 Red Hat, Inc.
2. molecule (https://github.com/ansible-community/molecule/blob/master/LICENSE) Copyright 2018 Red Hat, Inc.
3. testinfra (https://github.com/philpep/testinfra/blob/master/LICENSE) Copyright 2020 Philippe Pepiot.

DM20-0509


Author Information
------------------

This role was created in 2020 by [Matt Heckathorn](https://resources.sei.cmu.edu/library/author.cfm?authorID=2403).
