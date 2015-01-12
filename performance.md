### Barrel Development Best Practices

# Performance
 
## Everything

*Please elaborate!*

- Minification/Concat JS/CSS
- Minimize requests (but not to point of squeezing requests into too large of chunks)
- Server-side caching
    - Turn it on!
	- DB Caching
	- Page
	- Object/Model (memcache/redis)
- Client-side caching
    - Browser
	- Expires
	- CDN Libraries
- CDN Domain Sharding
- Social Libraries are slow
    - 3rd Party tracking as well
- Preloading images (actual pre-loading)
- Image minification/compression
    - Control upload size (ideal?)
	- Communicate and leverage the best way
- Optimize backend code
- CSS Hardware Acceleration (opacity warning)
- Server Config
    - gzip
	- keep alive (connections)
- Font caching, # of fonts, CDN Fonts
- Use of SSL should be limited
- Identify when ajax can happen
- Image sprites (load less)
- Queued Jobs
    - Email
	- Curl
- Non-blocking code
- Picking the right libraries when less is best
- NGROK
    -proxy to local