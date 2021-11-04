# Messaging subsystem

Messaging subsystem is a configurable platphorm for distribution of text and
binary messages The subsystem should be client agnostic.

## Message routes

- Client-server-client
- Client-server-group
- Client-server (optional server archive of Client-client direct messaging e.g
WebRtc)

## Message types

- Text: String (markdown with preconfidured maximal length)
- Value: Any (for clients synchronisation e.g. cooperative editing)
- Code: String/Number (Preconfigured code of system standard content e.g.
emoticons etc.)
- Binary: Images, Videos, Audio

## Message fields

- Content: text/binary
- Type: Enum
- Created time: Timestamp
- Edited time: Timestamp
- Deleted time: Timestamp
- Deleted by: String
