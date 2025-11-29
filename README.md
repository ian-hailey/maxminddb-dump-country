# maxminddb-dump-country
Dump maxmind DB for linux iptables xt_geoip

The program is designed to convert the GeoLite2-Country.mmdb file into a dataset for xtables-addons/xt_geoip.

It is also possible to get files GeoLite2-Country-Blocks-IPv4.csv, GeoLite2-Country-Blocks-IPv6.csv, GeoLite2-Country-Locations-en.csv

Install packages:

    apt install xtables-addons-dkms geoip-database geoip-bin libmaxminddb0 libmaxminddb-dev mmdb-bin

Build Install:

    make
    cp xt_geoip_build_maxmind /usr/local/bin

# IPTables Geo Filter

Get an account with MaxMind from https://dev.maxmind.com/geoip/updating-databases put populate /etc/GeoIP.conf

    cp cron/geoip to /etc/cron.weekly

Update your /etc/iptables/rules.v4 as per the example to file on IP src, in the attached exampe filtering is performed on incomming IMAP requests.

    *filter
    -A INPUT -s 192.168.0.0/16 -p tcp --dport 993 -j ACCEPT
    -A INPUT -m geoip --src-cc GB -p tcp --dport 993 -j ACCEPT
    -A INPUT -m geoip --src-cc DE -p tcp --dport 993 -j ACCEPT
    -A INPUT -p tcp --dport 993 -j LOG --log-prefix "iptables-dropped-imap: "
    -A INPUT -p tcp --dport 993 -j DROP