### Barrel Development Best Practices

# HTTPS

## Minimizing Security Related Performance Issues

### Only use HTTPS where necessary
- At least 4 TCP roundtrips are required to open a single SSL connection between the client and server. This means that users connecting to pages that employ HTTPS will experience a more significant a delay before data is transmitted.
- Clients may want to use HTTPS on all pages to give users a sense of security. If this is the case explain the potential performance implications and encourage them to reserve HTTPS for pages that require data to transmitted securely.

### Measure the impact of your HTTPS "handshakes"
- WebPageTest is a great way to evaluate how much of a performance hit your page is taking due to HTTPS "handshakes".
- Purple bars in the waterfall indicate time spent in SSL negotiation.
- Look out for pages displaying frequent or abnormally long purple bars.

### Make sure keepalive is enabled
- Enabling the keepalive signal in Apache (or your preferred web server) is a great way to avoid having to reestablish a secure connection for each of the requests made by the client.
