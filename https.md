### Barrel Development Best Practices

# HTTPS

## Minimizing Security Related Performance Issues

### Use HTTPS always
- We recommend only using servers with HTTP2 enabled or those that have HTTP2 compatibility. 
- Google gives secure sites ranking boosts and similarly Chrome flags HTTP sites as `unsecure` in the browser bar.

## Measure the impact of your HTTPS "handshakes"
### When HTTP/2 Cannot Be Implemented
- WebPageTest is a great way to evaluate how much of a performance hit your page is taking due to HTTPS "handshakes".
- Purple bars in the waterfall indicate time spent in SSL negotiation.
- Look out for pages displaying frequent or abnormally long purple bars.
