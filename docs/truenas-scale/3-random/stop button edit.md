# Change Stop button to Edit

Easy 1-liner. Run as root:

```bash
sed -i.bak 's/stop(vn.name)/edit(vn.name)/g; s/(2,2,\"Stop\")/(2,2,\"Edit\")/g'  /usr/share/truenas/webui/*.js
```
