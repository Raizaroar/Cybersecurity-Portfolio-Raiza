```zeek
@load base/frameworks/notice
@load base/protocols/conn

module ScanDetector;

export {
    redef enum Notice::Type += {
        Scan_Detected,
        Slow_Scan_Detected,
        Distributed_Scan_Detected
    };
    
    # Thresholds
    const scan_threshold = 50 &redef;
    const slow_scan_window = 10min &redef;
    const ports_threshold = 20 &redef;
}

# Track connection attempts per source IP
global scan_attempts: table[addr] of set[port] &create_expire=5min;
global slow_scan_attempts: table[addr] of set[port] &create_expire=10min;

# Detect rapid scanning
event connection_state_remove(c: connection) {
    if (c$id$resp_p !in scan_attempts[c$id$orig_h]) {
        add scan_attempts[c$id$orig_h][c$id$resp_p];
        add slow_scan_attempts[c$id$orig_h][c$id$resp_p];
    }
    
    # Check for rapid scan
    if (|scan_attempts[c$id$orig_h]| > scan_threshold) {
        NOTICE([$note=Scan_Detected,
                $msg=fmt("Port scan detected from %s (%d ports in 5 min)", 
                         c$id$orig_h, |scan_attempts[c$id$orig_h]|),
                $src=c$id$orig_h,
                $identifier=cat(c$id$orig_h)]);
    }
    
    # Check for slow scan
    if (|slow_scan_attempts[c$id$orig_h]| > ports_threshold) {
        NOTICE([$note=Slow_Scan_Detected,
                $msg=fmt("Slow scan detected from %s (%d ports in 10 min)", 
                         c$id$orig_h, |slow_scan_attempts[c$id$orig_h]|),
                $src=c$id$orig_h,
                $identifier=cat(c$id$orig_h)]);
    }
}
```

## Script explanation:

**Line 1-2:** Load required Zeek frameworks

base/frameworks/notice: Alert system

base/protocols/conn: Connection tracking

**Line 8-12:** Define notice types (alert categories)

**Line 15-17:** Thresholds

- scan_threshold = 50: Alert if 50+ ports contacted
- slow_scan_window = 10min: Extended window for slow scans
- &redef: Can be overridden in config

**Line 21-22:** Data structures

- table[addr] of set[port]: For each IP, track unique ports contacted
- &create_expire=5min: Auto-delete entries older than 5 min (memory management)

**Line 25:** connection_state_remove event

- Triggers when connection closes (not when it starts)

***Why?*** Ensures we only count completed/attempted connections

**Line 26-29:** Add port to tracking sets

**Line 32-37:** Rapid scan detection

|scan_attempts[c$id$orig_h]|: Count of unique ports

If >50 ports in 5 min → ALERT

**Line 40-46:** Slow scan detection

Same logic but 10min window

Threshold only 20 (because it's over longer period)