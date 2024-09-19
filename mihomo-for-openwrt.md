# Prerequisites

* OpenWrt/ImmortalWrt 23.05+
* Assume your routing system is arm64 architecture

**NOTE** Unless otherwise specified, all the following commands are executed in the router terminal.

**Update: 2024-07-20** Packaged: https://github.com/douglarek/mihomo-openwrt

## Install deps

        opkg update
        opkg install kmod-tun kmod-inet-diag curl

# Prepare

1. Obtain mihomo release

        curl -L https://github.com/MetaCubeX/mihomo/releases/download/v1.18.8/mihomo-linux-amd64-v1.18.8.gz -o mihomo-linux-amd64-v1.18.8.gz
        gzip -d mihomo-linux-amd64-v1.18.8.gz
        chmod +x mihomo-linux-amd64-v1.18.8
        mv mihomo-linux-amd64-v1.18.8 /usr/bin/mihomo

2. Create the `/etc/config/mihomo` file and populate it

        config mihomo 'main'
                option enabled '1'
                option user 'root'
                option conffile '/etc/mihomo/config.yaml'
                option workdir '/etc/mihomo'
                list ifaces 'wan'
                option log_stderr '1'
                option log_stdout '1'

3. Create the `/etc/init.d/mihomo` file and populate it

        #!/bin/sh /etc/rc.common

        USE_PROCD=1
        START=99

        script=$(readlink "$initscript")
        NAME="$(basename ${script:-$initscript})"
        PROG="/usr/bin/mihomo"

        start_service() {
                config_load "$NAME"

                local enabled user group conffile workdir ifaces
                local log_stdout log_stderr
                config_get_bool enabled "main" "enabled" "0"
                [ "$enabled" -eq "1" ] || return 0

                config_get user "main" "user" "root"
                config_get conffile "main" "conffile"
                config_get ifaces "main" "ifaces"
                config_get workdir "main" "workdir" "/etc/mihomo"
                config_get_bool log_stdout "main" "log_stdout" "1"
                config_get_bool log_stderr "main" "log_stderr" "1"

                mkdir -p "$workdir"
                local group="$(id -ng $user)"
                chown $user:$group "$workdir"

                procd_open_instance "$NAME.main"
                procd_set_param command "$PROG" -f "$conffile" -d "$workdir"

                # Use root user if you want to use the TUN mode.
                procd_set_param user "$user"
                procd_set_param file "$conffile"
                [ -z "$ifaces" ] || procd_set_param netdev $ifaces
                procd_set_param stdout "$log_stdout"
                procd_set_param stderr "$log_stderr"
                procd_set_param respawn

                procd_close_instance
        }

        service_triggers() {
                local ifaces
                config_load "$NAME"
                config_get ifaces "main" "ifaces"
                procd_open_trigger
                for iface in $ifaces; do
                        procd_add_interface_trigger "interface.*.up" $iface /etc/init.d/$NAME restart
                done
                procd_close_trigger
                procd_add_reload_trigger "$NAME"
        }
      Grant executable permissions:
  
        chmod +x /etc/init.d/mihomo

4. Create the `/etc/mihomo/config.yaml` file and populate it

        # Omitting any other unimportant configurations, I believe you will definitely

        tun:
          enable: true
          stack: mixed
          dns-hijack:
            - "any:53"
          auto-route: true
          auto-redirect: true # Key configuration, the above is actually all nonsense.
          auto-detect-interface: true

        dns:
          enable: true
          nameserver: [ 223.5.5.5 ]
          nameserver-policy:
            'geosite:geolocation-!cn,tiktok': tls://8.8.4.4
            'sgshort.wechat.com': tls://8.8.4.4


        # ......


# Launch

1. Start the service

        service mihomo start

2. View logs

        logread -l 100 -f | grep mihomo

3. About the ui download

      Assume your UI configuration is as follows:


        external-controller: 0.0.0.0:9090 # If you can't handle it, it's not recommended to use 0.0.0.0 binding. for example, use your router address
        external-ui: ui
        external-ui-url: "https://mirror.ghproxy.com/https://github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"
        secret: "123456" # change to your own


      After starting the mihomo service, use the API to download UI static files:

        curl -H 'Authorization: Bearer 123456' http://127.0.0.1:9090/upgrade/ui -X POST

      If there are no errors, it will return a successful download status:

        {"status":"ok"}

      Where did the UI download to? Take this article as an example:

        /etc/mihomo/ui/


      Finally, use the router address to access the UI:

        http://[your-router-address]:9090/ui

