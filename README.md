```
mkdir -p ~/.local/etc/ca-certificates ~/.local/run
touch ~/.local/etc/sniproxy.conf

cat >> ~/.local/etc/sniproxy.conf <<EOF
listen 80 {
    proto http
    table http_hosts
}

listen 443 {
    proto tls
    table https_hosts
}

table https_hosts {
    myblog.localhost unix:/run/myblog.tls.sock
}

table http_hosts {
    myblog.localhost unix:/run/myblog.sock
}
EOF

docker run -p 443:443 -v ~/.local/etc/sniproxy.conf:/etc/sniproxy.conf -v ~/.local/run:/run ecommpro/sniproxy
```
