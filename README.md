# WebRTC QR Flow

To establish the most basic WebRTC communication channel (the data channel, which is later
capable of bootstrapping a media channel), the two peers need to be able to exchange an
offer (from the first peer to the second peer) and an answer (from the second peer to the
first peer). They also both need a STUN service to work around potentially NAT'ed network.

Google provides a free STUN service and it is easy to spin up your own STUN server on a
cheap VPS. The STUN server is used for the IP resolution only and is stateless and does
not receive any confidential information (such as the SDP used to bootstrap the connection).

The STUN server and the mechanism used for SDP exchange (the "signaling") are the only two
pieces that prevent WebRTC from being truly P2P. Due to the above, STUN is less of a concern
as it can be easily replaced and does not host any confidential data.

Replacing the SDP exchange bit is more tricky. One P2P solution which I like is to use a
completely out of band, non-Internet channel for the connection establishment: QR codes.

SDP is a bit too bloated for QR, but it can be compressed and even reduced when building
with assumptions about the network, such as that the establishment will only ever be used
on a local network.

QR is built-in to all modern phones without the need for extra apps, which makes this very
convenient.

The SDP exchange flow using QR is as follows:

- The user loads the laptop peer application
- The laptop peer application generates and displays offer SDP in a QR code
- The user scans the offer SDP QR code and gets redirected to the mobile peer application
- The mobile peer application generates and displays answer SDP in a QR code
- The laptop peer is scanning for the user turning the phone around to show the QR code
- The laptop peer scans the answer SDP and completes the exchange
- A data channel opens between the two peers and the applications close the QR codes
- A full-duplex communication channel between the peers can be used for real-time sync now
