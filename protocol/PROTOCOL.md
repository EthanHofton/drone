## MQTT like lightweight pub/sub protocol

The idea for this is to mimic MQTT but over UDP for fast data transmission, to send telemetry data from the drone to the flight monitor UI

### Packet anatomy

Each packet consists of the header + payload:

#### The header

```
Offset  Size  Field          Notes
──────  ────  ─────────────  ─────────────────────────────────────
0       2     magic          0xAB 0xCD — discard if wrong
2       1     topic_id       filter here, drives payload parsing
3       4     timestamp_ms   ESP32 uptime — use this for graph X axis
7       2     payload_len    bytes of payload following header
9       4     sequence       per-topic counter, used only for drop detection
```

Total: 13 bytes

Immeatatly following the header is the playload, which can be variable configured by `payload_len` in the header.


#### Pub/Sub

To publish, simply send bytes. To sub, filter by topic_id
