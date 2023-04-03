# openwrt_mwan3
mwan3 script for openwrt wan iface reconnecting

```
#!/bin/sh

set_ping_params() {
    IFACE=$1
    uci set mwan3.${IFACE}.track_method='ping'
    uci set mwan3.${IFACE}.check_quality='0'
    uci set mwan3.${IFACE}.reliability='1'
    uci set mwan3.${IFACE}.count='5'
    uci set mwan3.${IFACE}.size='56'
    uci set mwan3.${IFACE}.timeout='2'
    uci set mwan3.${IFACE}.interval='60'
    uci set mwan3.${IFACE}.failure_interval='30'
    uci set mwan3.${IFACE}.recovery_interval='10'
    uci set mwan3.${IFACE}.down='2'
    uci set mwan3.${IFACE}.up='1'
    uci del mwan3.${IFACE}.track_ip
    uci add_list mwan3.${IFACE}.track_ip='1.1.1.1'
    uci add_list mwan3.${IFACE}.track_ip='1.0.0.1'
    uci add_list mwan3.${IFACE}.track_ip='8.8.8.8'
    uci add_list mwan3.${IFACE}.track_ip='8.8.4.4'
    uci set mwan3.${IFACE}.keep_failure_interval='1'
}

for i in $(seq 101 107); do
    set_ping_params "wan$i"
done

for i in $(seq 111 116); do
    set_ping_params "wan$i"
done

uci commit mwan3
/etc/init.d/mwan3 reload
```
