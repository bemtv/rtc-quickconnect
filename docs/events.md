## Events

The following events are emitted from the signalling object created by calling `quickconnect()`:

### Call Level Events

A "call" in quickconnect is equivalent to an established `RTCPeerConnection` between this quickconnect instance a remote peer.

- `call:started => function(id, peerconnection, data)`

  Triggered once a peer connection has been established been established between this quickconnect instance and another.

- `call:ended => function(id)`

  Triggered when a peer connection has been closed.  This may be due to the peer connection itself indicating that it has been closed, or we may have lost connection with the remote signaller and the connection has timed out.

### Data Channel Level Events

- `channel:opened => function(id, datachannel, data)`

  The `channel:opened` event is triggered whenever an `RTCDataChannel` has been opened (it's ready to send data) to a remote peer.

- `channel:opened:%label% => function(id, datachannel, data)`

  This is equivalent of the `channel:opened` event, but only triggered for a channel with label `%label%`.  For example:

  ```js
  quickconnect('http://rtc.io/switchboard', { room: 'test' })
    .createDataChannel('foo')
    .createDataChannel('bar')
    .on('channel:opened:foo', function(id, dc) {
      console.log('channel foo opened for peer: ' + id);
    });
  ```

  In the case above the console message would only be displayed for the `foo` channel once open, and when the `bar` channel is opened no handler would be invoked.

- `channel:closed => function(id, datachannel, label)`

  Emitted when the channel has been closed, works when a connection has been closed or the channel itself has been closed.

- `channel:closed:%label% => function(id, datachannel, label)`

  The label specific equivalent of `channel:closed`.

### Stream Level Events

- `stream:added => function(id, stream, data)`

  The `stream:added` event is triggered when an `RTCPeerConnection` has successfully been established to another peer that contains remote streams.  Additionally, if you are using quickconnect in it's "reactive" mode then you will also receive `stream:added` events as streams are dynamically added to the connection by the remote peer.

- `stream:removed => function(id)`

  As per the `stream:added` event but triggered when a stream has been removed.
