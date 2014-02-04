# Creating a client
Create a Kronos client with the URL of a running server.  Optionally
provide a `namespace` to explicitly work with events in a particular
namespace.
```python
kc = KronosClient('http://localhost:8151', namespace='demo')
start = datetime.now(tz=tzutc())

```
## A nonblocking client
Pass a `blocking=False` to the KronosClient constructor for a client
that won't block on the Kronos server when you insert data.  A
background thread will batch up data and send it to the server.  This
is useful for logging/metrics collection on machines that can't wait
for a write acknowledgement.  An optional `sleep_block` argument,
defaulting to `0.1` specifies how many seconds to wait between batches
to the server.  If the process running the client crashes before
flushing events, those events will be lost.
```python
nonblocking = KronosClient('http://localhost:8151', namespace='demo',
                           blocking=False)

```
# Inserting data
Insert data with the `put` command.  The argument is a dictionary of
stream names (e.g., `yourproduct.website.pageviews`) to a list of
JSON-encodable dictionaries to insert to each stream.
```python
kc.put({'yourproduct.website.pageviews': [
         {'source': 'http://test.com',
          'browser': {'name': 'Firefox', 'version': 26},
          'pages': ['page1.html', 'page2.html']}],
        'yourproduct.website.clicks': [
         {'user': 40, 'num_clicks': 7},
         {'user': 42, 'num_clicks': 2}]})

```
